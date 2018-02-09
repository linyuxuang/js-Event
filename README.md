# js-Event
js_事件


   事件对象
      
		   * 在触发DOM上的某个事件时，会产生一个事件对象  event, 这个对象中包含着
		    所有与事件有关的信息，包括导致事件的元素，事件的类型以及其他与特定事件相关的信息，
        
		     例如：鼠标操作导致的事件对象中，会包含鼠标位置信息，
		              而键盘操作导致的事件中，会包含与按下的键有关的信息，
		              所有浏览器都支持event对象，但支持方式不同

 

     例子
      
                
                 btn.onclick=function(){
                  alert(event.type)    //click
                 }

    *注意  在函数内部 this值等于事件的目标元素


            <input type="button" name="" id="btn1" value="按钮"/>

              var btn=document.getElementById("btn1");
                 btn.onclick=function(){
                  alert(this.value)    //按钮
                 }


  DOM中的事件对象

		currentTarget     事件处理程序当前正在处理事件的那个元素
		target         事件目标

		 *在事件处理程序内部,对象this始终等于currentTarget值,而target则只包含事件的实际目标
			
			
			 *如果直接将事件处理程序指定给了目标元素,
			  则this,currentTarget和target包含相同的值(如下例子)
			
			var btn=document.getElementById("btn1");

			   btn.onclick=function(){
			   
			    //<input type="button" name="" id="btn1" value="按钮"/>
				console.log(event.currentTarget);  
				
			    //<input type="button" name="" id="btn1" value="按钮"/>
				console.log(event.target);  
				
			    //<input type="button" name="" id="btn1" value="按钮"/>
				console.log(this)        
			} 

                    * 这个例子检测了currentTarget，target，this的值，由于click事件的目标都是按钮，
		       因此这三个值是相等的，
	    
	            * 如果事件处理程序存在于按钮的父节点，那么这些值是不同的
	    
	      如下例子

		document.body.onclick=function(){
		console.log(event.currentTarget);   //<body>...</boby>
		console.log(event.target);    //<input type="button" name="" id="btn1" value="按钮"/>
		console.log(this)           //<body>...</boby>
		}

		      * 当点击这个例子中的按钮时,this和currentTarget都等于document.body,
		       因为事件处理程序是注册到这个元素上的,然而, " target元素却等于按钮元素 ",
		       因为他是click事件正真的目标,由于按钮上并没有注册事件处理程序,
		       结果click事件就冒泡到了document.body,在哪里事件才能得到处理


      
      
      
 preventDefault()方法阻止特定事件的默认行为,
        

		例如:链接的默认行为就是在单击时会导航到href 特性指定的URL,如果想阻止链接这一默认行为,
		那么通过链接的onclick 事件处理程序可以取消它,
      
		 如下例子:
      
      	         <a href="indexa.html" id="a">连接</a>	
		 
		    var a=document.getElementById("a");		
		     a.onclick=function(event){
		      event.preventDefault();
	           }
	     
	     
	     
 stopPropagation() 方法用于立即停止事件在DOM层次中的传播,即取消进一步事件捕获或冒泡
		  
		    
	例如:直接添加到一个按钮事件处理程序可以调用stopPropagation(),从而避免触发注册在document.body
             上面事件处理程序,
		        
			
		如下例子:
				
	       var btn1=document.getElementById("btn1");		
		  btn1.onclick=function(event){
			  alert("input") 	  	
			 event.stopPropagation();   
	             }
		     
                document.body.onclick=function(){
                  	alert("body")
                  }	     
		  
	     
相应的cancelBubble 属性与DOM中 stopPropagation() 方法作用相同,
  

		都是用来停止事件冒泡,由于IE不支持事件捕获,因此只能取消事件冒泡,
		但stopPropagation() 可以同时取消事件捕获和冒泡,
		
		 如下例子:

		      var btn1=document.getElementById("btn1");		
			    btn1.onclick=function(event){
				     alert("input") 	  	
				    event.cancelBubble=true; 
			     }

			    document.body.onclick=function(){
				alert("body")
			      }	     
     	     
	     
 eventPhase 事件对象属性,可以用来确定事件当前正位于事件流的那个阶段,
 
   	 
		 如果是在捕获阶段调用事件处理程序,那么eventPhase 等于 1;
		 如果事件处理程序处于对象上  则 eventPhase 等于 2;
		 如果是在冒泡阶段调用事件处理程序, eventPhase 等于 3
			
	     注意  "处理目标" 发生在冒泡阶段,但eventPhase仍然一直等 2,
			 
	看下面例子:
			  
	var btn1=document.getElementById("btn1");

        btn1.onclick=function(event){
        	console.log(event.eventPhase)    //2
        };

	document.body.addEventListener("click",function(event){
		console.log(event.eventPhase)
	},true);   // 1
	
	document.body.onclick=function(event){
		console.log(event.eventPhase)   //3
	}

	     
	     当点击这个例子按钮时,首先执行的事件处理程序在捕获阶段触发的
	     添加到document.body中的哪一个 ; 结果输出  eventPhase等于  1;
	      接着会触发在按钮上注册的事件处理程序,此时eventPhase等于 2
	     最后一个被触发的事件程序,是在冒泡阶段执行的添加到document.body上的哪一个,
	     显示 eventPhase等于 3,    
	     而当eventPhase等于 2 时, this , target, currentTarget 始终都是相等的



跨浏览器的事件对象


              		     var EventUtil = {
			         addHandler: function(element, type, handler){
					//省略的代码
			           },
				 getEvent: function(event){
				      return event ? event : window.event;
				  },
				  getTarget: function(event){
				      return event.target || event.srcElement;
				     },
			          preventDefault: function(event){
				      if (event.preventDefault){
					  event.preventDefault();
					  } else {
					  event.returnValue = false;
				    	 }
				    },
				  removeHandler: function(element, type, handler){
					    //省略的代码
				     },
			          stopPropagation: function(event){
				      if (event.stopPropagation){
					   event.stopPropagation();
				    	 } else {
					   event.cancelBubble = true;
				    }
				  }
			      };
			
			
                上面这个例子：我们为EventUtil添加了4个新方法。
		
		        第一个：getEvent(),他返回对event对象引用，考虑到IE中事件对象的位置不同
		               可以使用这个方法来取得event()对象，而不必担心指定事件处理程序的方式，
			     
			      在使用这个方法时，必须假设一个事件对象传入到事件处理程序中，而且要把
			      该变量传给这个方法：
			 
			    如下例子  ：

                                <div id="div1">55555</div>
			
			       var btn = document.getElementById("div1");
			          btn.onclick=function(event){

				  event= EventUtil.getEvent(event)  }


			       在兼容DOM的浏览器中，event变量只是简单的传入传出，
			       而在IE中，event参数是未定义的(undfined),因此就会返回window.event。
			       将这一行代码添加到事件处理程序开头，就可以确保随时都可能使用event对象，
			       而不必担心浏览器的兼容问题了。


     第二个：是getTarget()方法，他返回事件的目标,在这个方法内部，会检测event对象的target属性，
            如果存在则返回该属性值，否则,返回srcElement属性值，
			       
			        可以像下面这样使用这个方法
			       var btn = document.getElementById("div1");
			        btn.onclick=function(event){

				  event= EventUtil.getEvent(event)

				  var target=EventUtil.getTarget(event);

				 alert(event.target)   //	<div id="div1">55555</div>
			     }



    第三个：是preventDefault()方法，用于取消事件默认行为，在传入event对象后，这个方法会检查是否存在
            preventDefault()方法，如果存在则调用该方法，如果是preventDefault()不存在，
	    则将returnValue()方法设置为false，
	    
	     下面例子
	              	<a id="a" href="index12.html">a</a>
	      
	             var btn = document.getElementById("a");
		          btn.onclick=function(event){

			  event= EventUtil.getEvent(event)

			  EventUtil.preventDefault(event);
		        }                           



              第四个: stopPropagation()方法,其实现方式类似, 首先尝试使用DOM方法阻止事件流,
              否则就使用cancelBubble属性,
                 
                  来看下面例子:
			              var btn = document.getElementById("div1");
			                 btn.onclick=function(event){
                                            alert("div");
			      	          event= EventUtil.getEvent(event);
			      	       
			      	          EventUtil.stopPropagation(event);
			      	       }
			               
			               document.body.onclick=function(){
			               	alert("body");
			               }
			               
	     
     load（加载完dom后开始执行）unload(切换本页面后执行,它一般用在清除引用)     
	     
	     
 
   onresize:  当浏览器被调整到一个新的高度或者宽度时，就会触发onresize事件
    
               注意这个事件只能在window上有效

	    例子:window.onresize监听div和屏幕的改变

	    <label id="show"></label>

	     window.onresize = adjuest;
	     adjuest();
	     function adjuest(){
	       var label = document.getElementById("show");
	       label.innerHTML = "width = "+window.innerWidth+";height="+window.innerHeight;
	     }



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

                * 这个例子检测了currentTarget，target，this的值，由于click事件的目标都是按钮，因此
	          这三个值是相等的，
	    
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
	     
	     

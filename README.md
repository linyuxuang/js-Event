# js-Event
js_事件


      事件对象
      
		    在触发DOM上的某个事件时，会产生一个事件对象  event, 这个对象中包含着
		    所有与事件有关的信息，包括导致事件的元素，事件的类型以及其他与特定事件相关的信息，
        
		     例如：鼠标操作导致的事件对象中，会包含鼠标位置信息，
		              而键盘操作导致的事件中，会包含与按下的键有关的信息，
		              所有浏览器都支持event对象，但支持方式不同

 

     例子
      
                
                 btn.onclick=function(){
                  alert(event.type)    //click
                 }

    注意  在函数内部 this值等于事件的目标元素


            <input type="button" name="" id="btn1" value="按钮"/>

              var btn=document.getElementById("btn1");
                 btn.onclick=function(){
                  alert(this.value)    //按钮
                 }




		currentTarget     事件处理程序当前正在处理事件的那个元素
		target         事件目标



		在事件处理程序内部,对象this始终等于currentTarget值,而target则只包含事件的实际目标
			
			
			如果直接将事件处理程序指定给了目标元素,
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

            这个例子检测了currentTarget，target，this的值，由于click事件的目标都是按钮，因此
	    这三个值是相等的，
	    
	    如果事件处理程序存在于按钮的父节点，那么这些值是不同的
	    
	 如下例子

	document.body.onclick=function(){
	console.log(event.currentTarget);   //<body>...</boby>
	console.log(event.target);    //<input type="button" name="" id="btn1" value="按钮"/>
	console.log(this)           //<body>...</boby>
        }


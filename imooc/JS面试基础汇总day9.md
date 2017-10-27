## 还是问题开头
- 编写一个通用的事件监听函数

      function bindEvent(obj,type,fn) {
        obj.addEventListener(type,fn);
      }
      bindEvent(a,'click',function(e){
        e.preventDefault();
        alert(...)
      })

> IE低版本使用 attachEvent()绑定事件，和W3C标准不一样

- 描述事件冒泡流程

      <body>
        <div id = 'box1'>
          <div id = 'div1'>激活</div>
          <div id = 'div2'>取消</div>
        </div>
        <div id = 'box2'>
          <div id = 'div3'>取消</div>
          <div id = 'div3'>取消</div>
        </div>
      </body>

      var div1 = $('#div1');
      var body = document.body;
      bindEvent(div1,'click',function(e){
        e.stopPropatation();
        alert('激活')
      })
      bindEvent(body,'click',function(e){
        alert('取消')
      })

- 对于一个无限下拉加载图片的页面，如何给每个图片绑定事件
    - 考点：事件代理(事件冒泡的应用)
    - demo: 如上给body绑定事件

- - -
## 完善通用事件绑定的函数(是否使用事件代理)

      function bindEvent(elem,type,selector,fn) {
        if(fn == null) {
            fn = selector;
            selector = null;
        }
        elem.addEventListener(type,function(e){
            var target;
            if(selector) {
                  target = e.target;
                  if(target.matches(selector)) {
                        fn.call(target,e)
                  }
            }else {
                  return e;
            }
        })
      }
      
使用代理

      var div1  = document.getElementById('#div1');
      bindEvent(div1,'click','a',function(e){...})
      
> div1：外层，a：目标的a标签

不使用代理

      var div1  = document.getElementById('#div1');
      bindEvent(div1,'click',function(e){...})














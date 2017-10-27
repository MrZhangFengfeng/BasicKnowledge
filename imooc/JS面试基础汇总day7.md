# 异步和单线程

## 异步和同步的区别是什么？分别举一个同步和异步的例子

      主要区别的是否阻塞
      console.log(100);
      setTimeout(function() {
        console.log(200);
      },1000);
      console.log(300);
      
对比同步

      console.log(100);
      alert(200); // 1秒后点击确定
      console.log(300);
      
- - -
## 什么时候需要异步
- 在可能需要等待的时候

### 使用异步的场景
- 定义任务(setTimeout,setInterval)
- 网络请求(如从后台拿数据，加载图片等)
- 事件绑定(click事件等。。。)

- - -
## 谈谈异步和单线程
    设置 setTimeout 延迟时间为0.
    单线程的特点就是不能同时干多件事。

- - -
## 其他知识点
- 获取 2017-10-27 格式的日期

      Date.now();  //获取当前毫秒数  1970年到现在
      var date = new Date();
      date.getTime();  //获取当前毫秒数
      
- 获取随机数，要求是长度一致的字符串格式

      var random = Math.random();
      random += random+'0000000000';
      random = random.slice(0,10);
      console.log(random)

- 写一个能遍历对象和数组通用的 forEach 函数

      function myForEach(obj,fn) {
        var key;
        if(obj instanceof Array) {
          obj.forEach(function(item,index) {
            fn(index,item)
          })
        }else {
          for(key in obj){
            fn(key, obj[key])
          }
        }
      }

- 随机数可以用作token，用来随时清除缓存
- `.map`对数组重新组装，生成一个新数组

      var  arr = [1,2,3]
      var res = arr.map(function(item,index) {
          return item*2
      })
      console.log(res)

- filter

      var  arr = [1,2,3]
      var res = arr.filter(function(item,index) {
          if(item>=2) {
             return true
             }
      })
      console.log(res)

- - -
## 

- - -







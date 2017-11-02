# 创建自己的Angular脏值检查

## 设置Scope
Scope仅仅只是一个函数，它其中包含任何我们想要存储的对象。我们可以扩展这个函数的原型对象来复制`$digest`和`$watch`。
我们不需要$apply方法，因为我们不需要在作用域的上下文中执行任何函数 – 我们只需要简单的使用`$digest`。Scope的代码如下所示：

    var Scope = function( ) {
        this.$watchers = [];   
    };
    Scope.prototype.$watch = function( ) {

    };
    Scope.prototype.$digest = function( ) {

    };
    
我们的$watch函数需要接受两个参数，`watchExp`和`listener`。当$watch被调用时，我们需要将它们push进入到Scope的$watcher数组中。
    var Scope = function( ) {
        this.$watchers = [];   
    };
    Scope.prototype.$watch = function( watchExp, listener ) {
        this.$watchers.push( {
            watchExp: watchExp,
            listener: listener || function() {}
        } );
    };
    Scope.prototype.$digest = function( ) {

    };

如果没有提供listener，我们会将listener设置为一个空函数 – 这样一来我们可以$watch所有的变量。

接下来我们将会创建$digest。我们需要来检查旧值是否等于新的值，如果二者不相等，监听器就会被触发。
我们会一直循环这个过程，直到二者相等。这就是”脏值”的来源 – 脏值意味着新的值和旧的值不相等！

    var Scope = function( ) {
        this.$watchers = [];   
    };

    Scope.prototype.$watch = function( watchExp, listener ) {
        this.$watchers.push( {
            watchExp: watchExp,
            listener: listener || function() {}
        } );
    };

    Scope.prototype.$digest = function( ) {
        var dirty;

        do {
                dirty = false;

                for( var i = 0; i < this.$watchers.length; i++ ) {
                    var newValue = this.$watchers[i].watchExp(),
                        oldValue = this.$watchers[i].last;

                    if( oldValue !== newValue ) {
                        this.$watchers[i].listener(newValue, oldValue);

                        dirty = true;

                        this.$watchers[i].last = newValue;
                    }
                }
        } while(dirty);
    };

- - -
接下来，我们将创建一个作用域的实例。我们将这个实例赋值给$scope。我们接着会注册一个监听函数，在更新$scope之后运行$digest！
    var Scope = function( ) {
        this.$watchers = [];   
    };

    Scope.prototype.$watch = function( watchExp, listener ) {
        this.$watchers.push( {
            watchExp: watchExp,
            listener: listener || function() {}
        } );
    };

    Scope.prototype.$digest = function( ) {
        var dirty;

        do {
                dirty = false;

                for( var i = 0; i < this.$watchers.length; i++ ) {
                    var newValue = this.$watchers[i].watchExp(),
                        oldValue = this.$watchers[i].last;

                    if( oldValue !== newValue ) {
                        this.$watchers[i].listener(newValue, oldValue);

                        dirty = true;

                        this.$watchers[i].last = newValue;
                    }
                }
        } while(dirty);
    };


    var $scope = new Scope();

    $scope.name = 'winter';

    $scope.$watch(function(){
        return $scope.name;
    }, function( newValue, oldValue ) {
        console.log(newValue, oldValue);
    } );

    $scope.$digest();


上述代码将会在控制台中输出下面的内容：

    winter undefined
    
这正是我们想要的结果 – $scope.name之前的值是undefined，而现在的值是winter。

- - -
## 实例

现在我们把$digest函数绑定到一个input元素的keyup事件上。这就意味着我们不需要自己去调用$digest。这也意味着我们现在可以实现双向数据绑定。

    var Scope = function( ) {
        this.$$watchers = [];   
    };

    Scope.prototype.$watch = function( watchExp, listener ) {
        this.$$watchers.push( {
            watchExp: watchExp,
            listener: listener || function() {}
        } );
    };

    Scope.prototype.$digest = function( ) {
        var dirty;

        do {
                dirty = false;

                for( var i = 0; i < this.$$watchers.length; i++ ) {
                    var newValue = this.$$watchers[i].watchExp(),
                        oldValue = this.$$watchers[i].last;

                    if( oldValue !== newValue ) {
                        this.$$watchers[i].listener(newValue, oldValue);

                        dirty = true;

                        this.$$watchers[i].last = newValue;
                    }
                }
        } while(dirty);
    };


    var $scope = new Scope();

    $scope.name = 'winter';

    var element = document.querySelectorAll('input');

    element[0].onkeyup = function() {
        $scope.name = element[0].value;

        $scope.$digest();
    };

    $scope.$watch(function(){
        return $scope.name;
    }, function( newValue, oldValue ) {
        console.log('Input value updated - it is now ' + newValue);

        element[0].value = $scope.name;
    } );

    var updateScopeValue = function updateScopeValue( ) {
        $scope.name = 'Bob';
        $scope.$digest();
    };

- - -
## OVER


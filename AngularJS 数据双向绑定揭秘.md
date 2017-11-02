# AngularJS 数据双向绑定揭秘
AngularJS在$scope变量中使用脏值检查来实现了数据双向绑定。脏治检查允许AngularJS监视那些存在或者不存在的变量。

- - -
## $scope.$watch

> $scope.$watch( watchExp, listener, objectEquality );

为了监视一个变量的变化，你可以使用`$scope.$watch`函数。这个函数有三个参数，它指明了:要观察什么`watchExp`,
在变化时要发生什么`listener`,以及你要监视的是一个变量还是一个对象。当我们在检查一个参数时，我们可以忽略第三个参数。例如下面的例子：

    $scope.name = 'winter';

    $scope.$watch( function( ) {
        return $scope.name;
    }, function( newValue, oldValue ) {
        console.log('$scope.name was updated!');
    } );

AngularJS将会在$scope中注册你的监视函数。你可以在控制台中输出$scope来查看$scope中的注册项目。

在控制台中看到$scope.name已经发生了变化 – 这是因为$scope.name之前的值似乎undefined而现在我们将它赋值为`winter`!

对于$wach的第一个参数，你也可以使用一个字符串。这和提供一个函数完全一样。AngularJS
将会把我们的watchExp设置为一个函数，它也自动返回作用域中我们已经制定了名字的变量。

- - -
## $watchers

$scope中的`$watchers`变量保存着我们定义的所有的监视器。如果你在控制台中查看`$watchers`，你会发现它是一个对象数组。

    $watchers = [
        {
            eq: false, // 表明我们是否需要检查对象级别的相等
            fn: function( newValue, oldValue ) {}, // 这是我们提供的监听器函数
            last: 'Ryan', // 变量的最新值
            exp: function(){}, // 我们提供的watchExp函数
            get: function(){} // Angular's编译后的watchExp函数
        }
    ];
    
$watch函数将会返回一个`deregisterWatch`函数。这意味着如果我们使用`$scope.$watch`对一个变量进行监视，我们也可以在以后通过调用某个函数来停止监视。

- - -
## $scope.$apply

当一个控制器/指令/等等东西在AngularJS中运行时，AngularJS内部会运行一个叫做`$scope.$apply`的函数。这个`$apply`函数会接收
一个函数作为参数并运行它，在这之后才会在rootScope上运行`$digest`函数。

下面我们来看看ng-keydown是怎么来使用$scope.$apply的。为了注册这个指令，AngularJS会使用下面的代码:

    var ngEventDirectives = {};
    forEach(
      'click dblclick mousedown mouseup mouseover mouseout mousemove mouseenter mouseleave keydown 
      keyup keypress submit focus blur copy cut paste'.split(' '),
      function(name) {
        var directiveName = directiveNormalize('ng-' + name);
        ngEventDirectives[directiveName] = ['$parse', function($parse) {
          return {
            compile: function($element, attr) {
              var fn = $parse(attr[directiveName]);
              return function ngEventHandler(scope, element) {
                element.on(lowercase(name), function(event) {
                  scope.$apply(function() {
                    fn(scope, {$event:event});
                  });
                });
              };
            }
          };
        }];
      }
    );

上面的代码做的事情是循环了不同的类型的事件，这些事件在之后可能会被触发并创建一个叫做ng-[某个事件]的新指令。
在指令的compile函数中，它在元素上注册了一个事件处理器，它和指令的名字一一对应。当事件被出发时，AngularJS就会
运行`scope.$apply`函数，并让它运行一个函数。

- - -
## 只是单向数据绑定吗？

上面所说的ng-keydown只能够改变和元素值相关联的$scope中的值 ==> 这只是单项数据绑定。这也是这个指令叫做ng-keydown的原因，
只有在keydown事件被触发时，能够给与我们一个新值。
- - -
## 双向数据绑定
我们现在来看一看ng-model。当你在使用ng-model时，你可以使用双向数据绑定 ==> 这正是我们想要的。
**AngularJS使用`$scope.$watch`（视图到模型）以及`$scope.$apply`（模型到视图）来实现这个功能。**

`ng-model`会把事件处理指令(例如keydown)绑定到我们运用的输入元素上 ==> 这就是`$scope.$apply`被调用的地方.
而`$scope.$watch`是在指令的控制器中被调用的。

如果你在调用`$scope.$watch`时只为它传递了一个参数，无论作用域中的什么东西发生了变化，这个函数都会被调用。
在ng-model中，这个函数被用来检查模型和视图有没有同步，如果没有同步，它将会使用新值来更新模型数据。
这个函数会返回一个新值，当它在$digest函数中运行时，我们就会知道这个值是什么.

- - -
## 为什么我们的监听器没有被触发？
如果我们在$scope.$watch的监听器函数中停止这个监听，即使我们更新了$scope.name，该监听器也不会被触发。

正如前面所提到的，AngularJS将会在每一个指令的控制器函数中运行$scope.$apply。如果我们查看$scope.$apply函数的代码，
我们会发现它只会在控制器函数已经开始被调用之后才会运行$digest函数 – 这意味着如果我们马上停止监听，$scope.$watch函数甚至都不会被调用！
但是它究竟是怎样运行的呢？

$digest函数将会在$rootScope中被$scope.$apply所调用。它将会在$rootScope中运行digest循环，然后向下遍历每一个作用域并在每
个作用域上运行循环。在简单的情形中，digest循环将会触发所有位于$$watchers变量中的所有watchExp函数，将它们和最新的值进行对比，
如果值不相同，就会触发监听器。

当digest循环运行时，它将会遍历所有的监听器然后再次循环，只要这次循环发现了”脏值”，循环就会继续下去。如果watchExp的值和最新的
值不相同，那么这次循环就会被认为发现了脏值。理想情况下它会运行一次，如果它运行超10次，你会看到一个错误。

因此当$scope.$apply运行的时候，$digest也会运行，它将会循环遍历$$watchers，只要发现watchExp和最新的值不相等，变化触发事件监听器。
在AngularJS中，只要一个模型的值可能发生变化，$scope.$apply就会运行。这就是为什么当你在AngularJS之外更新$scope时，
例如在一个setTimeout函数中，你需要手动去运行$scope.$apply()：这能够让AngularJS意识到它的作用域发生了变化。









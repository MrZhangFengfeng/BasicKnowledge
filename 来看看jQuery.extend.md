## jQuery.extend( [deep ], target, object1 [, objectN ] )

> 描述: 将两个或更多对象的内容合并到第一个对象。

> 返回：`Object`

- - -
### 详解
- `deep`

    类型: `Boolean`
    如果是 `true`，合并成为递归（又叫做深拷贝）。不支持给这个参数传递 `false`

- `target`

    类型: `Object`
    一个对象，如果附加的对象被传递给这个方法将那么它将接收新的属性，如果它是唯一的参数将扩展jQuery的命名空间。
    
- `object1`

    类型: `Object`
    一个对象，它包含额外的属性合并到第一个参数
    
- `objectN`

    类型: `Object`
    包含额外的属性合并到第一个参数
    
- - -
当我们提供两个或多个对象给`$.extend()`，对象的所有属性都添加到目标对象（`target参数`）。

如果只有一个参数提供给`$.extend()`，这意味着**目标参数**被省略。在这种情况下，**jQuery对象本身被默认为目标对象**。
这样，我们可以在jQuery的命名空间下添加新的功能。

请记住，目标对象（第一个参数）将被修改，并且将通过`$.extend()`返回。然而，如果我们想保留原对象，我们可以通过传递一个空对象作为目标对象：

        var object = $.extend({}, object1, object2);
        
在默认情况下，通过`$.extend()`合并操作不是递归的;如果第一个对象的属性本身是一个对象或数组，那么它将完全用第二个对象相同的`key`重写一个属性。这些值不会被合并。可以通过检查下面例子中 banana 的值，就可以了解这一点。然而，如果将 `true` 作为该函数的第一个参数，那么会在对象上进行递归的合并。

**警告:不支持第一个参数传递 false 。**

- - -

### 例子  *合并两个对象，并修改第一个对象*。

        <script>
                var object1 = {
                  apple: 0,
                  banana: { weight: 52, price: 100 },
                  cherry: 97
                };
                var object2 = {
                  banana: { price: 200 },
                  durian: 100
                };
                
                // Merge object2 into object1
                $.extend( object1, object2 );

                // Assuming JSON.stringify - not available in IE<8
                $( "#log" ).append( JSON.stringify( object1 ) );
        </script>
        
        {"apple":0,"banana":{"price":200},"cherry":97,"durian":100}
        
- - -
未定义的属性不会被复制。然而，从对象原型的继承属性将被复制。如果属性（Properties）是一个通过构造函数`new MyCustomObject(args)`定义的，或JavaScript中内置类型，如`Date` 或 `RegExp`，是不会重新创建的，并且将被当作普通的对象出现在返回的对象或数组中。

若设置了 `deep` 参数，对象和数组也会被合并进来，但是对象包裹的原始类型，比如`String`, `Boolean`, 和 `Number`是不会被合并进来的。

若要满足其它不同于该行为的需求，编写自定义的扩展方法代替。或者使用其他库，比如`lodash`

- - -
### 例子  *采用递归合并两个对象，并修改第一个对象*。

        <script>
                var object1 = {
                  apple: 0,
                  banana: { weight: 52, price: 100 },
                  cherry: 97
                };
                var object2 = {
                  banana: { price: 200 },
                  durian: 100
                };

                // Merge object2 into object1, recursively
                $.extend( true, object1, object2 );

                // Assuming JSON.stringify - not available in IE<8
                $( "#log" ).append( JSON.stringify( object1 ) );
        </script>
        
        {"apple":0,"banana":{"weight":52,"price":200},"cherry":97,"durian":100}

- - -


        
        


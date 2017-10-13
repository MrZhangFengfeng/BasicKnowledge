## 数组范围
数组内元素个数的范围是： `[0,2^23 -1]`

    [1,,2]//1,undefined,2

- - -
## 创建数组

    let arr1 = new Array();
    let arr2 = new Array(100); //创建了一个长度为100，内容为undefined的数组
    let arr3 = new Array(true,false,null,1,2,'hi'); //[true,false,null,1,2,'hi']

- - -
## 删除数组

    delete arr[1];
    arr[1] //变为undefined

    arr = [1,2,3,4];
    arr.length -= 1;
    arr;//[1,2,3]  4 is removed

    let arr= [1,2,3]
    delete(arr[2])
    console.log(arr) //[1, 2, undefined]  第三个还是在的
 
 - - -
 ## 稀疏数组
 稀疏数组： 并不含有从0开始的连续索引，一般length属性值比实际元素个数大。
 
    let arr1 = [undefined];
    let arr2 = new Array(1)
    let arr3 = [,,]
    arr1[100] = 2;

    console.log(0 in arr1)
    console.log(0 in arr2)
    console.log(0 in arr3)
    console.log(99 in arr1)

- - -
## 数组的方法

    arr.sort() //不会改变原数组

    let arr = [1,2,3]
    arr.concat(4,5) //[1,2,3,4,5]
    arr.concat([6,7],9) //[1,2,3,6,7,9]
    arr; //[1,2,3]   //不会改变原数组

    arr.splice()  //删除掉数组中的某些元素，改变原数组。返回值是被删除的元素的集合！

    arr.forEach(function(item,index,obj){...});  //obj 指向arr
    arr === obj // true

    arr.map(function(item){return item+10})// 返回新数组，不改变原数组
    
    arr.filter(function(item,index){
        return item >6 || index %2 ==0;
    })
- - -
##  every and some
    let arr = [1,2,3,4,5];
    arr.every(function(item){return item < 10})  //true
    arr.every(function(item){return item < 3})  //false

    arr.some(function(item){return item < 10})  //true
    arr.some(function(item){return item < 3})  //true

- - -
## reduce
    let arr = [1,2,3];
    let sum = arr.reduce(function(x,y){
        return x + y;
    }) 
    sum; //6
    arr; //[1,2,3]
    
求最大值

    let arr = [1,2,3];
    let max = arr.reduce(function(x,y){
        return x > y ? x : y;
    }) 
    max; //3

`reduce` 和 `reduceRight`的区别:  `reduceRight` 是从数组的右侧开始向左进行操作。

- - -







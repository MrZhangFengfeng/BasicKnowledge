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

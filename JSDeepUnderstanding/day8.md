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

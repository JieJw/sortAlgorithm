### 数组排序
#### 原生sort排序
js数组对象自带的sort排序，对于传入的参数，会给每一项调用toString()方法，在排序
也可传入函数参数，其中函数包含两个参数value1和value2，其中value1和value2分别表示前一个和后一个数组项，当返回的数大于0时，数组项对换，当返回属性小于或等于0时，数组项顺序不变
```
var arr = [13,23,2,15,9,11,76]
console.log( arr.sort() );  //[11, 13, 15, 2, 23, 76, 9]

console.log( arr.sort( (value1, value2) => value1-value2 ) );   //[2, 9, 11, 13, 15, 23, 76]

console.log( arr.sort( (value1, value2) => value2-value1 ) );   //
```

#### 冒泡排序
##### 个人理解
对于传入函数的数组，将最大（或最小）的冒泡到数组的最后一项，实现排序。主要思想是俩俩比较，交换位置，然后比较除一个最大（或最小）的数，交换到数组末尾，接着除了最后项再俩俩比较，以此类推
```
var arr = [4,2,5,1,3];
function bubbleSort( a ) {
    var len = a.length;
    for( var i = 0; i < len; i++ ) {
        for( var j = 0; j < len-i-1; j++ ) {
            if( a[j] > a[j+1] ) {
                var temp = 0;
                temp = a[j];
                a[j] = a[j+1];
                a[j+1] = temp;
            }
        }
    }
    return a;
}
```
##### 算法分析
最佳情况：当输入的数据已经是正序时 T(n) = O(n)
最差情况：当输入的数据是反序时 T(n) = O(n2)
平均情况：T(n) = O(n2)

#### 选择排序
##### 个人理解
将数组的分为一个已排序数组和未排序数组，在排序过程中，未排序数组的第一项设置为最小值所在位置，通过第一项与其他项对比，将最小值设为第一项，并将为排序数组的第一项换到已排序数组中，后续的则继续排序，以此类推
```
var a = [3,5,1,2,4];
function selectSort( arr ) {
    var len = arr.length;
    for(var i = 0; i < len-1; i++ ) {
        for( var j = i+1; j < len; j++ ) {
            if( arr[i] > arr[j] ) {
                var temp = 0;
                temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return arr;
}       
```
##### 算法分析
最佳情况：T(n) = O(n2) 最差情况：T(n) = O(n2) 平均情况：T(n) = O(n2)
#### 插入排序
##### 个人理解
对于一个待排序数组，第一次循环将第二项作为一个需要插入已排序数组的目标，该目标会对前面项的已排序数组中逐个比较，对于比其大的项会逐项后移，并将目标插入到合适的位置
**在重后向前的扫描中，需要不断的已排序数组向后移，为新元素提供插入空间**
```
var a = [2,5,3,1,4]
function insertSort( arr ) {
    var len  = arr.length;
    for( var i = 1; i< len; i++ ) {
        var key = arr[i];
        var j = i-1;
        while( j >= 0 && arr[j] > key ) {
            arr[j+1] = arr[j];
            j--;
        }
        arr[j+1] = key;
    }
    return arr;
}
```
##### 算法分析
最佳情况：输入数组按升序排列。T(n) = O(n) 
最坏情况：输入数组按降序排列。T(n) = O(n2) 
平均情况：T(n) = O(n2)
#### 希尔排序
##### 个人理解
希尔排序传入一个数组，在初始阶段，设置一个间隔序列，在这个序列中进行一个类似的插入排序，进行完一次后，间隔缩小，继续插入排序，直到每次移动的步长变为一最后进行插入排序
```
function shellSort( arr ) {
    var len = arr.length;
    var temp;
    var gap = 1;
    while( gap < (len/5) ) {
        gap = gap*5+1;
    }
    for( gap; gap > 0; gap = Math.floor( gap/5 ) ) {
    for( i = gap; i < len; i++ ) {
        temp = arr[i];
        for( var j = i-gap; j >= 0 && arr[j] > temp; j -= gap ) {
            arr[j+gap] = arr[j]
        }
        arr[j+gap] = temp;
    }
    }
    return arr;
}
    
```
##### 算法分析
最佳情况：T(n) = O(nlog2 n) 
最坏情况：T(n) = O(nlog2 n) 
平均情况：T(n) = O(nlog n)
#### 归并排序
##### 个人理解
归并排序是将一个数组分为最小的长度的小数组，即只有一项，然后根据逐项的对比，利用shift()函数取数组第一项push到一个新建数组，最后返回一个合并后且排序好的数组
有点递归的感觉，将数组划分为n/2长度，再划分为n/4长度。。。让子序列合并为排序好的数组
```
var a = [2,5,3,1,6,4,7]
function mergeSort( arr ) {
    var len = arr.length;
    if( len < 2) {
        return arr
    }
    var middle = Math.floor( len/2 );
    var left = arr.slice( 0, middle );
    var right = arr.slice( middle )
    return merge( mergeSort(left), mergeSort(right) );
}
function merge( left, right ) {
    var result = [];
    while( left.length && right.length ) {
        if( left[0] <= right[0] ) {
            result.push( left.shift() );
        }else{
            result.push( right.shift() );
        }
    }
    while( left.length ) {
        result.push( left.shift() );
    }
    while( right.length ) {
        result.push( right.shift() );
    }
    return result;
}
```
##### 算法分析
最佳情况：T(n) = O(n) 
最差情况：T(n) = O(nlogn) 
平均情况：T(n) = O(nlogn)
#### 快速排序
#### 方法一
##### 个人理解
传入数组以及数组的第一项序号和最后一项序号，紧接着，将数组的最后一项作为一个基准，将数组中比基准小的位置设置为i，从数组的第一项到最后一项，逐项与基准比较，在比较的过程中，若比基准小，则i加一，通过循环整个left到right，最后将i+1与right中的数据交换，则基准前的位置都比其小，后的位置都比基准大
```
function quickSort( arr, left, right ) {
    if( left < right ) {
        var x = arr[right], i = left-1, temp;
        for( var j = 0; j <= right; j++ ) {
            if( arr[j] <= x ) {
                i++;
                temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        quickSort( arr, left, i-1 );
        quickSort( arr, i+1, right );
    }
    return arr;
}
```
#### 方法二
##### 个人理解
函数将数组的分为两个子数组，把中间位置的函数移除，作为一个基准，大于它的放在右边数组，小于它的放在左边数组，再将左右数组以此类推，最后用concat方法，将数据连接起来
```
function quickSort( arr ) {
    if( arr.length <= 1 ) {
        return arr;
    }
    var len = arr.length;
    var middle = Math.floor( len/2 );
    var num = arr.splice( middle, 1 )[0];
    var left = [], right = [];
    for( var i = 0; i < arr.length; i++ ) {
        if( arr[i] <= num ) {
            left.push( arr[i] );
        }else{
            right.push( arr[i] );
        }
    }
    return quickSort(left).concat( [num], quickSort(right) );
}
```
##### 算法分析
最佳情况：T(n) = O(nlogn) 
最差情况：T(n) = O(n2) 
平均情况：T(n) = O(nlogn)
#### 计数排序
##### 个人理解
计数排序把数组中的值作为键保存在了一个额外数组中。
定义一个数组，数值中将目标数组的值作为键保存下来，且该键的值为数值出现的次数
再将数组转化，每一项的值表示对于最小值的的大于次数+1
最后通过填充目标数组B，把每个元素放在转化数组C的第C[i]项，每放一个元素就将C[i]-1
```
function countSort( arr ) {
    var B = [],
        C = [],
        len = arr.length,
        min = max = arr[0];
    for( var i = 0; i < len; i++ ) {
        min = min <= arr[i] ? min : arr[i];
        max = max >= arr[i] ? max : arr[i];
        C[ arr[i] ] = C[ arr[i] ] ? C[ arr[i] ] + 1 : 1;
    }
    for( var j = min; j < max; j++ ) {
        C[j+1] = ( C[j+1] || 0 ) + ( C[j] || 0 )
    }
    for( var k = len-1; k >= 0; k-- ) {
        B[ C[ arr[k] ] -1 ] = arr[k];
        C[ arr[k] ]--;  //去掉重复情况
    }
    return B;
}    
```
##### 算法分析
输入的元素是n个0到k之间的整数
最佳情况：T(n) = O(n+k) 
最差情况：T(n) = O(n+k) 
平均情况：T(n) = O(n+k)
#### 堆排序
#### 桶排序
#### 基数排序

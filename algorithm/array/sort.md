# 排序

排序是很常用的算法，尽管STL中有现成的排序实现，但是我们还是应该掌握常见的的排序算法，这里我们给出面试经常遇到的快速排序和归并排序的模板，其他排序算法很简单略。

## 1. 快速排序
### 1.1 算法描述
快排应该是最常用的排序算法了，面试时也经常被要求手撕快排，因此务必掌握，尤其是其核心的划分（partition）思想，在很多场合都能用到。

快速排序使用分治法（Divide and conquer）策略来把待排数组分为两个子数组，然后递归地排序两个子数组。

步骤为：

1. 挑选基准值：随机挑出一个元素（一般选第一个元素就可以了），称为“基准”（pivot），
2. 划分（partition）：重新排序数组，使所有比基准值小的元素摆放在基准前面，所有比基准值大的元素摆在基准后面（与基准值相等的数可以到任何一边）。
3.  递归排序子数组：递归地将小于基准值元素的子数组和大于基准值元素的子数组排序；递归出口是子数组大小小于1。


详解：
例如，[4,5,6,7,1,2,3],循环的目的是将大于基准的数字放在右面，小于基准的放在左面，最关键是基准和小于基准的最后一个数调换位置.基准设为left位置（0），arr[left]=4，设置一快一慢指针（i,j，初始值都为left+1）,快指针用来循环比较，切记录如果当前数字大于基准的位置，方便与慢指针调换数据；
慢指针用来记录小于基准的数字的位置。循环开始，如果数值大于基准，i+=1，j不动，如果数值小于基准，那么i和j数值调换，j+=1，最后因为j提前+1了，所以调换基准的位置就是left和j-1，然后返回基准的最终位置j-1，再次循环就从0到（j—1）-1和（j-1）+1到len(arr)-1开始，直到right-left=1，既只有一个元素。

选取基准值有数种具体方法，选取方法对排序的时间复杂度有决定性影响。

时间复杂度: 
* 平均（每次都划分得很均匀）: O(nlogn)
* 最坏（排序数组已为正序或逆序）: O(n^2)

### 1.2 代码

``` python
def quicksort(arr,left,right):
    if left<right:
        tmp=par(arr,left,right)
        quicksort(arr,left,tmp-1)
        quicksort(arr, tmp+1, right)

    return arr



def par(arr,left,right):
    pivot=left    #设最左面位置元素为基准
    index=pivot+1    #设置慢指针
    i=index     #设置快指针
    while i<=right:
        if arr[i]<arr[pivot]:
            swap(arr, index, i)
            index+=1
        i+=1
        
    swap(arr,pivot, index-1)   #把基准放在arr最终位置

    return index-1


def swap(arr, i, j):
    arr[i], arr[j] = arr[j], arr[i]
```

## 2. 归并排序
### 2.1 算法描述
归并排序采用的是分治法（Divide and Conquer）思想。基本步骤是：
1. 拆分：将原序列划分成子序列，然后假设子序列已经有序；
2. 合并：将有序的子序列合并，得到完全有序的序列；

详解：根据递归思想，当数列只有一个元素时，它是有序的，那么先将序列拆分到单个元素；然后再两两组合比较，放一个空列表，依次比较两个有序子数列，从小到大放入列表，并pop(0)，当一个子序列为空时,另一个有序子序列的剩下元素都放在列表里，循环到最后两个子序列比较，得出结果。




归并排序具体有两种思路：一种是自上而下递归地归并，另一种是自下而上迭代地归并。

时间复杂度：最坏、平均时间复杂度均为O(nlogn)。

### 2.2 代码

``` python
def mergesof(arr):
    import math
    if len(arr)<2:
        return arr
    middle=math.floor(len(arr)/2)
    left,right=arr[0:middle],arr[middle:]
    return merge(mergesof(left),mergesof(right))

def merge(left,right):
    result=[]

    while left and right:
        if left[0]>=right[0]:
            result.append(right[0])
            right.pop(0)
        else:
            result.append(left[0])
            left.pop(0)
    while left:
        result.append(left.pop(0))

    while right:
        result.append(right.pop(0))
    print("resule:",left,right,result)

    return result
```

排序算法(Python)
===
目录：
---
<ul>
<li>冒泡排序（bubble——sort）</li>
<li>选择排序（select——sort）</li>
<li>快速排序（quick——sort）</li>
<li>插入排序（insert——sort）</li>
<li>希尔排序（shell——sort）</li>
<li>归并排序（merge——sort）</li>
<li>堆排序（heap——sort）</li>
<ul>
<h3>冒泡排序</h3>
冒泡排序算法的原理如下：<br>
 1、比较相邻的元素。如果第一个比第二个大，就交换他们两个。<br>
 2、对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。<br>
 3、针对所有的元素重复以上的步骤，除了最后一个。<br>
 4、持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。<br>
 
```python
def bubble_sort(nums):
    n=len(nums)-1
    for i in range(n):    #控制循环次数
        for j in range (0,n-i):
            if nums[j] > nums[j + 1]:
                nums[j], nums[j + 1] = nums[j + 1], nums[j]
    return nums
```

优化版冒泡

```python
def bubble_sort(nums):
    for i in range(len(nums)-1):    #控制循环次数
        flag=ture   #标记
        for j in range (len (nums)-i-1):
            if nums[j] > nums[j + 1]:
                nums[j], nums[j + 1] = nums[j + 1], nums[j]
                flag=false
        if flag:
            break   #若某循环中没有数据交换，说明已完成排序，不必再迭代
    return nums
```

<h3>选择排序</h3>
选择排序工作原理：<br>
1、首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置。<br>
2、再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。<br>
3、重复第二步，直到所有元素均排序完毕。

```python
def select_sort(ary):
    for i in range(len(ary)-1):
        mini=i      #最小元素下标标记
        for j in range (i+1,len(ary)):
            if ary[mini] > ary[j]:
                mini=j
        ary[i], ary[mini] = ary[mini], ary[i]
    return ary
```

<h3>快速排序</h3>
快速排序的原理如下：<br>
1、从数列中挑出一个元素作为基准数。<br>
2、分区过程，将比基准数大的放到右边，小于或等于它的数都放到左边。<br>
3、再对左右区间递归执行第二步，直至各区间只有一个数。

```python
def quick_sort(nums):
    return qksort(nums,0,len(nums)-1)

def qksort (nums,start,end):
    if start <end:
        left=start
        right=end
        key=nums[start]     # 选择基准（pivot）
    else:
        return nums
    while left<right:
        while left<right and nums[right]>=key:
            right-=1
        if left<right:      # 说明打破while循环的原因是ary[right] <= key
            nums[left]=nums[right]
            left+=1
        while left<right and nums[left]<=key:
            left+=1
        if left<right:      # 说明打破while循环的原因是ary[left] >= key
            nums[right]=nums[left]
            right-=1
    nums[left]=key          # 此时，left=right，用key来填坑

    qksort(nums,start,left-1)     # 调用递归
    qksort(nums,left+1,end)
    return nums
```

另一种快速排序的方法是：<br>
    1、选择基准数，将数组划分为左子数组和右子数组<br>
    2、对左右两个子数组递归重复上述步骤直至左右两个子数组有序<br>
    3、返回值为左子数组+基准数+右子数组<br>
快速（分而治之+递归）

```python
def quicksort(nums):
    if len(nums)<=1:
        return nums

    less=[]           #左子数组
    greater=[]        #右子数组
    base=nums.pop()   #基准数
    
    for x in nums:
        if x <base:
            less.append(x)
        else:
            greater.append(x)
    return quicksort(less) + [base] + quicksort(greater)      #递归调用
```

<h3>插入排序</h3>
插入排序原理如下：<br>
    1、默认第一个元素已经被排序（即第一个元素就是有顺序的序列）<br>
    2、取出下一个元素，在已经排序的元素序列中从后向前扫描<br>
    3、如果被扫描的元素（已排序）大于新元素，将该元素后移一位<br>
    4、重复步骤3，直到找到已排序的元素小于或者等于新元素的位置<br>
    5、将新元素插入到该位置后<br>
    6、重复步骤2~5
    
```python
def insert_sort(nums):
    for i in (1,len(nums)):
        mark=nums[i]    #由于num[i]可能会被覆盖，因此必须用mark来存储它的值
        k=i-1
        while k>=0 and nums[k]>mark:
            nums[k+1]=nums[k]
            k-=1
        nums[k+1]=mark
    return nums
```   

<h3>希尔排序</h3>
希尔排序可以视为一种特殊的插入排序<br>
希尔排序原理如下：<br>
    1、选择一个增量序列 t1，t2，……，tk，其中 ti > tj, tk = 1；<br>
    2、按增量序列个数 k，对序列进行 k 趟排序；<br>
    3、每趟排序，根据对应的增量 ti，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。<br>
       仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。<br>
       
```python
def shell_sort(nums):
    count=len(nums)
    gap=count//2    #/在Python中得到浮点数，//在Python中意为整除
    while gap>=1:
        for i in range (gap,count):
            mark=nums[i]
            j=i
            while j-gap>=0 and nums[j-gap]>mark:
                nums[j]=nums[j-gap]
                j-=gap
            nums[j]=mark
        gap=gap//2
    return nums
```

<h3>归并排序</h3>
归并排序原理如下：<br>
    1、把长度为n的输入序列分成两个长度为n/2的子序列；<br>
    2、对这两个子序列分别采用归并排序；<br>
    3、将两个有序的子序列合并成一个最终的排序序列。<br>
    
```python
def mergesort(ary):
    if len(ary)<=1:
        return ary
    median = len(ary)//2      #将原有序列二分为子序列
    left=mergesort(ary[:median])
    right=mergesort(ary[median:])
    return merge(left,right)      #合并子序列

def merge(left,right):
    res=[]
    i=j=0
    while (i<len(left) and j<len(right)):     #对比左右两项，数字小的加入序列
        if left[i]<=right[j]:
            res.append(left[i])
            i+=1
        else:
            res.append(right[j])
            j+=1
    res += left[i:]     #对比结束后，将剩余的项加入列表。由于递归，剩余项一定比前面大，故而放入列表后侧
    res += right[j:]
    return res 
```

<h3>堆排序</h3>
堆排序利用了二叉树的数据结构来进行排序<br>
堆排序原理如下：<br>
    1、构建最大堆build max heap：将无序的初始列表排列成最大堆。根据二叉树的原理，在一组[R0,R1...Rn-1]中，可以认为单独一个元素是大根堆。<br>
      数组中后n/2的项已经排列好，所以只需要考虑前一半的项。那么只要从n/2-1开始，向前依次构造大根堆。<br>
      这样就能保证，构造到某个节点时，它的左右子树都已经是大根堆。<br>
    2、进行堆排序：由于堆是用数组模拟的。得到一个大根堆后，数组内部并非有序。因此需要将堆化数组有序化。思想是移除根节点，并做最大堆调整的递归运算。<br>
      将R[0]与R[n-1]交换，也就是将堆顶与堆尾互换。重复操作直至R[0]和R[1]交换。将最大的数并入到后面的有序区间，最后整个数组就是有序的了。<br>
    3、调整最大堆（即再次构建最大堆）max heapify：交换后新的堆顶可能违反堆的性质，因此需要对当前无序区(R1,R2,……Rn-2)调整为新堆<br>
    
```python
def heapsort(ary):
    n=len(ary)
    first=n//2-1
    for start in range(first,-1,-1):            #从下到上，从左到右对每个节点进行调整，循环得到非叶子节点  
        max_heapify(ary,start,n-1)              #去调整所有的节点  
    for end in range (n-1,0,-1):
        ary[end],ary[0] = ary[0],ary[end]       #顶部尾部互换位置 
        max_heapify(ary,0,end-1)                #重新调整子节点的顺序，从顶开始调整  
    return ary
    
def max_heapify(ary,start,end):
    root= start
    child=root*2+1          #左孩子
    while child<=end:       #孩子比最后一个节点还大，也就意味着最后一个叶子节点了，就得跳出去一次循环，已经调整完毕   
        if child +1<=end and ary[child]<ary[child+1]:
            child=child+1       #为了始终让其跟子元素的较大值比较，如果右边大就左换右，左边大的话就默认   
        if ary [root]<ary[child]:           #父节点小于子节点直接交换位置，同时坐标也得换，这样下次循环可以准确判断：是否为最底层且调整完毕
            ary[root],ary[child]=ary[child],ary[root]
            root=child
            child=root*2+1
        else:
            break
```

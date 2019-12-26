查找算法(更新中)
===
目录
---
<ul>
<li>顺序查找(sequential_search)</li>
<li>二分查找(binary_search)</li>
</ul>
<h3>顺序查找</h3>
顺序查找原理如下：<br>
  对于任意一个序列以及一个给定的元素，将给定元素与序列中元素依次比较，直到找出与给定关键字相同的元素，或者将序列中的元素与其都比较完为止。
  
```python
def sequential_search(lis,key):
    flag=0
    for i in range(len(lis)):
        if lis[i]==key:
            flag=1
            res=i+1   #下标从0开始，故找到的是第i+1个数
            break
    if flag:
        return res
    else:
        return False
    
if __name__=='__main__':
    LIST= [1, 5, 8, 123, 22, 54, 7, 99, 300, 222]
    print(sequential_search(LIST, 8))
```
<h3>二分查找</h3>
二分查找原理如下：<br>
  二分查找的表必须按照关键字大小有序排列，且采用顺序存储结构。在升序排列的表中，首先将表中间位置的数据与关键字对比，若相等则查找成功
  否则利用中间字将表分为前后两个子表。若关键字小，那么查找前子表；若关键字大，则查找后子表。重复步骤直至找到或子表不存在。
  
```python
def bin_search(lis, key):    
    low = 0                         # 最小数下标    
    high = len(lis) - 1       # 最大数下标    
    while low <= high:        
        mid = (low + high) // 2     # 中间数下标        
        if lis[mid] == key:   # 如果中间数下标等于key, 返回            
            return mid        
        elif lis[mid] > key:  # 如果key在中间数左边, 移动high下标            
            high = mid - 1        
        else:                       # 如果key在中间数右边, 移动low下标            
            low = mid + 1    
    return False        # val不存在, 返回False
    ```

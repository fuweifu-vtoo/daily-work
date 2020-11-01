## python中的排序

1. 对list进行排序：`nums = sorted(nums,key=lambda x: -x,reverse = True)`
2. 对list中的二元组进行排序：`nums.sort(key = lambda x:[x[1],-x[0]])`
3. list排序可以使用sort和sorted，对dict.items()排序只能使用sorted；
4. 对dict排序：
	1. ` nums = {1:(1,2),3:(2,1)}`
	2. `nums_list = sorted(nums.items(),key = lambda x : x[1][1]))`
5. 不使用list中的元素本身进行排序，使用两者的比较关系进行排序，此时使用`cmp_to_key`：
	1. `from functools import cmp_to_key`
	2. `temp = ['10','2']`
	3. `temp.sort(key = cmp_to_key(lambda x,y:int(x+y)-int(y+x)),reverse=True)`
	4. leetcode-剑指offer45
	5. `cmp_to_key`的本质是看内部的数字大于0、等于0还是小于0
	6. 但是这道题的本质考察的是没有额外存储消耗的快速排序
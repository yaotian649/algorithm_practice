# Binary Tree
Pros:  
* O(lgn), effective search method

Cons:  
* requires sorted array ot intervals  
	* average O(n) for dynamic delete and insert for array;
	* use balanced Binary-search Tree, O(nlogn) to build this tree for data structure;

## 递归写法 recursion 
```Java
int binarySearch(int[] nums, int target, int low, int high){
	// avoid endless loop
	if(low > high) return -1;
	
	int middle = low + (high - low) / 2;
	// find the middle is index of target 
	if(nums[middle] == target) return middle;
	// target is in left or right
	if(target < nums[middle]){
		return binarySearch(nums, target, low, middle - 1);
	} else{
		return binarySearch(nums, target, middle+1, high);
	}
}
```

## 非递归写法 non-recursion
```Java
int binarySearch(int[] nums, int target, int low, int high){
	while(low <= high){
		int middle = low + (high - low)/2;
		
		if(nums[middle] == target) return middle;
		
		if(target < nums[middle]){
			high = middle - 1;
		} else{
			low = middle + 1;
		}
	}
	return -1;
}
```

## 题型变换
### 找确定的边界 find deterined boundary
* refer lower bound and upper bound
* boundary value == target value
#### 34. Find First and Last Position of Element in Sorted Array 最长的子序列的长度
* Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.  
* Your algorithm's runtime complexity must be in the order of O(log n).  
* If the target is not found in the array, return [-1, -1].  
* Input: nums = [5,7,7,8,8,10], target = 8,   Output: [3,4]
##### 2个成为8的下边界的条件
1. 该数必须是8
2. 该数的左边一个数必须不是8
	- 8的左边有数，那么该数必须小于8
	- 8的左边没有数，即8是数组的第一个数
```Java
int searchLowerBound(int[] nums, int target, int low, int high){
	// avoid endless loop
	if(low > high) return -1;
	
	int middle = low + (high - low) / 2;
	// find the middle is index of target 
	if(nums[middle] == target && (middle == 0 || nums[middle- 1] < target)){
		return middle;
	} 
	// target is in left or right
	if(target <= nums[middle]){
		return searchLowerBound(nums, target, low, middle- 1);
	} else{
		return searchLowerBound(nums, target, middle+ 1, high);
	}
}
```
##### 2个成为8的上边界的条件
1. 该数必须是8
2. 该数的右边一个数必须不是8
	- 8的右边有数，那么该数必须大于8
	- 8的右边没有数，即8是数组的最后一个数
```Java
int searchUpperBound(int[] nums, int target, int low, int high){
	// avoid endless loop
	if(low > high) return -1;
	
	int middle = low + (high - low) / 2;
	// find the middle is index of target 
	if(nums[middle] == target && (middle == nums.length- 1 || nums[middle+ 1] > target)){
		return middle;
	} 
	// target is in left or right
	if(target < nums[middle]){
		return searchUpperBound(nums, target, low, middle- 1);
	} else{
		return searchUpperBound(nums, target, middle+ 1, high);
	}
}
```

### 找模糊的边界 find vague boundary
* find the first number bigger than 6 in the sorted array {-2, 0, 1, 4, 7, 9, 10}
#### 如何在有序数组中判断一个数是不是第一个大于6的数
* 这个数要大于6
* 这个数有可能是数组里的第一个数，或者它之前的一个数比6小
```Java
int firstGreaterThan(int[] nums, int target, int low, int high){
	// avoid endless loop
	if(low > high) return null;
	
	int middle = low + (high - low) / 2;
	// 判断middle指向的数是否为第一个比Target大的数时, 需要满足两大条件
	// 1. middle这个数必须大于target
	// 2. middle要么是第一个数，or 它之前的数小于或者等于target
	if(nums[middle] > target && (middle == 0 || nums[middle- 1] <= target)){
		return middle;
	} 
	// target is in left or right
	if(target <= nums[middle]){
		return searchLowerBound(nums, target, low, middle- 1);
	} else{
		return searchLowerBound(nums, target, middle+ 1, high);
	}
}
```

### 33. 旋转过的排序数组
* 给定一个经过旋转了的排序数组，判断一下某个数是否在里面
* 例如：给定数组{4, 5, 6, 7, 0, 1, 2},traget等于0，答案是4，即0所在的位置下标是4
	* 如何判断左边是不是排好序的那部分 ->比较nums[low]和nums[middle],如果nums[low]<= nums[middle],则左边排好序，否则右边排好序；
	* 判定某一边是排好序的作用 -> 若nums[low]<= target && target< nums[middle],则目标值在这个区间，反之在另一边；
```Java
int binarySearch(int[] nums, int target, int low, int high){
	if(low > high) return -1;
	int middle = low + (high - low) / 2;
	if(nums[middle] == target) return middle;
	
	//判断左半边是否排好序
	if(nums[low] <= nums[middle]){
		// 目标值是否在左半边，是的话在左边进行二分搜索
		if(nums[low] <= target && target < nums[middle]) return binarySearch(nums, target, low, middle-1);
		// 否则在右半边进行二分搜索
		return binarySearch(nums, target, middle+1, high);
	} else {
		if(nums[middle] < target && target <= nums[high]) return binarySearch(nums, target, middle+1, high);
		return binarySearch(nums, target, low, middle-1);
	}	
}
```

# Greedy 贪婪算法
## 例子
贪婪物品有：A B C  
重量分别是：25 10 10  
价值分别是：100 80 80  

# Binary Tree
Pros:  
* O(lgn), effective search method

Cons:  
* requires sorted array ot intervals  
	* average O(n) for dynamic delete and insert for array;
	* use balanced Binary-search Tree, O(nlogn) to build this tree for data structure;

## 递归写法
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

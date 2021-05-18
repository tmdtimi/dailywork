##Sort


### quickSort

![](../pics/sort1.png)

选定基准元素 pivot，并设定left和right两个指针，来指向最左和最右两个元素

![](../pics/sort2.png)


第一次循环：

while(right>=pivot) right--;

while(left<=pivot) left++;

![](../pics/sort3.png)

交换 left 和 right 元素

![](../pics/sort4.png)


继续重复循环

直到left==right

![](../pics/sort5.png)

此时交换 pivot 和 left\right重复的那个元素

![](../pics/sort6.png)


	    public static void sort(int[] nums,int left,int right){
        if(left<right){
        int patition=getPivot(nums,left,right);
        sort(nums,left,patition-1);
        sort(nums,patition+1,right);
        }

    }

    public static int getPivot(int []nums,int left,int right){
        int pivot=left;

        while(left<right){
            while(nums[right]>=nums[pivot] && left<right){
                right--;
            }
            while(nums[left]<=nums[pivot] && left<right){
                left++;
            }
            if(left<right){
                int temp=nums[left];
                nums[left]=nums[right];
                nums[right]=temp;
            }            
        }
        int temp=nums[pivot];
        nums[pivot]=nums[left];
        nums[left]=temp;
        pivot=left;

        return pivot;

    }

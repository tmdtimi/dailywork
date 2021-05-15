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




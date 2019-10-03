[TOC]

# 动态规划（Dynamic Program）

**1.递归 + 记忆化 ------>递推**，重点就是**递推（从尾部往头部递推）**，递归+记忆化只是过渡

**2.状态的定义：opt[n], dp[n], fib[n]**

**3.状态的转移方程：opt[n] = best_of(opt[n - 1], opt[n - 2],...)**

**4.最优子结构**

### 斐波拉契：

![1568546813658](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568546813658.png)

### 找路径

![1568546969927](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568546969927.png)

![1568547148928](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568547148928.png)

上述会有重复计算，时间复杂度为**O(2^n）**添加记忆化

递推：从end 往 start 推，下面的一表示只能往下走或者只能往右走

![1568547694270](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568547694270.png)

递推出状态方程，时间复杂度为**O(M*N)**

~~~Java 
opt[i, j] = opt[i - 1, j] + opt[i, j - 1];
//          表示往下走的步数     往右走的步数
~~~

~~~Java
if(a[i, j] == "空地"){
	opt[i, j] = opt[i - 1, j] + opt[i, j - 1];
}else{
    opt[i, j] = 0;
}
~~~



![1568547966695](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568547966695.png)

3表示这一格有3种走法走到终点， 可以往右走，也可以往下走。往下走的话就只有一种走法，往右走有2种走法





![1568548526816](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568548526816.png)



## 70 爬楼梯

**题目**

~~~HTML
 假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
 * 
 * 每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
 * 
 * 注意：给定 n 是一个正整数。
 * 
 * 示例 1：
 * 
 * 输入： 2
 * 输出： 2
 * 解释： 有两种方法可以爬到楼顶。
 * 1.  1 阶 + 1 阶
 * 2.  2 阶
 * 
 * 示例 2：
 * 
 * 输入： 3
 * 输出： 3
 * 解释： 有三种方法可以爬到楼顶。
 * 1.  1 阶 + 1 阶 + 1 阶
 * 2.  1 阶 + 2 阶
 * 3.  2 阶 + 1 阶
~~~



**思路**

**思路一：回溯**

递推公式：

`f(n) = f(n - 1) + f(n - 2)`

`f(0) = f(1) = 1`



**思路二：动态规划**

**DP状态的定义**

**DP的状态方程**

`for: i = 2; i <= n:`

​	`f[n] = f[n - 1] + f[n -2]`



**解法一：递归 分治 回溯**

~~~Java
//提交超出时间限制
class Solution {
    public int climbStairs(int n) {
        if( n == 0){
            return 0;
        }
        if( n == 1){
            return 1;
        }
        if( n == 2){
            return 2;
        }
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
}
~~~



**解法二：动态规划**

**DP状态的定义**

**DP的状态方程**

~~~Java
/**
 * 时间复杂度O(N)
 */
class Solution {
    public int climbStairs(int n) {
        if(n <= 2){
            return n; 
        }
       
        int[] opt = new int[n];
        // 只有一阶梯
        opt[0] = 1;
        //只有两个阶梯
        opt[1] = 2;
        for(int i = 2; i < n; i++){
            opt[i] = opt[i - 1] + opt[i - 2];
        }
        return opt[n - 1];
    }
}
~~~



只用两个变量来存储前一步和前两步的操作，时间复杂度一样为O(N)

~~~Java
class Solution {
    public int climbStairs(int n) {
        if(n <= 2){
            return n; 
        }
       
        int oneStepBefore = 2;
        int towStepBefore = 1;
        int allCount = 0;
        for(int i = 2; i < n; i++){
            allCount = oneStepBefore + towStepBefore;
            // 更新前一步和前两步的数值
            // oneStepBefore变成了前两步
            towStepBefore = oneStepBefore;
            //前一步的操作更新为最新的allCount
            oneStepBefore = allCount;
        }
        return allCount;
    }
}
~~~



## 120  三角形最小路径和

**题目**

~~~html

 给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。
 * 
 * 例如，给定三角形：
 * 
 * [
 * ⁠    [2],
 * ⁠   [3,4],
 * ⁠  [6,5,7],
 * ⁠ [4,1,8,3]
 * ]
 * 
 * 
 * 自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
~~~



**解题思路**

**思路一：DFS**

时间复杂度O(2^n)

triargle（i, j){

triargle(i + 1, j);//向左下相邻

triargle(i + 1, j + 1);//向右下相邻

}

**思路二：动态方程**



**解法一：DFS**

~~~Java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
       if(triangle.isEmpty()){
           return 0;
       }
       return DFS(triangle, 0, 0);



    }
    //终止条件，i大于等于层数，j 小于等于 每一行的列数
    public int DFS(List<List<Integer>> triangle, int i, int j){
        if( i >= triangle.size() || j >= triangle.get(i).size()){
            return 0;
        }

        //分治
        int left = DFS(triangle, i + 1, j);
        int right = DFS(triangle, i + 1, j + 1);

        //返回结果 左边和右边的最小值 + 当前值
        return Math.min(left, right) + triangle.get(i).get(j);
    }
}
~~~



****解法二：动态方程**

动态方程

**自底向上的实现**，即最后一组数字应该作为初始值，反推

定义状态：使用一个二维数组，记录当前每个节点的路径和 int/[]/[] dp

定义状态方程：**dp/[i]/[j] = Min(dp/[i + 1]/[j] ,  dp/[i + 1]/[j + 1] ) + trigle/[i]/[j]**

​					 Min(当前节点左下相邻节点 ,  当前节点右下相邻节点 ) + 当前本身节点

时间复杂度O(N^2)

~~~Java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {


        //获取最后一行作为初始值,注意size要减一
        List<Integer> lastList = triangle.get(triangle.size() - 1);
        //定义状态
        int[][] dp = new int[triangle.size()][lastList.size()];
        //定义状态初始值
        for(int i = 0; i < lastList.size(); i++){

            dp[triangle.size() - 1][i] = lastList.get(i);

        }

        //从底部开始遍历
        for(int i = triangle.size() - 2; i >= 0; i--){
            for(int j = triangle.get(i).size() - 1; j >= 0; j--){
                //动态方程
                // Min(当前节点左下相邻节点 ,  当前节点右下相邻节点 ) + 当前本身节点
                dp[i][j] = Math.min(dp[i + 1][j], dp[i + 1][ j + 1]) + triangle.get(i).get(j);
            }
        }
        return dp[0][0];
    }
}
~~~



## [152. 乘积最大子序列](https://leetcode-cn.com/problems/maximum-product-subarray/)

**题目**

~~~html
给定一个整数数组 nums ，找出一个序列中乘积最大的连续子序列（该序列至少包含一个数）。

示例 1:

输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
示例 2:

输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-product-
K著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
~~~



**思路一**

状态的定义、状态方程

~~~HTML
DP【i】下标为0的位置到 I 的位置， 表示走到第i个时的最大值，包含当前的i的最大值，不包括前面的全部，可能是<Strong>0-i的部分，只是一定包含i。</Strong>dp【n - 1】， dp【n - 2】 。。。dp【0】

Dp【i + 1】 = dp【i】 * a【i + 1】（有正负，所以定义错误）

​                          Max   *     正

​                          min   *     负

改写：

DP【i】【2】（0Max,1min），二维数组保存，Dp【i】表示0 ---- i 当前之间的最大值或负的最大值，第二维表示正负，负的只是中间过程产物，最终的结果还是最大值

dp【i】【0】 = 

​			if（a【i】>= 0): dp【i - 1】【0】 * a【i】

​			else: dp【i - 1】【0】 * a【 i】

dp【i】【0】 = 

​			if（a【i】>= 0): dp【i - 1】【1】 * a【i】

​			else: dp【i - 1】【1】 * a【 i】

最大最小值还需要和自身做比较
~~~



**思路二:递归**

每次都带上最大和最小值两个状态，当递归到数组末尾，返回，返回过程中始终记录最大值



解法一：DP

~~~Java

class Solution {
    public int maxProduct(int[] nums) {
        if(nums.length == 0){
            return 0;
        }
        int[][] dp = new int[nums.length][2];//状态定义，第二维是表示当前状态是正的最大值还是负的最大值,1是负的最大值，0是正的最大值
        //初始化值
        dp[0][0] = nums[0];
        dp[0][1] = nums[0];
        //结果值
        int result = nums[0];
        
        for(int i = 1; i < nums.length; i++){
            //当前值大于0
            if(nums[i] >= 0){
                //状态方程
                dp[i][0] = Math.max(nums[i], dp[i - 1][0] * nums[i]);// 正的最大值 * 正的 = 正的最大值
                dp[i][1] = Math.min(0, dp[i -1][1] * nums[i]);//负的最大值 * 正的 = 负的最大值
            }else{
                dp[i][0] = Math.max(nums[i], dp[i - 1][1] * nums[i]);//负的最大值 * 负的 = 正的最大值
                dp[i][1] = Math.min(nums[i], dp[i -1][0] * nums[i]);//正的最大值 * 负的 = 负的最大值
            }
            //每一轮遍历都需要判断其最大值
            result = Math.max(dp[i][0], result);

        }

        return result;
    }
}
~~~



**解法二：**

每次都带上最大和最小值两个状态，当递归到数组末尾，返回，返回过程中始终记录最大值



~~~Java
    private int result  = Integer.MIN_VALUE;
    public int maxProduct(int[] nums) {
        if(nums.length == 0){
            return 0;
        }
        DFS(0, 1, 1, nums);
        return  result;
    }
    public void DFS(int level, int positive, int negative, int[] nums){
        //终止条件
        if(nums.length == level){
            return;
        }
        if(nums[level] >= 0){
            positive = positive * nums[level];//记录正的最大值
            negative = negative * nums[level];//记录最小值
            if(nums[level] > positive){
                positive = nums[level];//如果最大值比当前值小，记得交换
            }

        }else{
            int temp = positive * nums[level];//中间变量，下面要交换相乘
            positive = negative * nums[level];//负的最大值 * 负的 = 正的最大值
            negative = temp;
            if(nums[level] < negative){
                negative = nums[level];//如果最小值比当前值大，记得交换
            }
        }
        result = Math.max(result, positive);
        DFS(level + 1, positive, negative, nums);
    }
~~~



## 股票买卖系列

## [121] 买卖股票的最佳时机 （只能买卖一次）

**题目**

~~~HTML
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

 \* 如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

 \* 注意你不能在买入股票前卖出股票。



 \* 示例 1:

 \* 输入: [7,1,5,3,6,4]

 \* 输出: 5

 \* 解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。

 \* ⁠    注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。


示例 2:

 \* 输入: [7,6,4,3,1]

 \* 输出: 0

 \* 解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
~~~

**思路一：**

遍历一次，比较记录最小值，判断每一个数和最小值的相差值，如果比原来记录的大，则替换

**思路二：DP**



**解法一：**

~~~java
//遍历一次，比较记录最小值，判断每一个数和最小值的相差值，如果比原来记录的大，则替换
class Solution {
    public int maxProfit(int[] prices) {

      if(prices.length == 0){
        return 0;
      }
      int minI  = prices[0];//记录数组中的最小值
      int max = 0;//记录结果最大值
      for(int i = 1; i < prices.length; i++){
        
        if(prices[i] < minI){//如果当前的数字小于最小值，则设置为最小值

          minI = prices[i];
        }
        if(prices[i] - minI > max){//如果当前数 - 最小值 》 大于最大值，则赋值为最大值
          max = prices[i] - minI;
        }

      }

      return max;

    }
}

~~~

**解法二：****DP**



~~~Java
class Solution {
    public int maxProfit(int[] prices) {

      if(prices.length == 0){
        return 0;
      }

    //最多只能买卖一次，不能持有多分股票， 
    /**
     * 一、定义状态
     * MP[][]表示第 0 ---》i 天一共交易了 一次  获得的最大利益
     * MP[i][j]: i是当前数组的下标，j是表示当前状态手上是否持有股票 0 无股票，1 有股票  2完成一次交易
     * 
     * 二、状态方程：
     * MP[i][0] = MP[i - 1][0] 第i天，手上没有股票  = 昨天手上也没有股票
     * 
     * MP[i][1] = Max(MP[i - 1][1], MP[i - 1][0] - prices[i])  第i天，手上有股票 = 昨天手上有股票 亦或 昨天手上没有股票，今天买入今天的股票份额
     * 
     * 
     * MP[i][2] = Max( MP[i - 1][1] + prices[i], MP[i - 1][2])第i天，已经完成了一次交易（买入、卖出） 等于昨天完成一次交易  和  昨天手上有股票，并且在今天卖出
     * 
     * 
     * 三、初始值
     * MP[0][0] = 0;//第一天，并且没有股票，利润为0
       MP[0][1] = -prices[0];//第一天，手上有股票，说明买了第一个股票
       MP[0][2] = 0;//第一天，表示当天买了股票，并且卖了出去，完成一次交易
     */

     int result = 0;
     //state
     int[][] MP = new int[prices.length][3];

     //init
     MP[0][0] = 0;
     MP[0][1] = -prices[0];
     MP[0][2] = 0;
     
     //for循环遍历
     for(int i = 1; i < prices.length; i++){
      MP[i][0] = MP[i - 1][0];
 
       MP[i][1] = Math.max(MP[i - 1][1], MP[i -1][0] - prices[i]);

       MP[i][2] = Math.max( MP[i - 1][1] + prices[i], MP[i - 1][2]);

       //最后的结果肯定是手上无股票 或者是 股票完成一一次交易  的最大值
       result = Math.max(Math.max(MP[i][0], MP[i][2]), result);
     }
     return result;

    }


}
~~~



## 122 买卖股票的最佳时机  买卖无数次

**题目**

~~~HTML
 给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
 * 设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。
 * 注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
 * 示例 1:
 * 输入: [7,1,5,3,6,4]
 * 输出: 7
 * 解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
 * 随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。

 * 示例 2:
 * 输入: [1,2,3,4,5]
 * 输出: 4
 * 解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4
 * 。
 * 注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
 * 因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。

 * 示例 3:
 * 输入: [7,6,4,3,1]
 * 输出: 0
 * 解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
~~~



**解题思路**

思路一：贪心算法



**解法**

**解法一：贪心算法**

~~~Java
class Solution {
    /**
     * 贪心解法
     * 因为可以无限交易，所以只要发现明天的比今天的大，就可以抛出
     * @param prices
     * @return
     */
    public int maxProfit(int[] prices) {
        if(prices.length == 0){
            return 0;
          }
        int result = 0;
        
        for(int i = 0; i < prices.length - 1; i++){
            if(prices[i] < prices[ i+ 1]){
                result += prices[i + 1] - prices[i];
            }
        }
        return result;
       
        
    }
}
~~~



## 123 买卖股票的最佳时机 买卖2次

**题目**

~~~html 
给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。
 * 
 * 设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。
 * 
 * 注意: 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
 * 
 * 示例 1:
 * 
 * 输入: [3,3,5,0,0,3,1,4]
 * 输出: 6
 * 解释: 在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
 * 随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。
 * 
 * 示例 2:
 * 
 * 输入: [1,2,3,4,5]
 * 输出: 4
 * 解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4
 * 。   
 * 注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。   
 * 因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
 * 
 * 
 * 示例 3:
 * 
 * 输入: [7,6,4,3,1] 
 * 输出: 0 
 * 解释: 在这个情况下, 没有交易完成, 所以最大利润为 0。
~~~



**解题思路**

思路一：DP动态规划

1，定义状态

//MP【i】【k】【j】 k表示交易次数， j表示当前的手上的股票状态(0无股票，1有股票，2完成一次交易)

  int[][]【i】【k】【j】 MP = new int[【prices.length】【3】【3】；

2，状态方程

 MP【i】【k】【j】= MP【i - 1】【k】【j】；

3，初始值

~~~java 
MP[0][0][0] = 0;
MP[0][0][1] = -prices[0];
MP[0][1][0] =0;//一次交易
MP[0][1][1] = -prices[0];
MP[0][2][0] = 0;//第二次交易，第一天的第二次交易
~~~



4，最优解





**解法一：**

~~~Java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 0){
            return 0;
        }
        int result = 0;
         //DP state
         //MP【i】【k】【j】 k表示交易次数， j表示当前的手上的股票状态(0无股票，1有股票，2完成一次交易)
         int[][][] MP = new int[prices.length][3][3]; 
         //初始值
         MP[0][0][0] = 0;
         MP[0][0][1] = -prices[0];
         MP[0][1][0] =0;//一次交易
         MP[0][1][1] = -prices[0];
         MP[0][2][0] = 0;//第二次交易，第一天的第二次交易

         for(int i = 1; i < prices.length; i++){
             MP[i][0][0] = MP[i - 1][0][0];//手上无股票，昨天手上也无股票
             MP[i][0][1] = Math.max(MP[i - 1][0][1], MP[i -1][0][0] - prices[i]);//之前有股票 或 昨天无股票，今天买入            
             MP[i][1][0] = Math.max(MP[i - 1][1][0], MP[i - 1][0][1] + prices[i]);//之前已经完成了一次交易，并且无股票  或 之前没有完成一次交易，手上有股票，这i天卖出
             MP[i][1][1] = Math.max(MP[i - 1][1][1], MP[i -1][1][0] - prices[i]);//之前已经完成了一次交易并且手上有股票 = 之前完成了一次交易并且手上有股票 或 之前完成了一份交易，当前手上无股票，现在买入
             MP[i][2][0] = Math.max(MP[i -1][2][0],MP[i -1][1][1] + prices[i]);//第二完成交易 = 之前一次手上有股票并卖出 + 之前一次已经完成了两次交易
             
             result = Math.max(Math.max(Math.max(Math.max(MP[i][0][0], MP[i][0][1]), Math.max(MP[i][1][0], MP[i][1][1])), MP[i][2][0]), result); 
            } 
         return result;  
    }
}
~~~





## 188 【买卖股票最佳时机 买卖k次】



~~~HTML
 * 给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。
 * 
 * 设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。
 * 
 * 注意: 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
 * 
 * 示例 1:
 * 
 * 输入: [2,4,1], k = 2
 * 输出: 2
 * 解释: 在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
 * 
 * 
 * 示例 2:
 * 
 * 输入: [3,2,6,5,0,3], k = 2
 * 输出: 7
 * 解释: 在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4
 * 。
 * 随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。
 * 
~~~





**解法：动态规划**

~~~Java
    /**
     * 一、定义状态
     * MP[i][j][k]:表示第 0 ---》i 天一共交易了 k  获得的最大利益
     * : i是当前数组的下标，j是表示当前状态手上是否持有股票 0 无股票，1 有股票  
     * 
     * 二、状态方程：
     *
     mp[i][0][kk] = mp[i - 1][0][kk];//kk为0 j为0，即交易0次 手上无股票 等于 之前也交易0次并且手上无股票
     
     mp[i][0][kk] = Math.max(mp[i - 1][0][kk], mp[ i - 1][1][kk - 1] + prices[i]);//kk不为0，即有交易，手上没股票 = 手上之前有股票，在这一天卖出 或 之前手上本来就没股票
     
      mp[i][1][kk] = Math.max(mp[i - 1][1][kk], mp[i - 1][0][kk] - prices[i]);//手上有股票 = 手上之前没有股票，并买入股票 或 之前手上有股票
     * 
     * 三、初始值
     //inint，设置初始值
        for(int kk = 0; kk <= k; kk++){
            mp[0][0][kk] = 0;//第一天 买入-卖出 循环K次之后不再买入，初始值为0
            mp[0][1][kk] = -prices[0];//第一天 买入-卖出 循环k次之后又买入，所以初始值为 -prices[0]
        }
     */

class Solution {
    public int maxProfit(int k, int[] prices) {
        if (prices == null || prices.length == 0)
            return 0;
        
        int result = 0;

        if(k > prices.length / 2){
            return greedy(prices);
        }
        //state
        int[][][] mp = new int[prices.length][3][k + 1];
        //inint
        for(int kk = 0; kk <= k; kk++){
            mp[0][0][kk] = 0;//第一天 买入-卖出 循环K次之后不再买入，初始值为0
            mp[0][1][kk] = -prices[0];//第一天 买入-卖出 循环k次之后又买入，所以初始值为 -prices[0]
        }
        for(int i = 1; i < prices.length; i++){
            for(int kk = 0; kk <= k; kk++){
                if(kk == 0){
                    mp[i][0][kk] = mp[i - 1][0][kk];//kk为0，即交易0次，所以等于 之前也交易0次
                }else{
                    mp[i][0][kk] = Math.max(mp[i - 1][0][kk], mp[ i - 1][1][kk - 1] + prices[i]);//kk不为0，即有交易，手上没股票 = 手上之前有股票，在这一天卖出 或 之前手上本来就没股票
                }

                mp[i][1][kk] = Math.max(mp[i - 1][1][kk], mp[i - 1][0][kk] - prices[i]);//手上有股票 = 手上之前没有股票，并买入股票 或 之前手上有股票

                result = Math.max(Math.max(mp[i][0][kk], mp[i][1][kk]), result);

            }
            
        }

        return result;
    }
    
    
    public int greedy(int[] prices){
        int reuslt = 0;
        for(int i = 0; i < prices.length - 1; i++){
            if(prices[i] < prices[i + 1]){
                reuslt += prices[i + 1] - prices[i];
            }
        }
        return reuslt;

    }
        
}
~~~



## 300 最长上升子序列

**题目**

~~~HTML
 * 给定一个无序的整数数组，找到其中最长上升子序列的长度。
 * 
 * 示例:
 * 
 * 输入: [10,9,2,5,3,7,101,18]
 * 输出: 4 
 * 解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
 * 
 * 说明:
 * 
 * 
 * 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
 * 你算法的时间复杂度应该为 O(n^2) 。
 * 
 * 
 * 进阶: 你能将算法的时间复杂度降低到 O(n log n) 吗?
~~~



**思路**

思路一：DFS + 剪枝

思路二：动态规划

dp[i]表示以**i**为结尾的最长上升序列的长度 ，要和**dp[j]+1**进行比较，**j在0-（i-1）的范围**

**解法**

**解法二**：

动态规划

1，定义状态

dp【i】表示当前数组下标为i的最长上升子序列

2，动态方程

dp[i] = dp[j] + 1;

j的范围：0 ----->i - 1, 并且dp[j] < dp[i]

3，最优解

最优解并不是最后一个n - 1，而是0 ----> n之间的最大值

![img](https://img-blog.csdnimg.cn/20190605210215350.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l5c2F2ZQ==,size_16,color_FFFFFF,t_70)

~~~java
class Solution {
    public int lengthOfLIS(int[] nums) {
       if(nums.length == 0){
           return 0;
       }
        
        //定义状态
        int[] dp = new int[nums.length];

        //初始值,第一位的长度为1
        dp[0] = 1;

        int result = dp[0];

        //状态方程
        for(int i = 1; i < nums.length; i++){
            //当前这个数的长度为1
            dp[i] = 1;
            //从0----> i - 1 判断在i之前的最大子序列
            for(int j = 0; j < i; j++){
                //dp[j] + 1 是因为要加上当前的自己本身这个数的长度，所以加1
                //如果当前的值大于当前之前的值，并且 dp【i】<dp[j] + 1
                if(nums[j] < nums[i] && dp[i] < dp[j] + 1){
                    dp[i] = dp[j] + 1;
                }
                //result存储每一次的结果值，如果当前的dp[i] > result ,则把当前值赋值给result
                result = Math.max(dp[i], result);
            }
            
        }
        return result;
        
    }

}
~~~



## 322 零钱兑换

题目

~~~HTML
 * 给定不同面额的硬币 coins 和一个总金额
 * amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。
 * 
 * 示例 1:
 * 
 * 输入: coins = [1, 2, 5], amount = 11
 * 输出: 3 
 * 解释: 11 = 5 + 5 + 1
 * 
 * 示例 2:
 * 
 * 输入: coins = [2], amount = 3
 * 输出: -1
 * 
 * 说明:
 * 你可以认为每种硬币的数量是无限的。
~~~



解题思路

思路一：暴力求解

思路二：**动态规划**

转变思路，转变成爬楼梯问题，**硬币的的面值**就变成能**爬楼梯的步数**，要**拼凑的金额**变成**一共多少级阶梯**

注意：**Integer.MAX_VALUE + 1会溢出，变成最小值**

**解法**



**解法二：**

1，定义状态

 //定义状态dp【i】上到i阶的最小值
        **int[] dp = new int[amount + 1];**

2，状态方程

if（i >= coins[i]）:

**dp[i] = Math.min(dp[i - coins[j]] + 1, dp[i]);**

3,最优解

 //如果最后一个和Integer.MAX_VALUE - 1相等，说明无解
        return **dp[amount] == Integer.MAX_VALUE - 1 ? -1 : dp[amount];**

~~~Java
/**
 * 装换成爬楼梯问题
 * amount就是需要爬的n阶楼梯
 * coins就是可以行走的步数
 */
class Solution {
    public int coinChange(int[] coins, int amount) {
        if(coins.length == 0){
            return -1;
        }
        //定义状态dp【i】上到i阶的最小值
        int[] dp = new int[amount + 1];
        //数组默认初始值为0
        //状态方程
        for(int i = 1; i <= amount; i++){
            //Integer.MAX_VALUE + 1会溢出，变成最小值
            dp[i] = Integer.MAX_VALUE - 1;
            for(int j = 0; j < coins.length; j++){
                //当i大于等于coins的面值时才可以进行运行
                if(i >= coins[j]){
                    //记录当前的最小值
                    dp[i] = Math.min(dp[i - coins[j]] + 1, dp[i]);
                }else{

                }
                
            }
        }
        //如果最后一个和Integer.MAX_VALUE - 1相等，说明无解
        return dp[amount] == Integer.MAX_VALUE - 1 ? -1 : dp[amount];

        
    }
}
~~~



## 72 编辑距离

 **\* 字符串匹配 动态规划解法的入门题**

 **\* 经典题，常考题**

题目

~~~html
 * 给定两个单词 word1 和 word2，计算出将 word1 转换成 word2 所使用的最少操作数 。
 * 
 * 你可以对一个单词进行如下三种操作：
 * 
 * 
 * 插入一个字符
 * 删除一个字符
 * 替换一个字符
 * 
 * 
 * 示例 1:
 * 
 * 输入: word1 = "horse", word2 = "ros"
 * 输出: 3
 * 解释: 
 * horse -> rorse (将 'h' 替换为 'r')
 * rorse -> rose (删除 'r')
 * rose -> ros (删除 'e')
 * 
 * 
 * 示例 2:
 * 
 * 输入: word1 = "intention", word2 = "execution"
 * 输出: 5
 * 解释: 
 * intention -> inention (删除 't')
 * inention -> enention (将 'i' 替换为 'e')
 * enention -> exention (将 'n' 替换为 'x')
 * exention -> exection (将 'n' 替换为 'c')
 * exection -> execution (插入 'u')
~~~



解题思路

思路一：DFS

思路二：BFS

思路三：DP动态规划



**解法**

**解法三**

我们定义函数 **f(i, j)**表示表示**第一个字符串的第i个位置**与**第二个字符串的第j个位置**转换的需要的**最少步骤**

 ~~~html
 1、定义状态

​    dp[i][j] 表示第一个字符串的第i个位置与第二个字符串的第j个位置操作几次到达

  2、状态方程

​    如果w1(i) == w2(j) 则不用处理，即等于上一次的操作数 dp[i][j] = dp[i - 1][j - 1]

​    

​    不相等就有三种情况

​    插入：w1中的i的位置不变，而w2中的j要向前一个位置，即f(i, j) = f(i, j - 1) + 1。

​    删除：删除的话，w1中会减少一个需要比较的元素，而w2中的j的位置不变f(i, j) = f(i - 1, j) + 1

​    

​    替换：replace会减少word1和word2中一个需要比较的元素（i和j会向前挪一个位置），也就是f(i,j)=f(i−1,j−1)+1 

  3、最优解

​    就是最后的dp[m][n]
 ~~~



~~~Java
 /**
  * 动态规划：
  * 
  1、定义状态
    dp[i][j] 表示第一个字符串的第i个位置与第二个字符串的第j个位置操作几次到达

  2、状态方程
    如果w1(i) == w2(j) 则不用处理，即等于上一次的操作数 dp[i][j] = dp[i - 1][j - 1]
    
    不相等就有三种情况

    插入：w1中的i的位置不变，而w2中的j要向前一个位置，即f(i, j) = f(i, j - 1) + 1。

    删除：删除的话，w1中会减少一个需要比较的元素，而w2中的j的位置不变f(i, j) = f(i - 1, j) + 1
    
    替换：replace会减少word1和word2中一个需要比较的元素（i和j会向前挪一个位置），也就是f(i,j)=f(i−1,j−1)+1 

  3、最优解
    就是最后的dp[m][n]
  */
class Solution {
    public int minDistance(String word1, String word2) {
        int w1Length = word1.length();
        int w2Length = word2.length();
        //当有一个字符串为空时
        if(w1Length * w2Length == 0){
            return w1Length > w2Length ? w1Length : w2Length;
        }
        //定义状态
        int[][] dp = new int[w1Length + 1][w2Length + 1];
        //初始值
        for(int i = 1; i <= w1Length; i++){
            //初始化,j为0，则要进行i次的操作
            dp[i][0] = i;
            for(int j = 1; j <= w2Length; j++){
                //初始化,i为0，则要进行j次的操作
                dp[0][j] = j;
                if(word1.charAt(i - 1) == word2.charAt(j - 1)){
                    dp[i][j] = dp[i - 1][j -1];
                }else{
                    dp[i][j] = Math.min(Math.min(dp[i][j - 1], dp[i -1][j]), dp[i - 1][j - 1]) + 1;
                }
            }
        }
        return dp[w1Length][w2Length];
        
    }
}
~~~


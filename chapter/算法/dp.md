##dp 动态规划

**最长回文子串**

给你一个字符串 s，找到 s 中最长的回文子串。

	输入：s = "babad"
	输出："bab"
	解释："aba" 同样是符合题意的答案。

	class Solution {
    public String longestPalindrome(String s) {
         int[][] dp=new int[s.length()][s.length()];

         int m=0,n=0;
         int max=1;
         
         for(int i=s.length()-1;i>=0;i--){
             for(int j=i;j<s.length();j++){
                 if(i==j){
                     dp[i][j]=1;
                 }else if(j==i+1){
                     dp[i][j]=s.charAt(i)==s.charAt(j)?1:0;
                 }else{
                     if(dp[i+1][j-1]==1 &&s.charAt(i)==s.charAt(j)){
                        dp[i][j]=1;
                     }else{
                        dp[i][j]=0;
                     }
                     
                 }
                 if(max<j-i+1 && dp[i][j]==1){
                     max=j-i+1;
                     m=i;
                     n=j;
                 }
             }
         }
         return s.substring(m,n+1);

    }

	}


**无重复字符的最长子串**

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度

	输入: s = "abcabcbb"
	输出: 3 
	解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

	class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s.length()<=1)   return s.length();
        int[] dp=new int[s.length()];
        dp[0]=1;
        int max=1;
        for(int i=1;i<s.length();i++){
            Set<Character> set=new HashSet<>();
            if(s.charAt(i)==s.charAt(i-1)){
                dp[i]=1;
            }else{
                dp[i]=getNum(s,i,set);
            }
            if(dp[i]>max){
                max=dp[i];
            }
        }
        return max;
    }

    public int getNum(String s,int i,Set<Character> set){
        int count=1;
        set.add(s.charAt(i));
        i-=1;
        while(i>=0){
            if(set.contains(s.charAt(i))){
                break;

            }else{
                set.add(s.charAt(i));
                count++;
                i--;
            }
        }
        return count;
    }
	}


**股票问题**

给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

	输入：[7,1,5,3,6,4]
	输出：5
	解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。

	
	class Solution {
    public int maxProfit(int[] prices) {
        int[][] dp=new int[prices.length][2];
        dp[0][0]=0;
        dp[0][1]=-prices[0];
        for(int i=1;i<prices.length;i++){
            dp[i][0]=Math.max(dp[i-1][0],dp[i-1][1]+prices[i]);
            dp[i][1]=Math.max(-prices[i],dp[i-1][1]);
        }
        return dp[prices.length-1][0];
    }
	}


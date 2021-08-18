# Dynamic Programming
## Maximum Product Subarray
Given an integer array nums, find a contiguous non-empty subarray within the array that has the largest product, and return the product.
### Solution 1: (wit restriction that each item diff is 1/-1)
```java
class Solution {
    public int maxProduct(int[] nums) {
        // iterate element in the array and when we find the element is not continuous, we cut it and calculate the product.
        // then move one element up and start the search again.'
        System.out.print(Arrays.toString(nums));
        int product = nums[0];
        int preNum = nums[0];
        int type = 0;
        for(int i = 1; i < nums.length; i++){
            int diff = nums[i] - preNum;
            System.out.println("diff="+diff+"|preNum="+preNum+"|nums[i]="+nums[i]);
            if(diff != 1 && diff != -1){
                // not continuous
                if(i < nums.length){
                    int nxtProduct = maxProduct(Arrays.copyOfRange(nums, i, nums.length));
                    System.out.print("child prod="+nxtProduct+"current prod="+product);
                    if(nxtProduct > product){
                        product = nxtProduct;
                    }
                }
                System.out.println("return="+product);
                return product;
                
            }else{
                if(type == 0){
                    type = diff;
                }
                if(type == diff){
                    product = product * nums[i];
                }
            }
        }
        System.out.println("return="+product);
        return product;
    }
}
```
## Solution 2:
```java
class Solution {
    public int maxProduct(int[] nums) {
        int ans = nums[0];
        int p = 0;
        for (int i = 0; i < nums.length; i++) {
            if (p == 0) {
                p = nums[i];
            } else {
                p *= nums[i];
            }
            ans = Math.max(ans, p);
        }
        p = 0;
         for (int i = nums.length - 1; i >= 0; i--) {
            if (p == 0) {
                p = nums[i];
            } else {
                p *= nums[i];
            }
            ans = Math.max(ans, p);
        }
        return ans;
    }
}
```

## Decode Ways
A message containing letters from A-Z can be encoded into numbers using the following mapping:
o decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, "11106" can be mapped into:

* "AAJF" with the grouping (1 1 10 6)
* "KJF" with the grouping (11 10 6)
* 
Note that the grouping (1 11 06) is invalid because "06" cannot be mapped into 'F' since "6" is different from "06".
Given a string s containing only digits, return the number of ways to decode it.

### Solution 1: Dynamic Programming Bottom Up
```java
class Solution {
    public int numDecodings(String s) {
        // dynamic programming bottom up
        // we consider base cases and pill it up to solve the bigger problem
        // dp is used to store how many combinations it is available for each index length of string
        
        int[] dp = new int[s.length()+1];
        dp[0] = 1; //if s length is 0, there is only one way
        dp[1] = s.charAt(0) == '0' ? 0 : 1; //if s length is 1, we need to check the value
        for(int i = 2; i <= s.length(); i++){
            // i represents the length of string.
            // get current char
            int num1 = Integer.valueOf(s.substring(i-1, i)); 
            // get the number that can be formed with current char
            int num2 = Integer.valueOf(s.substring(i-2, i));
            if(num1 != 0){
                // since previous char is valid number, we add that combinations to current case.
                dp[i] += dp[i-1];
            }
            if(num2 >= 10 && num2 <= 26){
                // since previous two chars is valid number, we add that combinations
                dp[i] += dp[i-2];
            }
        }
        return dp[s.length()];
    }  
}
```

## Best Time to Buy and Sell Stock with Cooldown
You are given an array prices where prices[i] is the price of a given stock on the ith day.
Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

* After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

### Solution:
```java
class Solution {
    public int maxProfit(int[] prices) {

        int[][] dp = new int[prices.length][2];
        // for each day, you either posess a stock or not so we have two dimensional array
        // case 1 - has a stock on day i - dp[i][1]
        // 1. bought the stock at day i => dp[i][1] = dp[i-2][0] - prices[i]
        //    since we bought a stock from the money you gained, we subtract it and store how much money we own
        //    in our pocket. We also need to think about cooldown day so the value we get from is i-2 days ago.
        // 2. carry forward stock
        //    we did not buy any stock or sell it so we just carry forward we have from previous day
        //    dp[i][1] = dp[i-1][1]
        // case 2 - does not have a stock on day i - dp[i][0]
        // 1. sold the stock at day i
        //    If we sold the stock on day i, we would gain the money so we add it to our pocket.
        //    dp[i][0] = dp[i-1][1] + prices[i]. Note that in case 1, we have subtracted the amount of money we           //    used to buy the stock so we just need to add the price back to our pocket money to gain the profit.
        // 2. sold the stock on other day prior to day i
        //    we did not do anything on day i so we just carry the value
        //    dp[i][0] = dp[i-1][0];
        if(prices.length <= 1){
            return 0;
        }

        // note index 0 represents the day 1.

        for(int i = 0; i < prices.length; i++){
            if(i == 0){
                // for day 1, we have not make any profit so our pocket does not have any money.
                // if we does not buy any stock, we does not have any money
                // if we buy a stock, we will get a debt
                dp[i][0] = 0;
                dp[i][1] = -prices[i];
            }else{
                // for day i, we either sell the stock today or not.
                dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);

                if(i-2 < 0){
                    //out of range
                    dp[i][1] = Math.max(dp[i-1][1], -prices[i]);
                }else{
                    dp[i][1] = Math.max(dp[i-1][1], dp[i-2][0] - prices[i]);
                }
            }
        }
        return dp[prices.length-1][0];
    }
}
```

## Climbing Stairs
You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

```java
class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n+1];
        //base case
        dp[0] = 0; //there is no step so no way to climb
        dp[1] = 1; //there is only one step so only one way to climb
        
        
        for(int i = 2; i <= n; i++){
            if(i == 2){
                dp[i] = dp[i-1] + dp[i-2] + 1;
            }else{
                dp[i] = dp[i-1] + dp[i-2];
            }
        }
        return dp[n];
    }
}
```

## BestTime to Buy and Sell Stock
You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.
### Solution:
```java
public class Solution {
    public int maxProfit(int prices[]) {
        int minprice = Integer.MAX_VALUE;
        int maxprofit = 0;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < minprice)
                minprice = prices[i];
            else if (prices[i] - minprice > maxprofit)
                maxprofit = prices[i] - minprice;
        }
        return maxprofit;
    }
}
```

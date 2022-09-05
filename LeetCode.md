LeetCode 热题 HOT 100

#### [1. 两数之和](https://leetcode.cn/problems/two-sum/)

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

示例1

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

解析：（相同元素指两个值相同的元素）

构建哈希表，对于元素和元素的下标形成了唯一的映射，元素作为哈希表的键，下标作为哈希表的值。数组中可能存在相同的元素，这两个相同元素和为目标值，如果先将所有元素添加到哈希表中，会覆盖掉相同元素的较小的下标，无法获取结果。所以应该每次添加元素时，先判断，此时哈希表的键和当前要添加的元素之和是否满足目标值，如果满足，则返回当前元素的下标，和哈希表该键对应的值。

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] arr=new int[2];
        HashMap<Integer,Integer> map=new HashMap<>();
        int N=nums.length;
        for(int i=0;i<N;i++){
            if(map.containsKey(target-nums[i])){
                arr[0]=i;
                arr[1]=map.get(target-nums[i]);
                return arr;
            }
            map.put(nums[i],i);
        }
        return arr;
    }
}

```

#### [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

解析：

构建一个新的ListNode，对原有两个链表节点取值求和，值可能大于等于10，链表可以存储的值为0-9，设置一个int整形存储当大于等于10时，carry赋值为1，下一个节点求和时进行求和加上carry。（模拟10进制）。最后两个节点相加后可能大于等于10，最后要判断carry

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int carry =0;
        ListNode node=new ListNode(0);
        ListNode head=node;
        int val=0;
        while(l1!=null &&l2!=null){
            val=l1.val+l2.val+carry;
            node.next=new ListNode(val%10);
            node=node.next;
            carry=val/10;
            l1=l1.next;
            l2=l2.next;
        }
        if(l1==null){
            while(l2!=null){
                val=l2.val+carry;
                node.next=new ListNode(val%10);
                node=node.next;
                carry=val/10;
                l2=l2.next;
            }
        }else{
            while(l1!=null){
                val=l1.val+carry;
                node.next=new ListNode(val%10);
                node=node.next;
                carry=val/10;
                l1=l1.next;
            }
        }
        if(carry==1){
            node.next=new ListNode(1);
        }
        return head.next;
    }
}
```

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int carry =0;
        ListNode node=new ListNode(0);
        ListNode head=node;
        int val=0;
        while(l1!=null || l2!=null){
            int n1 = l1 != null ? l1.val : 0;
            int n2 = l2 != null ? l2.val : 0;
            val=n1+n2+carry;
            node.next=new ListNode(val%10);
            node=node.next;
            carry=val/10;
             if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }
        }
        if(carry==1){
            node.next=new ListNode(1);
        }
        return head.next;
    }
}
```

#### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

解析：

滑动窗口

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int N=s.length();
        HashMap<Character,Integer> map=new HashMap<>();
        int left=0;
        int maxlen=0;
        for(int right=0;right<N;right++){
            map.put(s.charAt(right),map.getOrDefault(s.charAt(right),0)+1);
            while(map.get(s.charAt(right))>1){
                map.put(s.charAt(left),map.get(s.charAt(left))-1);
                left++;
            }
            maxlen=Math.max(maxlen,right-left+1);
        }
        return maxlen;
    }
}
```

#### [4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)

#### [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

解析：

中心扩展

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s==null ||s.length()<1){
            return "";
        }
        int start=0,end=0;
        for(int i=0;i<s.length();i++){
            int len1=expandAroundCenter(s,i,i);
            int len2=expandAroundCenter(s,i,i+1);
            int len = Math.max(len1, len2);
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }
         return s.substring(start, end + 1);
    }

    public int expandAroundCenter(String s,int left,int right){
        while(left>=0 && right<s.length() && s.charAt(left)==s.charAt(right)){
            left--;
            right++;
        }
        return right-left-1;
    }
}
```

#### [10. 正则表达式匹配](https://leetcode.cn/problems/regular-expression-matching/)

#### [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

给定一个长度为 n 的整数数组 height 。有 n 条垂线，第 i 条线的两个端点是 (i, 0) 和 (i, height[i]) 。

找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量

解析：

双指针，从数组两面开始遍历，底现在最大，高为数组两面的最小值，得出此时的最大值，移动双指针，问题移动那边，如果移动数组两面的最大值，则下一次的值必定比现在的值更小。因为此时底变小了，高只有不变和变小两种可能，所以不应该移动数组两面的最大值，而应该移动最小值。

```java
class Solution {
    public int maxArea(int[] height) {
        int left=0,right=height.length-1;
        int maxArea=0;
        while(left<right){
            maxArea=Math.max(maxArea,(right-left)*Math.min(height[left],height[right]));
            if(height[left]<height[right]){
                left++;
            }else{
                right--;
            }
        }
        return maxArea;
    }
}
```

#### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy=new ListNode(-1);
        dummy.next=head;
        ListNode second=dummy;
        ListNode first=head;
        for(int i=0;i<n;i++){
            first=first.next;
        }
        while(first!=null){
            second=second.next;
            first=first.next;
        }
        second.next=second.next.next;
        return dummy.next;
    }
}
```

#### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
每个右括号都有一个对应的相同类型的左括号。

```java
class Solution {
    public boolean isValid(String s) {

        int N=s.length();
        Stack<Character> stack=new Stack<>();
        for(int i=0;i<N;i++){
            if(s.charAt(i)=='(' || s.charAt(i)=='[' || s.charAt(i)=='{'){
                stack.push(s.charAt(i));
            }else{
                if(stack.empty()){
                    return false;
                }
                if(stack.peek()=='(' &&s.charAt(i)==')' ){
                    stack.pop();
                    continue;
                }else if(stack.peek()=='[' &&s.charAt(i)==']' ){
                    stack.pop();
                    continue;
                }else if(stack.peek()=='{' &&s.charAt(i)=='}' ){
                    stack.pop();
                    continue;
                }
                return false;
            }
        }
        if(stack.empty()){
            return true;
        }
        return false;
    }
}
```

#### [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode head=new ListNode(-1);
        ListNode pre=head;
        while(list1 !=null && list2 !=null){
            if(list1.val<=list2.val){
                pre.next=list1;
                list1=list1.next;
            }else{
                pre.next=list2;
                list2=list2.next;
            }
            pre=pre.next;
        }
        pre.next=list1==null?list2:list1;
        return head.next;
    }
}
```

#### [48. 旋转图像](https://leetcode.cn/problems/rotate-image/)

给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        // 水平翻转
        for (int i = 0; i < n / 2; ++i) {
            for (int j = 0; j < n; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - i - 1][j];
                matrix[n - i - 1][j] = temp;
            }
        }
        // 主对角线翻转
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
    }
}
```

#### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

```java
class Solution {
    public int maxSubArray(int[] nums) {
       int pre=0;
       int maxSum=nums[0];
       for(int x:nums){
           pre=Math.max(pre+x,x);
           maxSum=Math.max(maxSum,pre);
       }
       return maxSum;
    }
}
```

#### [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)

给定一个整数数组 temperatures ，表示每天的温度，返回一个数组 answer ，其中 answer[i] 是指对于第 i 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 0 来代替。

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        Stack<Integer> stack=new Stack<>();
        int N=temperatures.length;
        int[] arr=new int[N];
        for(int i=0;i<N;i++){
            while(!stack.isEmpty() && temperatures[stack.peek()]<temperatures[i]){
                int k=stack.pop();
                arr[k]=i-k;
            }
            stack.push(i);
        }
        return arr;
    }
}
```


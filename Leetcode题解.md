## 713 
### 思路：滑动窗口+双指针 以右指针为计算的索引，right-left+1是右指针所能产生的满足条件的个数。要注意k<=1的情况，k<=1直接返回0.
### 代码：
class Solution 
{
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) 
    {
        if (k <= 1)
            return 0;
        int count = 0, left = 0 , one = 1;
        for (int right = 0; right < nums.size(); right++)
        {
            one *= nums[right];
            while (one >= k)
            {
                one /= nums[left];
                left++;
            }
            count += right - left + 1;
        }
        return count;
    }
};

## 560 和为K的数组
### 思路：使用Map<和，次数> preSum 来存储之前出现过的和，利用sum来对所有的元素进行逐一叠加，然后判断sum-k是否在preSum中，如果存在，说明
可以用sum-preSum的形式来得到一个前序和，因为有可能出现不同的组合都等于前序和，我们存储次数。每次找到后就用cnt加上前序和的次数，就得到了最终的结果。
### 代码
class Solution {
    public int subarraySum(int[] nums, int k) {
        if(nums==null || nums.length==0) return 0;
        Map<Integer,Integer> preSum=new HashMap();
        preSum.put(0,1);
        int sum=0,cnt=0;
        for(int i=0;i<nums.length;i++){
            sum+=nums[i];
            if(preSum.containsKey(sum-k)) cnt+=preSum.get(sum-k);
            preSum.put(sum,preSum.getOrDefault(sum,0)+1);
        }
        return cnt;
    }
}

## 33
## 思路：使用二分搜索法，nums[mid]如果大于nums[0]，说明左边是有顺序的。这时如果判断target在nums[mid]的左边，则令light=mid-1
注意：while(left<right){left=mid+1; right=mid;}
     while(left<=right){left=mid+1; right=mid-1;//防止right=mid跑不出循环}
     class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) return -1;
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if(nums[mid]==target) return mid;
            if (nums[mid] >= nums[0]) { 
                if (nums[mid] > target && target>=nums[0]) right = mid-1;
                else left = mid + 1;
            } else { 
                if (nums[mid] < target && target<=nums[nums.length-1]) left = mid + 1;
                else right = mid-1;
            }
        }
        return  -1;
    }
}

## 积水问题
### 思路1：双指针，考虑实际情况，接水量由两侧的最大最大高度控制，能够把两侧兜住。判断左边和右边的高度的大小，当height[left]<height[right]
时，左指针移动，移动后判断左指针的当前高度和leftMax的大小。若leftMax小，则更新leftMax。否则，用leftMax-height[left]计算当前所能盛水的面积**为什么不是Math.min(leftMax,rightMax)?因为开始的时候已经判断过height[left]和height[right]的大小，由于height[left]<height[right]<=rightMax,所以当左边移动时，一定是左边的最大小于右边的最大的时候**
### 代码
```java
public static int trap(int[] height) {
        int left = 0, right = height.length-1;
        int leftMax = 0, rightMax = 0;
        int res = 0;
        while (left < right) {
            if (height[left] < height[right]) {
                if (height[left] >= leftMax) leftMax = height[left];
                else res += (leftMax - height[left]);
                left++;
            } else {
                if (height[right] >= rightMax) rightMax = height[right];
                res += ( rightMax-height[right]);
                right--;
            }
        }
        return res;
    }
```

### 思路2 使用单调递减栈来做
### 利用单调递减栈里的元素都比栈顶大的特性，当当前欲添加的元素大于栈顶时，栈顶元素a弹出，计算a与它的两侧的最短高度之间所能承载的水量，当
栈顶元素小于当前欲压栈元素，就弹出，同时计算这一层的面积。
### 代码
```
public int trap(int[] height) {
          if (height == null || height.length == 0) return 0;
        Stack<Integer> s = new Stack();
        int cur = 0, res = 0;
        while (cur < height.length) {
            while (!s.isEmpty() && height[s.peek()] < height[cur]) {
                int temp=s.pop();
                if(s.isEmpty()) break;
                int dis=cur-s.peek()-1;
                int minHeight=Math.min(height[s.peek()],height[cur])-height[temp];
                res+=minHeight*dis;
            }
            s.push(cur++);
        }
        return res;
    }
```
     

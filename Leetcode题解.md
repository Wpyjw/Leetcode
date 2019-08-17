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
     

## 713 
### 思路：滑动窗口+双指针
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
### 思路：Map

第一章 编程技巧

判断两个浮点数a和b是否相等， 不用a == b. 应该判断两者之差的绝对值小于某一个阈值。fabs(a - b) < 1e-9.
判断某个数是否为奇数。x % 2 != 0, 不用 x % 2 == 1. 因为 x 有可能为负数。
vector和string 优先于动态分配的数组。

第二章 线性表

2.1 数组
2.1.1 Remove Duplicates from Sorted Array
/*时间复杂度O(n),空间复杂度O(1)*/
Class Solution{
public:
    int removeDuplicates(vector<int>& nums){
        if(nums.empty()) return 0;
        int index = 0;
        for(int i = 1; i < nums.size(); ++i>){
            if(nums[index] != nums[i])
                nums[++index] = nums[i];
        }
        return index + 1;
    }
}

/*使用STL，时间复杂度O(n),空间复杂度O(1)*/
Class Solution{
public:
    int removeDuplicates(vector<int>& nums){
        return distance(nums.begin(), unique(nums.begin(), nums.end()));
    }
}

2.1.2 Remove Duplicates from Sorted Array II
/*上一题的follow up 最多允许两个重复*/
Class Solution{
public:
    int removeDuplicates(vector<int>& nums){
        if(nums.size() <= 2>) return nums.size();
        int index = 2;
        for(int i = 2; i < nums.size(); ++i){
            if(nums[i] != nums[index - 2])
                nums[index++] = nums[i];
        }
        return index;
    }
}

Class Solution{
public:
    int removeDuplicates(vector<int>& nums){
        int n = nums.size();
        int index = 0;
        for(int i = 0; i < n; ++i){
            if(i > 0 && i < n - 1 && nums[i - 1] == nums[i] && nums[i + 1] == nums[i])
                continue;
            nums[index++] = nums[i];
        } 
    }
}

2.1.3 Search in Rotated Sorted Array
/**/
Class Solution{
public:
    int search(const vector<int>& nums, int target){
        int left = 0, right = nums.size();
        while(left != right){
            const int mid = left + (right - left)/2;
            if(nums[mid] == target) return mid;
            if(nums[left] <= nums[mid]){
                if(nums[left] <= target && nums[mid] > target)
                    right = mid;
                else
                    left = mid + 1;
            }
            else
            {
                if(nums[mid] < target && target <= nums[right - 1])
                    left = mid + 1;
                else
                    right = mid;
            }
        }
        return -1;
    }

2.1.4 Search in Rotated Sorted Array II
Follow up for 2.1.3, what if duplicates are allows?

Class Solution{
public:
    bool search(const vector<int>& nums, int target){
        if(nums.empty()) return false;
        int left = 0, right = nums.size();
        while(left < right){
            const int mid = left + (right - left)/2;
            if(nums[mid] == target) return true;
            if(nums[left] < nums[mid]){
                if(nums[left] <= target && nums[mid] > target)
                    right = mid;
                else
                    left = mid + 1;
            }
            else if(nums[left] > nums[mid]){
                if(nums[mid]< target && nums[right - 1] >= target)
                    left = mid + 1;
                else
                    right = mid;
            }
            else
                left++;
        }
        return false;
    }

2.1.5 Median of Two Sorted Arrays
/*中位数需要判断长度是奇数or偶数*/
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        const int m = nums1.size(), n = nums2.size();
        int total = m + n;
        if(total & 0x1)
            return findKth(nums1, 0, nums2, 0, total/2 + 1);
        else
            return (findKth(nums1, 0, nums2, 0, total/2 + 1) + findKth(nums1, 0, nums2, 0, total/2 + 2))/2.0;
    }
preivate:
    static int findKth(vector<int>& nums1, int i, vector<int>& nums2, int j, int k){
        if(i >= nums1.size()) return nums2[j + k - 1];
        if(j >= nums2.size()) return nums1[i + k - 1];
        if(k == 1) return min(nums1[i], nums2[j]);
        int midVal1 = (i + k/2 -1 < nums1.size()) ? nums1[i + k/2 -1] : INT_MAX;
        int midVal2 = (j + k/2 -1 < nums2.size()) ? nums2[j + k/2 -1] : INT_MAX;
        if(midVal1 < midVal2)
            return findKth(nums1, i + k/2, nums2, j, k - k/2);
        else
            return findKth(nums1, i, nums2, j + k/2, k - k/2);
    }

2.1.6 Longest Consecutive Sequence(128)
/*如果是nlogn 的时间复杂度，可以先排序，然后遍历找出连续的子序列
由于序列里的元素是无序的，想到用hashmap*/
class Solution{
public:
    int longestConsecutive(const vector<int>& nums){
        if(nums.empty()) return 0;
        unordered_map<int, bool> used;
        int longest = 0;
        for(auto i : nums) used[i] = false;
        for(auto i : nums){
            if(used[i]) continue;
            length = 1;
            used[i] = true;
            for(int j = i + 1; used.find(j) != used.end(); ++j){
                used[j] = true;
                length++;
            }
            for(int j = i - 1; used.find(j) != used.end(); --j){
                used[j] = true;
                length++;
            }
            longest = max(longest, length);
        }
        return longest;
    }
}
/*Hashset 解法*/
class Solution{
public:
    int longestConsecutive(const vector<int>& nums){
        int res = 0;
        unordered_set<int> s(nums.begin(), nums.end());
        for(auto val : nums){
            if(!s.count(val)) continue;
            s.erase(val);
            int next = val + 1, pre = val - 1;
            while(s.count(pre)) s.erase(pre--);
            while(s.count(next)) s.erase(next++);
            res = max(res, next - pre -1);  
        }
        return res;
    }
}

2.1.7 Two Sum (1)
class Solution{
public:
    vector<int> twoSum(vector<int>& nums, int target){
        unordered_map<int, int> m;
        for(int i = 0; i < nums.size(); ++i>){
            int diff = target - num;
            if(m.count(diff)) return {m[diff], i};
            m[nums[i]] = i; 
        }
        return {};   
    }
}

2.1.8 3Sum (15)
class Solution{
public:
    vector<vector<int>> threeSum(vector<int>& nums){
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        if(nums.size() < 3 || nums.front() > 0 || nums.back() < 0) return {};
        for(int k = 0; k < nums.size() - 2; ++k){
            if(nums[k] > 0) break;
            if(k > 0 && nums[k - 1] == nums[k]) continue;
            int target = 0 - nums[k], i = k + 1, j = (int)nums.size() - 1;
            while(i < j){
                if(nums[i] + nums[j] == target){
                    res.push_back({nums[k], nums[i], nums[j]});
                    while(i < j && nums[i] == nums[i + 1]) ++i;
                    while(i < j && nums[j] == nums[j - 1]) --j;
                    ++i; --j;
                }
                else if(nums[i] + nums[j] < target)
                    ++i;
                else
                    --j;
            }
        }
        return res;
    }
}

2.1.9 3Sum Colset

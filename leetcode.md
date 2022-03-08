

> x<<k = x * 2^k
>
> x>>k =  x/2^k for usigned # (logical right shift) MSB = 0 Most Significant Bit
>
> x>>k = x/2^k for signed # (arithmetic left shift) MSB = 1

# Array



Binary Search

二分查找的前提：

有序数组，数组中无重复元素

```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int begin = 0;
        int end = nums.size()-1;
        while(begin<=end){
            int middle = (end-begin)/2+begin;
            if(nums[middle] < target){
                begin = middle + 1;
            }
            else if(nums[middle] > target){
                end = middle - 1;
            }
            else{
                return middle;
            }
        }
        return -1;
    }
};

```





### 27. Remove Element

[27. 移除元素](https://leetcode-cn.com/problems/remove-element/)

```C++
//时间复杂度：O(n^2)
//空间复杂度：O(1)
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int count = 0;
        int s = nums.size();
        for(int count = 0; count < s; count++){
            if(nums[count] == val){
                for(int i = count; i < s-1; i++){
                    nums[i] = nums[i+1];
                }
                s--;
                count--;               
            }
        }
        return s;
    }
};
```

```C++
// 时间复杂度：O(n)
// 空间复杂度：O(1)
// double pointers
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int s = nums.size();
        int slow = 0;
        for(int quick = 0; quick < s; quick++){
            if(nums[quick] != val){
                nums[slow++] = nums[quick]; 
            }
        }
        return slow;
    }
};
```



### [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

```C++
// 时间复杂度： O(n)
// 空间复杂度：O(1)
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {       
        for(int i = 0; i < nums.size(); i++){           
            if(nums[i]>=target){
                return i;
            }
        }
        return nums.size();
    }
};
```

```C++
// 时间复杂度： O(logn)
// 空间复杂度：O(1)
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int begin = 0;
        int end = nums.size()-1;
        while(begin <= end){
            int middle = (end-begin)/2+begin;
            if(nums[middle] < target){
                begin = middle + 1;       
            }
            else if(nums[middle] > target){
                end = middle - 1;
            }
            else{
                return middle;
            }
        }
        return end+1;
    }
};
```



### 209. Minimum Size Subarray Sum

https://leetcode.com/problems/minimum-size-subarray-sum/

```C++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int res = INT32_MAX;
        int sum = 0;
        int temp_res = 0;
        for(int i = 0; i < nums.size(); i++){    
            sum = 0;   //need to initialize    
            for(int j = i; j < nums.size(); j++){
                sum += nums[j];
                if(sum>=target){
                    temp_res = j-i+1;
                    res = res > temp_res ? temp_res : res;
                    break; //in case of useless iteration
                }       
            }           
        }
        return res == INT32_MAX ? 0 : res;
    }
};
```

```C++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int res = INT32_MAX;
        int sum = 0;
        int temp_res = 0;
        int i = 0;
        for(int j = 0; j < nums.size(); j++){         
            sum += nums[j];
            while(sum >= target){               
                temp_res = j-i+1;
                res = res>temp_res ? temp_res : res;
                sum -= nums[i++];
              // ++i:increment the value of i, then return the value；
              // i++:increment the value of i, but return the original value that i held before being incremented.
            }
        }
        return res == INT32_MAX ? 0 : res;
    }

};
```

```C++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int res = INT32_MAX;
        int temp = 0;
        int sum = 0;
        int j = 0;
        for(int i = 0; i < nums.size(); i++){
            sum += nums[i];
            while(sum >= target){
                temp = i - j + 1;
                res = res > temp ? temp : res;
                sum -= nums[j];
                j++;
            }
        }
        return res = res == INT32_MAX ? 0 : res;
    }
};
```



### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```C++
class Solution {
private:
    int findLeftBorder(vector<int>& nums, int target){
        int left = 0;
        int right = nums.size()-1;        
        int leftBorder = INT32_MAX;
        while(left <= right){
            int middle = (right-left)/2+left;
            if(nums[middle] < target){
                left = middle + 1;
            }
            else{
                right = middle - 1;
                leftBorder = right;
            }
        }
        return leftBorder;
    }
    int findRightBorder(vector<int>& nums, int target){
        int left = 0;
        int right = nums.size()-1;       
        int rightBorder = INT32_MAX;
        while(left <= right) {
            int middle = (right-left)/2+left;
            if(nums[middle] > target) {
                right = middle - 1;
            }
            else {
                left = middle + 1;
                rightBorder = left;
            }
        }
        return rightBorder;
    }
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int l = findLeftBorder(nums, target);
        int r = findRightBorder(nums, target);
        vector<int> res;
        if(l == INT32_MAX || r == INT32_MAX){
            return {-1, -1};
        }
        else if(r-l > 1){ 
            res.push_back(l+1);
            res.push_back(r-1);
            return res;
        }      
       
        return {-1, -1};       
    }
};
```

### [977. 有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)

```C++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        for(int i = 0; i < nums.size(); i++){
            nums[i] *= nums[i];
        }
        sort(nums.begin(), nums.end());
        return nums;
    }
};
```

```C++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int j = nums.size() - 1;
        int i = 0;
        int m = nums.size() - 1;
        vector<int> res(nums.size(), 0);//
        while(i <= j){
            if(nums[i]*nums[i] < nums[j]*nums[j]){
                res[m--] = nums[j]*nums[j];
                j--;    
            }
            else {
                res[m--] = nums[i]*nums[i];
                i++;       
            }          
        }
        return res;
    }
};
```

### [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

```C++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {       
        int begin = 0;
        int end = nums.size() - 1;
        while(begin <= end){
            int mid = begin + (end-begin)/2;
            if(nums[mid] < target){
                begin = mid + 1;
            }
            else if(nums[mid] > target){
                end = mid - 1;
            }
            else{
                return mid;
            }
        }
        return end + 1;
    }
};
```

### [209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)

```C++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int sum = 0;
        int j = 0;
        int res = INT32_MAX;
        int temp = 0;
        for(int i = 0; i < nums.size(); i++){
            sum += nums[i];
            while(sum >= target){               
                temp = i - j + 1;
                res = temp >= res ? res : temp;
                sum -= nums[j++];               
            }
        }
        return res==INT32_MAX ? 0 : res;
    }
};
```



### [剑指 Offer 04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

```C++
//时间复杂度：O(n*m)
//空间复杂度：O(1)
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        for(int i = 0; i < matrix.size(); i++){
            for(int j = 0; j < matrix[0].size(); j++){
                if(matrix[i][j] == target){
                        return true;
                }
                else{ continue ;}              
            }
        }
        return false;
    }
};
```



```C++
//时间复杂度：O(n+m)
//空间复杂度：O(1)
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        int i = matrix.size() - 1, j = 0;
        while(i >= 0 && j < matrix[0].size())
        {
            if(matrix[i][j] > target) i--;
            else if(matrix[i][j] < target) j++;
            else return true;
        }
        return false;
    }
};
```

###  59.螺旋矩阵II

```C++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n, vector<int>(n, 0));
        int mid = n/2;
        int cycle = n/2;
        int startX = 0;
        int startY = 0;
        int offset = 1;
        int count = 1;
        while(cycle--){
            int i,j;
            for(i = startY; i < startY+n-offset; i++){
                res[startX][i] = count++;
            }
            for(j = startX; j < startX+n-offset; j++){
                res[j][i] = count++;
            }
            for(; i > startY; i--){
                res[j][i] = count++;
            }
            for(; j > startX; j--){
                res[j][i] = count++;
            }
            offset += 2;
            startX++;
            startY++;

        }
         if (n % 2) {
            res[mid][mid] = count;
        }
        return res;
    }
};
```

### [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode * dummyHead  =  new ListNode(0);
        dummyHead->next = head;
        ListNode * cur = dummyHead;
        while(cur->next != NULL && cur != NULL){
            if(cur->next->val == val){
            ListNode * temp = cur->next;
            cur->next = cur->next->next;
            delete temp;
            }           
            else {
                cur = cur->next;
            }
        }
        head = dummyHead->next;
        delete dummyHead;
        return head;
    }
};
```

# Linked List

### [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyHead = new ListNode(0);
        ListNode* cur = dummyHead;
        dummyHead->next = head;
        while(cur->next != NULL){
            if(cur->next->val == val){
                ListNode* temp = cur->next;
                cur->next = cur->next->next;
                delete temp;
            }
            else {cur = cur->next;}
        }
        head = dummyHead->next;
        delete dummyHead;
        return head;
    }
};
```

### [707. 设计链表](https://leetcode-cn.com/problems/design-linked-list/)

```C++
class MyLinkedList {
public:
 struct LinkedNode{
        int val;
        LinkedNode* next;
        LinkedNode(int val): val(val), next(NULL){}
    };

    MyLinkedList() {
        dummyHead = new LinkedNode(0);
        size = 0;
    }
    
    int get(int index) {
        if(index < 0 || index > size-1){
            return -1;
        }
        LinkedNode* cur = dummyHead->next;
        while(index--){           
            cur = cur->next;          
        }
        return cur->val;
    }
    
   void addAtHead(int val) {
        LinkedNode* add = new LinkedNode(val);
        add->next = dummyHead->next;
        dummyHead->next = add;
        size++;
        return;
    }

    void addAtTail(int val) {
        LinkedNode* newTail = new LinkedNode(val);
        LinkedNode* cur = dummyHead;
        while(cur->next != NULL){
            cur = cur->next;
        }
        cur->next  = newTail;
        size++;
    }
    void addAtIndex(int index, int val) {
        LinkedNode* cur = dummyHead;
        LinkedNode* newNode = new LinkedNode(val);
        if(index > size-1){
            return;
        }
        else if(index < 0){
            addAtHead(val);
        }
        else if(index == size){
            addAtTail(val);
        }else{
            while(index--){
                cur = cur->next;
            }
            newNode->next = cur->next;
            cur->next = newNode;
        }
        return;
    }
    
    void deleteAtIndex(int index) {
        LinkedNode* cur = dummyHead;
        index -= index;
        while(index--){
            cur = cur->next;
        }
        LinkedNode* temp = cur->next;
        cur->next = cur->next->next;
        delete temp;
        return;
    }
private:
    int size;
    LinkedNode* dummyHead;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```

### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* cur = head;;
        ListNode* pre = NULL;
        while(cur){
            ListNode* temp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = temp;           
        }
        head = pre;
        return head;
    }
};
```

### [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        if(head == NULL || head->next == NULL){
            return head;
        }
        ListNode* pre = dummyHead;
        ListNode* A = head;

        while(A != NULL && A->next != NULL){
            ListNode* B = A->next;
            A->next = A->next->next;
            B->next = A;
            pre->next = B;
            pre = A;
            A = pre->next;
        }
        head = dummyHead->next;
        delete dummyHead;
        return head;

    }
};
```

### [19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* first = dummyHead;
        ListNode* second = dummyHead;
        while(n--){
            first = first->next;
        }    
        while(first->next != NULL){
            first = first->next;
            second = second->next;
        }  
        ListNode* temp = second->next;  
        second->next = second->next->next;
        delete temp;
        return dummyHead->next;
    }
};
```

### [面试题 02.07. 链表相交](https://leetcode-cn.com/problems/intersection-of-two-linked-lists-lcci/)

```C++
//时间复杂度：O(n + m)
//空间复杂度：O(1)
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* A = headA;
        ListNode* B = headB;
        int sizeA = 0;
        int sizeB = 0;
        while(A != NULL){
            A = A->next;
            sizeA++;
        }
        while(B != NULL){
            B = B->next;
            sizeB++;
        }       
        int diff = abs(sizeA - sizeB);
        if(sizeA >= sizeB){
            A = headA;
            B = headB;
        }
        else{
            B = headA;
            A = headB;
        }
        while(diff--){
            A = A->next;
        }
        while(A != B){
            A = A->next;
            B = B->next;
        }
        return A;
    }
};
```

### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while(fast != NULL && fast->next != NULL){//take care about the fast->next
            slow = slow->next;
            fast = fast->next->next;
            if(slow == fast){
                slow = head;
                while(slow != fast){
                    slow=slow->next;
                    fast= fast->next;
                }
                return slow;
            }

        }
        return NULL;
    }
};
```

# Hash Table

### [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

```C++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int a[26] = {0};
        for(int i = 0; i < s.size(); i++){
            a[s[i]-'a']++;
        }
        for(int i = 0; i < t.size(); i++){
            a[t[i]-'a']--;
        }
        for(int i = 0; i < 26; i++){
            if(a[i]!=0) return false;
        }
        return true;
    }
};
```

### [349. 两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

```C++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> s1(nums1.begin(), nums1.end());
        unordered_set<int> res;
        for(int num : nums2){
            if(s1.find(num)!=s1.end()) {
              res.insert(num);
            }
        }
        vector<int> vec(res.begin(), res.end());
        return vec;
    }
};
```



# String

### [71. 简化路径](https://leetcode-cn.com/problems/simplify-path/)

```C++
class Solution {
public:
    string simplifyPath(string path) {
        vector<string> v;
        istringstream iss(path);
        string buf;
        while(getline(iss, buf, '/')){
            if(!buf.empty() && buf != "." && buf != ".."){
                v.push_back(buf);
            }
            if(!v.empty() && buf == ".."){
                v.pop_back();
            }
        }
        if(v.empty()){
            return "/";
        }
        buf.clear();
        string res;
        for(string s:v){
            res += "/";
            res += s;
        }
        return res;
    }
};
```

### [415. 字符串相加](https://leetcode-cn.com/problems/add-strings/)

```C++
class Solution {
public:
    string addStrings(string num1, string num2) {
        string str;
        int num = 0;
        int s1 = num1.size()-1;
        int s2 = num2.size()-1;
        while(s1 >= 0 || s2 >= 0 || num > 0) {
            if(s1>=0) num += num1[s1--]-'0';
            if(s2>=0) num += num2[s2--]-'0';
            str = to_string(num%10) + str;
            num /= 10;           
        }
        return str;
    }
};
```



# Double Pointers

### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        for(int i = 0; i < nums.size(); i++){
            if(nums[i] > 0){
                return res;
            }
            if(i > 0 && nums[i] == nums[i-1]){
                continue;
            }
            int left = i + 1;
            int right = nums.size()-1;
            while(left < right){
                if(nums[i] + nums[left] + nums[right] < 0){
                    left++;
                }else if(nums[i] + nums[left] + nums[right] > 0){
                    right--;
                }else{
                    res.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    while(right > left && nums[right] == nums[right-1]) right--;
                    while(right > left && nums[left] == nums[left+1]) left++;
                    right--;
                    left++;
                }               
            }            
        }
        return res;  
    }
};
```



# Stack and Queue

### 232. Implement Queue using Stacks

https://leetcode.com/problems/implement-queue-using-stacks/

```C++
class MyQueue {
public:
    stack<int> stk1;
    stack<int> stk2;
    MyQueue() {        
    }
    
    void push(int x) {
        stk1.push(x);
    }  
    
    int pop() {      
        while(!stk1.empty()){
            stk2.push(stk1.top());
            stk1.pop();
        }        
        int res = stk2.top();
        stk2.pop();
        while(!stk2.empty()){
            stk1.push(stk2.top());
            stk2.pop();
        }
        return res;
    }
    
    int peek() {
        /*
        int res;
        while(!stk1.empty()){
            stk2.push(stk1.top());
            stk1.pop();
        }
        res = stk2.top();       
        while(!stk2.empty()){
            stk1.push(stk2.top());
            stk2.pop();
        }        
        return res;
        */        
        int res = this->pop();
        while(!stk1.empty()){
            stk2.push(stk1.top());
            stk1.pop();
        }
        stk1.push(res);
        while(!stk2.empty()){
            stk1.push(stk2.top());
            stk2.pop();
        }
        return res;   
    }
    
    bool empty() {
        return stk1.empty() && stk2.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```



```C++
class MyQueue {
public:
    stack<int> stk1;
    stack<int> stk2;
    MyQueue() {        
    }
    
    void push(int x) {
        stk1.push(x);
    }  
    
    int pop() {
        if(stk2.empty()){
        while(!stk1.empty()){
            stk2.push(stk1.top());
            stk1.pop();
            }
        }
        int res = stk2.top();
        stk2.pop();
        return res;
    }
    
    int peek() {   
        int res = this->pop();
        stk2.push(res);
        return res;    
    }
    
    bool empty() {
        return stk1.empty() && stk2.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```



### 225. Implement Stack using Queues

```C++
class MyStack {
public:
    queue<int> que1;
    queue<int> que2;
    MyStack() {}
    
    void push(int x) {
        que1.push(x);
    }
    
    int pop() {
        int s = que1.size();
        s--;
        while(s--){
            que2.push(que1.front());
            que1.pop();            
        }
        int a = que1.front();
       que1 = que2;
        while(!que2.empty()){            
            que2.pop();
        }
        return a;        
    }
    
    int top() {
        return que1.back();
    }
    
    bool empty() {
        return que1.empty();
    }
};
```

use only one queue to implement stack

Push the front one to the queue until the last one, then return the last one and pop the last one

**一个队列在模拟栈弹出元素的时候只要将队列头部的元素（除了最后一个元素外） 重新添加到队列尾部，此时在去弹出元素就是栈的顺序了。**



### 20. Valid Parentheses

https://leetcode.com/problems/valid-parentheses/

```C++
class Solution {
public:
    bool isValid(string s) {
        stack<int> st;
        for(int i = 0; i < s.size(); i++){
            if(s[i] == '(') st.push(')');            
            else if(s[i] == '{') st.push('}');
            else if(s[i] == '[') st.push(']');
            else if(st.empty() || st.top() != s[i]) return false; //must put empty() in the first           
            else st.pop();
        }                    
        return st.empty();   
    }
};
```



### 1047. Remove All Adjacent Duplicates In String

https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/

```C++
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> st;
        for(auto str : s){
            if(st.empty() || str != st.top()){
                st.push(str);
            }
            else{
                st.pop();
            }
        }
        string res = "";
        while(!st.empty()){
            res += st.top();
            st.pop();
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```



### 150. Evaluate Reverse Polish Notation

https://leetcode.com/problems/evaluate-reverse-polish-notation/

```C++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> st;
        for(int i = 0; i < tokens.size(); i++){
            if(tokens[i] == "+" || tokens[i] == "-" || tokens[i] == "*" || tokens[i] == "/"){
               int num1 = st.top();
               st.pop();
               int num2 = st.top();
               st.pop();                  
               if(tokens[i] == "+"){
                   st.push(num1+num2);
               }
               if(tokens[i] == "-"){
                   st.push(num2-num1);
               }
               if(tokens[i] == "*"){
                   st.push(num2*num1);
               }
               if(tokens[i] == "/"){
                   st.push(num2/num1);
               }               
           }
            else{
                st.push(stoi(tokens[i]));
           }
        }
        int result = st.top();
            return result;            
    }        
};
```



### 239. Sliding Window Maximum

https://leetcode.com/problems/sliding-window-maximum/

```C++
class Solution {
private:
    class MyQueue{
    public:
        deque<int> deq;
        void pop(int value){
            if(!deq.empty() && deq.front() == value){
                deq.pop_front();
            }            
        }
        
        void push(int value){
            while(!deq.empty() && value > deq.back()){
                deq.pop_back();
            }
            deq.push_back(value);
        }
        
        int front(){
            return deq.front();
        }
    };
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        MyQueue que;
        vector<int> res;
        for(int i = 0; i < k; i++){
            que.push(nums[i]);
        }
        res.push_back(que.front());
        for(int i = k; i< nums.size(); i++){
            que.pop(nums[i-k]);
            que.push(nums[i]);
            res.push_back(que.front());
        }
        return res;
    }
};
```

### 347. Top K Frequent Elements

https://leetcode.com/problems/top-k-frequent-elements/

```C++
class Solution {
public:
    class myComparison{
    public:
        bool operator()(const pair<int, int>& lhs, const pair<int,int>& rhs){
            return lhs.second > rhs.second;
        }
    };
    vector<int> topKFrequent(vector<int>& nums, int k) {
       unordered_map<int, int> map; //map<nums[i],times show up>
       for(int i = 0; i < nums.size(); i++){
           map[nums[i]]++;
       } 
        //complete binary tree performed as vector    	
        priority_queue<pair<int, int>, vector<pair<int, int>>, myComparison> prio_que;     
      	// "it" is a pointer, if you use auto it : map, next line should be prio_que.push(it);
        for(unordered_map<int,int>::iterator it = map.begin(); it!=map.end(); it++){
            prio_que.push(*it);
            if(prio_que.size() > k){
                prio_que.pop();
            }
        }
        vector<int> res(k);
        for(int i = k-1; i >= 0; i--){
            res[i] = prio_que.top().first;
            prio_que.pop();
        }
        return res;        
    }
};
```

```C++
class Solution {
public:    
    vector<int> topKFrequent(vector<int>& nums, int k) {
        vector<int> res;
        unordered_map<int, int> m;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        for(auto i : nums){
            m[i]++;
        }
        for(auto it : m){            
            pq.push(make_pair(it.second, it.first));   
            if(pq.size()>k) pq.pop();         
        }
        for(int i  = 0; i < k; i++) {
            res.push_back(pq.top().second);
            pq.pop();
        }
        return res;
    }    
};
```



# Binary Tree

- complete binary tree: 在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。
- full binary tree: depth k, root = 2^k - 1
- binary search tree: ordered
- blanced binary search tree (AVL-Adelson-Velsky and Landis): 它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。
- **C++中map、set、multimap，multiset的底层实现都是平衡二叉搜索树**, 所以map、set的增删操作时间时间复杂度是logn, unordered_map底层实现是哈希表。
- **二叉树可以链式存储，也可以顺序存储。**链式存储方式就用指针， 顺序存储的方式就是用数组(内存连续分布)。**如果父节点的数组下标是 i，那么它的左孩子就是 i * 2 + 1，右孩子就是 i * 2 + 2。**
- 二叉树主要有两种遍历方式：
  1. 深度优先遍历：先往深走，遇到叶子节点再往回走。
     - 前序遍历（递归法，迭代法）
     - 中序遍历（递归法，迭代法）
     - 后序遍历（递归法，迭代法）
  2. 广度优先遍历：一层一层的去遍历。
     - 层次遍历（迭代法）

### Definition

```C++
struct TreeNode{
  int val;
  TreeNode* left;
  TreeNode* right;
  TreeNode(): val(0), left(NULL), right(NULL){}
  TreeNode(int x): val(x), left(NULL), right(NULL){}
  TreeNode(int x, TreeNode* left, TreeNode* right): val(x), left(left), right(right){}
};
```



### 144. Binary Tree Preorder Traversal

https://leetcode.com/problems/binary-tree-preorder-traversal/

#### Recursion

```C++
class Solution {
public:
    void traversal(TreeNode* cur, vector<int>& vec){
        if(cur == NULL) return;
        vec.push_back(cur->val);
        traversal(cur->left, vec);
        traversal(cur->right, vec);        
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        traversal(root, res);
        return res;
    }
};
```

#### Iteration

```C++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> stk;
        vector<int> res;
        if(root == NULL) return res;  
        stk.push(root);
        while(!stk.empty()){
            TreeNode* node = stk.top();
            res.push_back(node->val);
            stk.pop();           
            if(node->right){stk.push(node->right);}
            if(node->left){stk.push(node->left);}
        }
        return res;    
    }
};
```

```C++
```





### 94. Binary Tree Inorder Traversal

https://leetcode.com/problems/binary-tree-inorder-traversal/

```C++
class Solution {
public:
    void traversal(TreeNode* root, vector<int>& vec){
        if(root == NULL) return;
        traversal(root->left, vec);
        vec.push_back(root->val);   
        traversal(root->right, vec);            
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        traversal(root, res);
        return res;        
    }
};
```

```C++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        if(root==NULL) return res;
        stk.push(root);
        while(!stk.empty()){
            TreeNode* node = stk.top();
            stk.pop();
            res.push_back(node->val);
            if(node->right) stk.push(node->right);
            if(node->left) stk.push(node->left);
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```



### 145. Binary Tree Postorder Traversal

### [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

```C++
class Solution {
public:
    void traversal(TreeNode* cur, vector<int>& vec){
        if(cur == NULL) return;
        traversal(cur->left, vec);
        traversal(cur->right, vec);
        vec.push_back(cur->val);
    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        traversal(root, res);
        return res;
    }
};
```

```C++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        if(root==NULL) return res;
        stk.push(root);
        while(!stk.empty()){
            TreeNode* node = stk.top();
            stk.pop();
            res.push_back(node->val);
            if(node->left) stk.push(node->left);
            if(node->right) stk.push(node->right);           
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

```C++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
       vector<int> res;
       stack<TreeNode*> stk;
       if(root==NULL) return res;
       stk.push(root);
       while(!stk.empty()){
           TreeNode* node = stk.top();
           if(node != NULL){
               stk.pop();
               stk.push(node);
               stk.push(NULL);
               if(node->right)stk.push(node->right);
                if(node->left)stk.push(node->left);
           }else{
               stk.pop();
               node = stk.top();
               res.push_back(node->val);
               stk.pop();
           }
       }
       return res;
    }
};
```

### [589. N 叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

```C++
class Solution {
public:
    void traversal(Node* root, vector<int>& res){    //keep in mind the &!!!!!    
        if(root==NULL) return;
        res.push_back(root->val);
        int size = root->children.size();
        for(int i = 0; i < size; i++){
            traversal(root->children[i], res);
        }
    }
    vector<int> preorder(Node* root) {
        vector<int> res;
        traversal(root, res);
        return res;        
    }
};
```

### [590. N 叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/)

```C++
class Solution {
public:
    void traversal(Node* root, vector<int>& res){
        if(root==NULL)return;
        for(Node* child : root->children){
            traversal(child, res);
        }
        res.push_back(root->val);
    }
    vector<int> postorder(Node* root) {
        vector<int> res;
        traversal(root, res);
        return res;       
    }
};
```

### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

```C++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> que;
        vector<vector<int>> res;
        if(root==NULL) return res;
        que.push(root);
        while(!que.empty()){
            vector<int> temp;
            int size = que.size();
            for(int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                temp.push_back(node->val);
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            res.push_back(temp);
            //que.clear();there is no such method, we can just declare in while loop
        }
        return res;
    }
};
```

### [107. 二叉树的层序遍历 II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

```C++
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        queue<TreeNode*> que;
        vector<vector<int>> res;
        if(root==NULL){return res;}
        que.push(root);
        while(!que.empty()){
            vector<int> temp;           
            int size = que.size();
            for(int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                temp.push_back(node->val);
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);               
            }
            res.push_back(temp);
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

### [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

```C++
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> res;
        if(root==NULL) return res;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()){
            stack<int> stk;            
            int size = que.size();
            for(int i = 0; i < size; i++){
                TreeNode* node = que.front();
                stk.push(node->val);
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }            
            res.push_back(stk.top());
        }
        return res;
    }
};
```

### [637. 二叉树的层平均值](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/)

```C++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> res;
        queue<TreeNode*> que;
        que.push(root);
        if(root==NULL) return res;
        while(!que.empty()){
            vector<int> temp;
            int size = que.size();
            for(int i = 0; i < size; i++){
                TreeNode* node = que.front();
                temp.push_back(node->val);
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            double sum = 0;
            for(int j = 0; j < temp.size(); j++){                
                sum += temp[j];               
            }
            double avg = sum/temp.size();
            res.push_back(avg);
        }
        return res;
    }
};
```

### [429. N 叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

```C++
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> res;
        if(root==NULL) return res;
        queue<Node*> que;
        que.push(root);
        while(!que.empty()){
            vector<int> temp;
            int size = que.size();
            for(int i = 0; i < size; i++){
                Node* node = que.front();
                que.pop();
                temp.push_back(node->val);
                for(int j = 0; j < node->children.size(); j++){
                    que.push(node->children[j]);
                }
            }
            res.push_back(temp);
        }
        return res;        
    }
};
```

### [515. 在每个树行中找最大值](https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/)

```C++
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        vector<int> res;
        if(root==NULL) return res;        
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()){
            vector<int> temp;
            int size = que.size();
            int m = INT32_MIN;
            for(int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                m = node->val > m ? node->val : m;                       
                temp.push_back(m);                              
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }            
            res.push_back(m);
        }
        return res;
    }
};
```

### [116. 填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)

```C++
class Solution {
public:
    Node* connect(Node* root) {
        queue<Node*> que;
        if(root != NULL){           
            que.push(root);
        }       
        while(!que.empty()){
            Node* pre;
            Node* node;
            int size = que.size();
            for(int i = 0; i < size; i++){
                if(i == 0){
                    pre=que.front();
                    que.pop();
                    node = pre;
                } else {
                    node = que.front();
                    que.pop();
                    pre->next = node;
                    pre = node;
                }
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);               
            }
            node->next = NULL;            
        }
        return root;
    }   
};
```

### [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

```C++
class Solution {
public:
    int getdepth(TreeNode* root){       
        if(root==NULL) return 0;      
        int a = getdepth(root->left);
        int b = getdepth(root->right);
        int depth = 1 + (a>b?a:b);
        return depth;        
    }
    int maxDepth(TreeNode* root) {        
        return getdepth(root);        
    }
};
```



### [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

```C++
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(root == NULL) return 0;
        queue<TreeNode*> que;
        if(root!=NULL) {
            que.push(root);
        }
        int depth = 1;
        while(!que.empty()) {
            int size = que.size();                      
            for(int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
                if(!node->left && !node->right) return depth;   //condition            
            }
            depth++;           
        }
        return depth;
    }
};
```

```C++
class Solution {
public:
    int getDepth(TreeNode* node) {
        if (node == NULL) return 0;
        int leftDepth = getDepth(node->left);           // 左
        int rightDepth = getDepth(node->right);         // 右
                                                        // 中
        // 当一个左子树为空，右不为空，这时并不是最低点
        if (node->left == NULL && node->right != NULL) { 
            return 1 + rightDepth;
        }   
        // 当一个右子树为空，左不为空，这时并不是最低点
        if (node->left != NULL && node->right == NULL) { 
            return 1 + leftDepth;
        }
        int result = 1 + min(leftDepth, rightDepth);
        return result;
    }

    int minDepth(TreeNode* root) {
        return getDepth(root);
    }
};
```



### [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

```C++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root==NULL) return root;
        swap(root->left, root->right);
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }
};
```

### [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

```C++
class Solution {
public:
    bool compare(TreeNode* left, TreeNode* right){
        if(left==NULL && right==NULL) return true;
        else if(left != NULL && right == NULL) return false;
        else if(left == NULL && right != NULL) return false;
        else if(left->val != right->val) return false;
        else{
        bool a = compare(left->left, right->right);
        bool b = compare(left->right, right->left);
        if(a && b) return true;
        else return false;
        }
    }
    bool isSymmetric(TreeNode* root) {
        if(root==NULL) return true;
        return compare(root->left, root->right);
    }
};
```

### [222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

```C++
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(root==NULL) return 0;
        int leftHeight = 0, rightHeight = 0;
        TreeNode* left = root->left;
        TreeNode* right = root->right;
        while(left){
            left = left->left;
            leftHeight++;
        }
        while(right){
            right = right->right;       
            rightHeight++;
        }
        if(leftHeight==rightHeight){
            return (2<<leftHeight)-1;
        }
        return 1+countNodes(root->left)+countNodes(root->right);
    }
};
```

### [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

```C++
class Solution {
public:
    int getdepth(TreeNode* root){
        if (root==NULL) return 0;
        int leftHeight = getdepth(root->left);
        if(leftHeight==-1) return -1;
        int rightHeight = getdepth(root->right);
        if(rightHeight==-1) return -1;
        return abs(leftHeight-rightHeight)>1?-1:1+max(leftHeight,rightHeight);
    }
    bool isBalanced(TreeNode* root) {
        return getdepth(root)==-1?false:true;
    }
};
```

### [257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)

```C++
class Solution {
public:
    void traversal(vector<string>& res, string path, TreeNode* root){
        path += to_string(root->val);
        if(!root->left && !root->right){ 
            res.push_back(path);
            return;
        }
        if(root->left) traversal(res, path + "->", root->left);
        if(root->right) traversal(res, path + "->", root->right);       
    }
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> res;
        string path;
        if (root==NULL) return res;
        traversal(res, path, root);
        return res;
    }
};
```

### [572. 另一棵树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)

```C++
class Solution {
public:
    bool compare(TreeNode* p, TreeNode* q){
        if(!p && !q) return true;
        if((!p && q) || (p && !q) || (p->val!= q->val)) return false;
        return compare(p->left, q->left) && compare(p->right, q->right);
    }
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
    if(!root && !subRoot) return true;
    if(!root) return false;
    return compare(root, subRoot) || isSubtree(root->left, subRoot) || isSubtree(root->right, subRoot);
    }
};
```

### [404. 左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/)

```C++
class Solution {   
public:
    int sumOfLeftLeaves(TreeNode* root) {                 
        if(!root) return 0;    
        int res=0;  
        if(root->left && !root->left->left && !root->left->right) {res = root->left->val;}       
        return res+sumOfLeftLeaves(root->left)+sumOfLeftLeaves(root->right);
    }
};
```

### [513. 找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)

```C++
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {        
        if(root==NULL) return 0;
        vector<vector<int>> store;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()){
            int size = que.size();
            vector<int> path;
            for(int i = 0; i < size; i++){
                TreeNode* node = que.front();
                path.push_back(node->val);
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            store.push_back(path);          
        } 
        int s = store.size();
        int res = store[s-1][0];
        return res;
    }
};
```

### [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

```C++
//recursion
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root==NULL) return false;        
        if(!root->left && !root->right && targetSum==root->val) return true;       
        return hasPathSum(root->left, targetSum - root->val) || hasPathSum(root->right, targetSum - root->val);
    }
};
```

```C++
//iteration
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root == NULL) return false;
        stack<pair<TreeNode*, int>> stk;
        stk.push(pair<TreeNode*, int> {root, root->val});
        while(!stk.empty()) {
            pair<TreeNode*, int> cur = stk.top();
            stk.pop();
            if(!cur.first->left && !cur.first->right && targetSum == cur.second){
                return true;
            }
            if(cur.first->left) { 
                stk.push(pair<TreeNode*, int> {cur.first->left, cur.second+cur.first->left->val}); 
            }
            if(cur.first->right) {
                stk.push(pair<TreeNode*, int> {cur.first->right, cur.second+cur.first->right->val}); 
            }
        }
        return false;
    }
};
```

### [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

```C++
class Solution {
private:
    vector<vector<int>> res;
    vector<int> path;
    void traversal(TreeNode* root, int sum) {  
        if (!root) return;
        if (!root->left && !root->right && sum == 0){
            res.push_back(path);
            return;
        }    
        if(root->left){
            sum -= root->left->val;    
            path.push_back(root->left->val);              
            traversal(root->left, sum);
            sum += root->left->val;
            path.pop_back();           
        }
        if(root->right){
            sum -= root->right->val;     
            path.push_back(root->right->val);       
            traversal(root->right, sum);
            sum += root->right->val;
            path.pop_back();        
        }       
    }
public:
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {      
        if(root==NULL) return res; 
        path.push_back(root->val);      
        traversal(root, targetSum - root->val);
        return res;
    }
};
```

### [106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

```C++
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        int size = inorder.size();
        if(size==0) return NULL;
        int value = postorder[size-1];
        TreeNode* node = new TreeNode(value);
        if(size==1) return node;
        int index;
        for(int i = 0; i < size; i++){
            if(inorder[i]==value) {
                index = i;
                break;
            }
        }
        vector<int> leftInorder(inorder.begin(), inorder.begin()+index);
        vector<int> rightInorder(inorder.begin()+index+1, inorder.end());
        postorder.resize(size-1);
        vector<int> leftPostorder(postorder.begin(), postorder.begin()+leftInorder.size());
        vector<int> rightPostorder(postorder.begin()+leftInorder.size(), postorder.end());
        node->left = buildTree(leftInorder, leftPostorder);
        node->right = buildTree(rightInorder, rightPostorder);
        return node;
    }
};
```

### [654. 最大二叉树](https://leetcode-cn.com/problems/maximum-binary-tree/)

```C++
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {             
        int size = nums.size();
        if (size == 0) return NULL;            
        int index = 0;
        int temp = nums[0];
        for(int i = 0; i < size; ++i){           
            if(nums[i]>temp){
                index = i;
                temp = nums[i];
            }
        }      
        TreeNode* res = new TreeNode(temp);       
        vector<int> l(nums.begin(), nums.begin()+index);
        vector<int> r(nums.begin()+index+1, nums.end());       
        res->left = constructMaximumBinaryTree(l);
        res->right = constructMaximumBinaryTree(r);
        return res;
    }
};
```

### [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

```C++
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if(root1 == NULL && root2 == NULL) { return NULL;}
        if(root1 == NULL){ return root2;}
        if(root2 == NULL){ return root1;}
        TreeNode* res = new TreeNode(root1->val+root2->val);
        res->left = mergeTrees(root1->left, root2->left);
        res->right = mergeTrees(root1->right, root2->right);
        return res;
    }
};
```

### [700. 二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)

```C++
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {        
        if(root==NULL || root->val==val) return root;
        if(root->val<val) return searchBST(root->right, val);
        if(root->val>val) return searchBST(root->left, val);
        return NULL;        
    }
};
```

### [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

```C++
class Solution {
public:
    TreeNode* pre=NULL;
    bool isValidBST(TreeNode* root) {
        if(root==NULL) return true;
        bool left = isValidBST(root->left);
        if(pre && pre->val>=root->val) return false;
        pre = root;
        bool right = isValidBST(root->right);
        return left&&right;         
    }
};
```

```C++
class Solution {
private:
    vector<int> l;
    void traversal(TreeNode* root){
        if(root==NULL) return;
        traversal(root->left);
        l.push_back(root->val);
        traversal(root->right);
    }
public:   
    bool isValidBST(TreeNode* root) {
        if(root==NULL) return true;
        traversal(root);
        for(int i = 1; i < l.size(); i++){
            int pre = l[i-1];
            if(pre >= l[i]) return false; 
        }
        return true;               
    }
};
```

### [530. Minimum Absolute Difference in BST](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)

```C++
class Solution {
private:
    vector<int> vec;
    void traversal(TreeNode* root) {
        if(root==NULL) return;
        traversal(root->left);
        vec.push_back(root->val);
        traversal(root->right);
    }
public:
    int getMinimumDifference(TreeNode* root) {
        if(root==NULL) return 0;
        traversal(root);
        int diff = INT32_MAX;
        for(int i = 1; i < vec.size(); i++) {
            int temp = vec[i] - vec[i-1];
            diff = temp < diff? temp : diff;                  
        }
        return diff;
    }
};
```

### [501. Find Mode in Binary Search Tree](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)

```C++
class Solution {
private:
    void traversal(TreeNode* root, unordered_map<int, int>& map){
        if(root==NULL) return;
        map[root->val]++;
        traversal(root->left, map);
        traversal(root->right, map);
    }

bool static cmp(const pair<int, int>&a, const pair<int, int> &b){
    return a.second > b.second;
}

public:
    vector<int> findMode(TreeNode* root) {
        vector<int> res;
        unordered_map<int, int> map;
        traversal(root, map);
        vector<pair<int, int>> vec(map.begin(), map.end());
        sort(vec.begin(), vec.end(), cmp);
        int max = vec[0].second;
        res.push_back(vec[0].first);
        for(int i = 1; i < vec.size(); i++){
            if(vec[i].second==max) res.push_back(vec[i].first);
            else break;
        }
        return res;
    }
};
```

### [235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root==NULL) return root;
        if (root->val > p->val && root->val > q->val) {
            return lowestCommonAncestor(root->left, p, q);
        } else if (root->val < p->val && root->val < q->val) {
            return lowestCommonAncestor(root->right, p, q);
        } else return root;
    }
};
```

### [701. 二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

```C++
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if(root==NULL) {
            TreeNode* node = new TreeNode(val);
            return node;
        }
        if(val < root->val) root->left = insertIntoBST(root->left, val);
        if(val > root->val) root->right = insertIntoBST(root->right, val);
        return root;
    }
};
```

### [450. 删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)

```C++
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if(root==NULL) return root;
        if(root->val == key) {
            if(root->left == NULL & root->right == NULL) {
                delete root;
                return NULL;
            }
            else if(root->left == NULL) {
                TreeNode* next = root->right;
                delete root;
                return next;
            }
            else if(root->right == NULL) {
                TreeNode* next = root->left;
                delete root;
                return next;
            }
            else{
                TreeNode* next = root->right;
                TreeNode* cur = next;
                while(next->left!=NULL){
                    next = next->left;
                }
                next->left = root->left;
                delete root;
                return cur;
            }            
        }
        else if (root->val < key) {
            root->right = deleteNode(root->right, key);
        } else {
            root->left = deleteNode(root->left, key);
        }
        return root;
    }
};
```

### [669. 修剪二叉搜索树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)

```C++
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (root==NULL) return NULL;
        if (root->val < low) return trimBST(root->right, low, high);
        if (root->val > high) return trimBST(root->left, low, high);
        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);
        return root;
    }
};
```

### [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

```C++
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        int size = nums.size();
        if(size==0) return NULL;// don't forget
        TreeNode* root = new TreeNode(nums[size/2]);
        vector<int> new_vec(nums.begin(), nums.begin()+(size/2));   
        vector<int> new_vec1(nums.begin()+(size/2)+1, nums.end());
        root->left = sortedArrayToBST(new_vec);
        root->right = sortedArrayToBST(new_vec1);
        return root;
    }
};
```

### [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

```C++
class Solution {
private:
    int pre;
    void traversal(TreeNode* cur){
        if(cur==NULL) return;
        traversal(cur->right);
        cur->val += pre;
        pre = cur->val;
        traversal(cur->left);
    }
public:
    TreeNode* convertBST(TreeNode* root) {
        pre = 0;
        traversal(root);
        return root;
    }
};
```

```C++
class Solution {
private:
    int pre;
    void traversal(TreeNode* cur){
        stack<TreeNode*> stk;
        while(cur!=NULL || !stk.empty()){
            if(cur!=NULL){
                stk.push(cur);
                cur = cur->right;
            } else{
                cur = stk.top();
                stk.pop();
                cur->val += pre;
                pre = cur->val;
                cur = cur->left;
            }
        }
    }
public:
    TreeNode* convertBST(TreeNode* root) {
        pre = 0;
        traversal(root);
        return root;
    }
};
```



# Backtracking Method

```C++
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```



### 77. Combinations

https://leetcode.com/problems/combinations/

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> res;
    void backtracking(int n, int k, int startInt){
        if(path.size() == k){
            res.push_back(path);
            return;
        }
        for(int i = startInt; i <= n; i++){
            path.push_back(i);
            backtracking(n, k, i+1);
            path.pop_back();
        }        
    }
public:
    vector<vector<int>> combine(int n, int k) {
        res.clear();
        path.clear();
        backtracking(n, k, 1);
        return res;
    }
};
```

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> res;
    void backtracking(int n, int k, int startInt){
        if(path.size() == k){
            res.push_back(path);
            return;
        }
        for(int i = startInt; i <= n-(k-path.size())+1; i++){//pruning
            path.push_back(i);
            backtracking(n, k, i+1);//here is i+1 not startInt+1
            path.pop_back();
        }        
    }
public:
    vector<vector<int>> combine(int n, int k) {
        res.clear();
        path.clear();
        backtracking(n, k, 1);
        return res;
    }
};
```



### 216. Combination Sum III

[216. 组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/)

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> res;
    void backtracking(int k, int n, int startInt) { 
        if(path.size() == k) {      
            int sum = accumulate(path.begin(), path.end(), 0);//快速求和
            if(sum == n){
                res.push_back(path);               
            }  
            return;         
        }      
        for(int i = startInt; i <= 10-(k-path.size()); i++) {//剪枝
            path.push_back(i);          
            backtracking(k, n, i+1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum3(int k, int n) {       
        backtracking (k, n, 1);
        return res;
    }
};
```



### 17. Letter Combinations of a Phone Number

[17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

```C++
class Solution {
private:   
    string path;
    vector<string> res;
    const string myMap[8] = {"abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};//const
    void backtracking(int k, int n, string digits){       
        if(path.size() == k){
            res.push_back(path);
            return;
        }
        int m = digits[n] - '0';
        string s = myMap[m-2];
        for(int j = 0; j < s.size(); j++){            
            path.push_back(s[j]);                       
            backtracking(k, n + 1, digits);
            path.pop_back();
        }
    }
public:
    vector<string> letterCombinations(string digits) {        
        int k = digits.size();
        if(k==0){return res;}
        backtracking(k, 0, digits);
        return res;
    }
};
```



### 39. Combination Sum

[39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> res;
    void backtracking(vector<int>& candidates, int target, int startInt,  int temp){
        if(temp == target){
            res.push_back(path);
            return;
        }    
        for(int i = startInt; i < candidates.size() && temp + candidates[i] <= target; i++){
            temp += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target, i, temp);
            temp -= candidates[i];
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());      
        if(candidates[0]>target){
            return res;
        }
        backtracking(candidates, target, 0, 0);
        return res;
    }
};
```

### 40. Combination Sum II

[40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> res;
    void backtracking(vector<int>& candidates, int target, int startInt,  int temp){
        if(temp == target){
            res.push_back(path);
            return;
        }    
        for(int i = startInt; i < candidates.size() && temp + candidates[i] <= target; i++){
            if (i > startInt && candidates[i] == candidates[i - 1]) { continue; } //only change
            temp += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target, i+1, temp);
            temp -= candidates[i];
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());      
        if(candidates[0]>target){
            return res;
        }
        backtracking(candidates, target, 0, 0);
        return res;
    }
};
```

### [131. 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

```C++
class Solution {
private:
    vector<string> path;
    vector<vector<string>> res;
    void backtracking(string s, int startIndex) {
        if (startIndex >= s.size()) {
            res.push_back(path);
            return;
        }
        for (int k = startIndex; k < s.size(); k++) {
            if (check(s, startIndex, k)) {
                string str = s.substr(startIndex, k-startIndex+1);//k-startIndex+1 is the length 
                path.push_back(str);
            } else continue;
            backtracking(s, k+1);
            path.pop_back();
        }
    }
    bool check(const string& s, int start, int end){
        for (int i = start, j = end; i < j; i++, j--) {
            if(s[i] != s[j]) return false;
        }
        return true;
    }   
public:
    vector<vector<string>> partition(string s) {
        path.clear();
        res.clear();
        backtracking(s, 0);
        return res;
    }
};
```

### [93. 复原 IP 地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

```C++
class Solution {
private:
    vector<string> res;
    void backtracking (string& s, int startIndex, int times) {
        if (times==3) {
            if(check(s, startIndex, s.size()-1)){
                res.push_back(s);
            }           
            return;
        }
        for (int i = startIndex; i < s.size(); i++) {
            if (check(s, startIndex, i)) {
                s.insert(s.begin() + i + 1 , '.');
                times++;
                backtracking(s, i+2, times);
                times--;
                s.erase(s.begin()+i+1);
            } else break;
        }
    }
    bool check(const string& s, int start, int end) {     
        if (start > end) { return false; }
        if (s[start]=='0' && end-start != 0) return false;
        int num = 0;
        for (int i = start; i <= end; i++) {
            if(s[i] < '0' || s[i] > '9') return false;
            num = num*10 + (s[i]-'0');
            if(num > 255) return false;
        }
        return true;
    }
public:
    vector<string> restoreIpAddresses(string s) {
        res.clear();
        backtracking(s, 0, 0);
        return res;
    }
};
```

### [78. 子集](https://leetcode-cn.com/problems/subsets/)

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> res;
    void backtracking(vector<int>& nums, int startIndex) {
        res.push_back(path);
        if(startIndex == nums.size()) {           
            return;
        }
        for(int i = startIndex; i < nums.size(); i++) {  
            path.push_back(nums[i]);                 
            backtracking(nums, i+1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        path.clear();
        res.clear();        
        backtracking(nums, 0);
        return res;
    }
};
```

### [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/)

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> res;  
    void backtracking(vector<int>& nums, int startIndex, vector<bool>& record){
        res.push_back(path);
        if (startIndex == nums.size()) {
            return;
        }
        for(int i = startIndex; i < nums.size(); i++) {
            if(startIndex < i && nums[i] == nums[i-1] && record[i-1] == false) {
                continue;
            }
            path.push_back(nums[i]);
            record[i] == true;
            backtracking(nums, i+1, record);
            record[i] == false;
            path.pop_back();
        }
    }
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        path.clear();
        res.clear();
        vector<bool> record(nums.size(), false);
        sort(nums.begin(), nums.end());
        backtracking(nums, 0, record);
        return res;
    }
};
```

### [491. 递增子序列](https://leetcode-cn.com/problems/increasing-subsequences/)

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> res;
    void backtracking(vector<int>& nums, int startIndex){
        if(path.size() > 1) {
            res.push_back(path);
        }
        unordered_set<int> uset;
        for(int i = startIndex; i < nums.size(); i++) {
            if(uset.find(nums[i])!=uset.end()){
                continue;
            }
            else if(!path.empty() && nums[i] < path.back()){
                continue;
            }
            uset.insert(nums[i]); 
            path.push_back(nums[i]);
            backtracking(nums, i+1);           
            path.pop_back();
        }
    }

public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        vector<bool> record;
        backtracking(nums, 0);
        return res;
    }
};
```



### [46. 全排列](https://leetcode-cn.com/problems/permutations/)

不包含重复数字的序列 nums

```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking (vector<int>& nums, vector<bool>& used) {
        // 此时说明找到了一组
        if (path.size() == nums.size()) {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (used[i] == true) continue; // path里已经收录的元素，直接跳过
            used[i] = true;
            path.push_back(nums[i]);
            backtracking(nums, used);
            path.pop_back();
            used[i] = false;
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        result.clear();
        path.clear();
        vector<bool> used(nums.size(), false);
        backtracking(nums, used);
        return result;
    }
};
```

### [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

可包含重复数字的序列 nums

同一树层使用过的相同数字需跳过

同一树枝使用过的相同位置数字需跳过

```cpp
used[i - 1] == true，说明同一树枝nums[i - 1]使用过
used[i - 1] == false，说明同一树层nums[i - 1]使用过
如果同一树层nums[i - 1]使用过则直接跳过
```

```C++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking (vector<int>& nums, vector<bool>& used) {       
        if (path.size() == nums.size()) {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (i>0 && used[i-1] == false && nums[i] == nums[i-1]) continue;
          //if (used[i] == true) continue;
            if(used[i] == false){
                used[i] = true;
                path.push_back(nums[i]);
                backtracking(nums, used);
                path.pop_back();
                used[i] = false;
            }           
        }
    }
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        result.clear();
        path.clear();
        vector<bool> used(nums.size(), false);
        sort(nums.begin(), nums.end());
        backtracking(nums, used);
        return result;
    }
};
```

### [332. 重新安排行程](https://leetcode-cn.com/problems/reconstruct-itinerary/)

```C++
class Solution {
private:
    vector<string> res;
    unordered_map<string, map<string, int>> my_map;
    bool find(vector<vector<string>>& tickets) {
        if(res.size() == tickets.size()+1) return true;
        for(pair<const string, int>& element : my_map[res[res.size()-1]]){//don't forget &
            if(element.second>0){
                element.second--;
                res.push_back(element.first);
                if(find(tickets)) return true;
                element.second++;
                res.pop_back();
            }
        }
        return false;      
    }
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        for(const vector<string>& vec : tickets) {
            my_map[vec[0]][vec[1]]++;
        }        
        res.push_back("JFK");
        find(tickets);
        return res;
    }
};
```



### [51. N 皇后](https://leetcode-cn.com/problems/n-queens/)

```C++
class Solution {
private:
    vector<vector<string>> res;  
    void backtracking(int n, int row, vector<string>& chessboard) {    
        if(row==n) {                      
            res.push_back(chessboard);
            return;                  
        }
        for(int col = 0; col < n; col++) {
            if(isValid(row, col, chessboard, n)){
                chessboard[row][col] = 'Q';
                backtracking(n, row + 1, chessboard);
                chessboard[row][col] = '.';
            } 
          return;
        }
    }
    bool isValid(int row, int col, vector<string>& chessboard, int n) {
        for(int i = 0; i < row; i++){
            if(chessboard[i][col] == 'Q'){
                return false;
            }           
        }
        for(int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if(chessboard[i][j] == 'Q'){
                return false;
            }            
        }
        for(int i = row - 1, j = col + 1; i >= 0 && j <= n - 1; i--, j++ ) {
            if(chessboard[i][j] == 'Q'){
                return false;
            }
        }
        return true;      
    }

public:
    vector<vector<string>> solveNQueens(int n) {
        vector<string> chessboard(n, string(n, '.'));
        backtracking(n, 0, chessboard);
        return res;
    }
};
```

### [126. 单词接龙 II](https://leetcode-cn.com/problems/word-ladder-ii/)

```C++
class Solution {
public:
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> dict(wordList.begin(), wordList.end()), current, next;
        if (dict.find(endWord) == dict.end()) {
            return {};
        }
        unordered_map<string, vector<string>> children;
        vector<vector<string>> ladders;//result
        vector<string> ladder;
        current.insert(beginWord);
        ladder.push_back(beginWord);
        while (true) {
            for (string word : current) {//if word exist in current, erase in dict
                dict.erase(word);
            }
            for (string word : current) {//find suitable word and put it in children(构造图的过程)
                findChildren(word, next, dict, children);
            }
            if (next.empty()) {
                break;
            }
            if (next.find(endWord) != next.end()) {//can find endWord in next
                genLadders(beginWord, endWord, children, ladder, ladders);
                break;
            }
            current.clear();
            swap(current, next);//current now store all the possible words becasue we want to find all 
        }
        return ladders;
    }
private:
    void findChildren(string word, unordered_set<string>& next, unordered_set<string>& dict, unordered_map<string, vector<string>>& children) {
        string parent = word;
        for (int i = 0; i < word.size(); i++) {
            char t = word[i];
            for (int j = 0; j < 26; j++) {
                word[i] = 'a' + j;
                if (dict.find(word) != dict.end()) {//exist in dict
                    next.insert(word);//possible words store in next
                    children[parent].push_back(word);//possible next word store in map
                }
            }
            word[i] = t;
        }
    }
//backtracking
    void genLadders(string beginWord, string endWord, unordered_map<string, vector<string>>& children, vector<string>& ladder, vector<vector<string>>& ladders) {
        if (beginWord == endWord) {
            ladders.push_back(ladder);
        } else {
            for (string child : children[beginWord]) {
                ladder.push_back(child);
                genLadders(child, endWord, children, ladder, ladders);
                ladder.pop_back();
            }
        }
    }
};
```

### [140. 单词拆分 II](https://leetcode-cn.com/problems/word-break-ii/)

```C++
class Solution {
public:
     vector<string> wordBreak(string s, vector<string>& wordDict) {
        unordered_map<string, vector<string>> m;
        return helper(s, wordDict, m);
    }
    vector<string> helper(string s, vector<string>& wordDict, unordered_map<string, vector<string>>& m){
        if(m.count(s)){
            return m[s];
        }
        if(s.empty()){
            return {""};
        }
        vector<string> res;
        for(auto word : wordDict){
            if(s.substr(0, word.size()) != word) continue;
            vector<string> temp = helper(s.substr(word.size()), wordDict, m);
            for(auto itm : temp){
                res.push_back(word+(itm.empty()?+"":" "+itm));
            }
        }
        m[s] = res;
        return res;
    }
    
};
```

### [37. 解数独](https://leetcode-cn.com/problems/sudoku-solver/)

```C++
class Solution {
private:
    bool backtracking(vector<vector<char>>& board){
        for(int i = 0; i < board.size(); i++) {
            for(int j = 0; j < board[0].size(); j++) {
                if(board[i][j] != '.') continue;
                for(char k = '1'; k <= '9'; k++) {                    
                    if(valid(i, j, k, board)){
                        board[i][j] = k;
                        if(backtracking(board)) return true;
                        board[i][j] = '.';
                    }
                }
                return false;
            }
        }
        return true;
    }
    bool valid(int row, int col, char k, vector<vector<char>>& board) {
        for(int i = 0; i < 9; i++) {
            if(board[row][i] == k) return false;
        }
        for(int j = 0; j < 9; j++) {
            if(board[j][col] == k) return false;
        }
        int new_row = (row/3)*3;
        int new_col = (col/3)*3;
        for(int m = new_row; m < new_row+3; m++){
            for(int n = new_col; n < new_col+3; n++){
                if(board[m][n] == k) return false;                
            }
        }
        return true;
    }
public:
    void solveSudoku(vector<vector<char>>& board) {
        backtracking(board);
        return;
    }
};
```

## Greedy Algorithm

### [455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

```C++
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int index = s.size()-1;
        int res = 0;
        for(int i = g.size()-1; i >= 0; i--) {
            if(index >= 0 && g[i] <= s[index]){
                index--;
                res++;
            }
        }
        return res;
    }
};
```

### [376. 摆动序列](https://leetcode-cn.com/problems/wiggle-subsequence/)

```C++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int res = 1;
        int pre = 0;
        int curr = 0;
        for(int i = 0; i < nums.size()-1; i++) {
            curr = nums[i+1] - nums[i];
            if((curr < 0 && pre >= 0) ||
            (curr > 0 && pre <= 0)){
                res++;
                pre = curr;
            }
        }
        return res;
    }
};
```



## Dynamic programming

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组

### [509. 斐波那契数](https://leetcode-cn.com/problems/fibonacci-number/)

```C++
class Solution {
public:
    int fib(int n) {
        if(n < 2){ return n; }
        int dp[2];
        dp[0] = 0;
        dp[1] = 1;
        for(int i = 2; i <= n; i++){
            int sum = dp[0] + dp[1];
            dp[0] = dp[1];
            dp[1] = sum;
        }
    return dp[1];
    }
};
```

### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

```C++
class Solution {
public:
    int climbStairs(int n) {
        if(n <= 1){
            return n;
        }
        int dp[2];
        dp[0] = 1;
        dp[1] = 2;
        for(int i = 3; i <= n; i++){
            int res = dp[0] + dp[1];
            dp[0] = dp[1];
            dp[1] = res;
        }
        return dp[1];
    }
};
```

### [746. 使用最小花费爬楼梯](https://leetcode-cn.com/problems/min-cost-climbing-stairs/)

```C++
//时间复杂度：O(n)
//空间复杂度：O(n)
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {    
        if(cost.size() == 1){
            return cost[0];
        }
        if(cost.size() == 2){
            return cost[0] <= cost[1] ? cost[0] : cost[1];
        }
        vector<int> dp(cost.size());
        dp[0] = cost[0];
        dp[1] = cost[1];
        for(int i = 2; i < cost.size(); i++){
            dp[i] = min(dp[i-1], dp[i-2]) + cost[i];            
        }
        return min(dp[cost.size()-2], dp[cost.size()-1]);
    }
};
//时间复杂度：O(n)
//空间复杂度：O(1)
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int dp0 = cost[0];
        int dp1 = cost[1];
        for (int i = 2; i < cost.size(); i++) {
            int dpi = min(dp0, dp1) + cost[i];
            dp0 = dp1; 
            dp1 = dpi;
        }
        return min(dp0, dp1);
    }
};
```



### [面试题 16.07. 最大数值](https://leetcode-cn.com/problems/maximum-lcci/)

编写一个方法，找出两个数字`a`和`b`中最大的那一个。不得使用if-else或其他比较运算符。

```C++
class Solution {
public:
    int maximum(int a, int b) {
        // 计算 int 类型的位数，避免不同系统下长度不同
        int bitlen = sizeof(a) * 8;

        // 计算 a 的符号位，b 的符号位
        // C/C++ 中负数右移最高位会补 1，因此需要转成无符号类型后再右移
        int asign = static_cast<unsigned>(a) >> (bitlen - 1);
        int bsign = static_cast<unsigned>(b) >> (bitlen - 1);
        // 假设 a 与 b 异号，计算 k 的值
        int k = asign ^ 1;

        // 当 a 和 b 异号时，asign ^ bsign ^ 1 为 0，由于 逻辑与运算 的短路性，将不再计算后半行代码，避免溢出
        // 当 a 和 b 同号时，asign ^ bsign ^ 1 为 1，此时会执行后半行代码，重新对 k 赋值
        int temp_cond = (asign ^ bsign ^ 1) && (k = static_cast<unsigned>(a - b) >> (bitlen - 1) ^ 1);
        return a * k + b * (k ^ 1);
    }
};
```





### [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

```C++
        priority_queue<int,vector<int>,greater<int> > q;

class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int,vector<int>,greater<int> > q;
        for(int i=0;i<nums.size();i++){
            if(q.size()<k){
                q.push(nums[i]);
            }
            else if(q.top()<nums[i]){
                    q.pop();
                    q.push(nums[i]);               
            }
        }
        return q.top();
    }
};
```



### [419. Battleships in a Board](https://leetcode-cn.com/problems/battleships-in-a-board/)

```C++
class Solution {
public:
    int countBattleships(vector<vector<char>>& board) {
        int count = 0;
        bool is_bs;
        for(int i = 0; i < board.size(); i++){
            for(int j = 0; j < board[0].size(); j++){
                if(board[i][j] == 'X'){
                    is_bs = true;
                    if((i>0 && board[i-1][j]=='X') || 
                    (j>0 && board[i][j-1]=='X')){
                        is_bs = false;
                    }
                    if(is_bs) count++;
                }               
            }
        }
        return count;
    }
};
```


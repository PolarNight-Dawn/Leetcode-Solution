# LeedCode

## 1.两数之和

## 2.两数相加

### Thought

#### Old：

分别遍历链表中l1，l2中的值，各自取出后还原成一个计算数，然后两数相加，令结果逆序插入新的链表

#### New：

同时遍历两个链表，逐位计算它们的和，并与当前位置的进位值相加，顺序插入新的链表，将链表翻转逆序输出

### Doubts&Gains

链表的底层原理

链表逆序插入，顺序取出

链表的基本操作

插入

```C++
//将新节点插入到已有链表的头部
ListNode* cur = new ListNode(val);
cur->next = l3;
l3 = cur;
//创建一个新的链表，并将新节点作为链表的头节点
ListNode *head = new ListNode(val);

ListNode* cur = l1;
    for (int i = 0; i < 5; i++) {
        cur->next = new ListNode(0);
        cur = cur->next;
    }
    cur->next = new ListNode(1);
//对于这个插入的操作有些不解，不是把l1复制到cur了吗，为什么最后的传参还是l1
```

### Code&Analysis

<old>

由于使用int，导致当测试用例过大时，结果崩溃

```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        size_t num1 = 0, num2 = 0, cnt1 = 0, cnt2 = 0, sum;
        ListNode* cur1 = l1;
        while (cur1 != nullptr) {
            size_t ps_val = cur1->val;
            for (int i = 1; i <= cnt1; i++) {
                ps_val = ps_val * 10;
            }
            num1 += ps_val;
            cur1 = cur1->next;
            cnt1++;
        }
        ListNode* cur2 = l2;
        while (cur2 != nullptr) {
            size_t ps_val = cur2->val;
            for (int i = 1; i <= cnt2; i++) {
                ps_val = ps_val * 10;
            }
            num2 += ps_val;
            cur2 = cur2->next;
            cnt2++;
        }
        sum = num1 + num2;

        ListNode* l3 = nullptr;
        if (sum == 0) {
            l3 = new ListNode(0);
        } else {
            std::string str = "";
            while (sum > 0) {
                char c = static_cast<char>(sum % 10 + '0');
                str = c + str;
                sum /= 10;
            }

            for (int i = 0; i <= str.length() - 1; i++) {
                int a = str[i] - '0';
                ListNode *newNode = new ListNode(a);
                newNode->next = l3;
                l3 = newNode;
            }
        }

        return l3;
    }
};
```

<new>

这里将链表翻转的操作时间和空间开销都很大

```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* l3 = nullptr;
        int carry = 0, val = 0;

        while (l1 || l2) {
            int num1 = l1 ? l1->val : 0;
            int num2 = l2 ? l2->val : 0;

            int sum = num1 + num2 + carry;
            val = sum % 10;
            carry = sum / 10;

            ListNode* cur = new ListNode(val);
            cur->next = l3;
            l3 = cur;

            if (l1) l1 = l1->next;
            if (l2) l2 = l2->next;
        }

        if (carry != 0) {
            ListNode* cur = new ListNode(carry);
            cur->next = l3;
            l3 = cur;
        }

        if (l3 == nullptr) {
            ListNode* cur = new ListNode(0);
        }
        
        //链表翻转
        ListNode* prev = nullptr;
        ListNode* curr = l3;
        while (curr != nullptr) {
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
};
```

## 3.无重复字符的最长子串

### Thought

#### Old：

找到每个重复字符的在字符串中的位置，两两比较，找出最大间隔的相邻重复字符，从字符串中取出不含重复字符间的子串，计算最长子串长度

#### New：

用哈希表来记录每个字符最后出现的位置，当遍历到某个字符时，通过哈希表来快速确定这个字符上一次出现的位置。然后，根据上一次出现的位置和当前位置的距离来计算当前的无重复子串的长度

### Error&Cause

<old>

> `interrupted by signal 11: SIGSEGV`是一个错误信息，它通常出现在程序崩溃的时候，提示了一个由操作系统发送给进程的信号。其中，SIGSEGV 表示“段错误”，它是由程序试图访问不属于它的内存区域时出现的错误。

通常情况下，这种错误是由以下几种情况引起的：

1. 空指针引用：当程序试图访问一个空指针时，就会发生 SIGSEGV 错误。
2. 访问已经释放的内存：当程序试图访问已经释放的内存时，就会发生 SIGSEGV 错误。
3. 访问越界：当程序试图访问超出了数组或指针所指内存空间的范围时，就会发生 SIGSEGV 错误。
4. 栈溢出：当程序使用了太多的栈空间时，就会发生栈溢出错误，也会产生 SIGSEGV 错误。

### Code&Analysis

<old>

时间复杂度较高，不太适合长字符串的处理，同时代码中使用了一个很大的数组a，会占用大量的内存空间

对于没有重复字符的字符串，程序无法处理

但是可以处理有多个不同的重复字符及相同的重复字符的情况

```c++
        //定义一个二维数组来储存不同的重复字符的位置
        int a[25000][50000] = {};
        int row = 0, col = 0;
        for (int i = 0; i < s.length(); i++) {
            if (col != 0) {
                col = 0;
                row++;
            }
            for (int j = i + 1; j < s.length(); j++) {
                if (s[i] == s[j]) {
                    if (col == 0) {
                        a[row][col] = i;
                        col++;
                        a[row][col] = j;
                    } else {
                        col++;
                        a[row][col] = j;
                    }
                }
            }
        }

        //取得最长子串的头尾位置str1，str2
        int num = 0, str1 = 0, str2 = 0;
        for (int i = 0; i < row; i++) {
             for (int j = 0; j < 49999; j++) {
                 int num1 = a[i][j + 1] - a[i][j];
                 if (num <= num1) {
                     num = num1;
                     str1 = j;
                     str2 = j + 1;
                 }
             }
        }

        //从字符串里取出不含重复字符的最长子串
        return s.substr(str1, str2 - str1);
        /*这里没有注意题目是取长度，代码应该是：
        int num = 0;
        for (int i = 0; i < row; i++) {
             for (int j = 0; j < 49999; j++) {
                 int num1 = a[i][j + 1] - a[i][j];
                 if (num <= num1) {
                     num = num1;
                 }
             }
        }
        
        return num*/
```

<new>

这种方法的时间复杂度为O(n)，空间复杂度为O(1)或O(min(n,m))，其中m表示字符集的大小

无法处理有多个不同的重复字符及相同的重复字符的情况

```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.length();
        unordered_set<char> hash_set;
        int i = 0, j = 0, ans = 0;
        while (i < n && j < n) {
            if (hash_set.find(s[j]) == hash_set.end()) {
                hash_set.insert(s[j++]);
                ans = max(ans, j - i);
            } else {
                hash_set.erase(s[i++]);
            }
        }
        return ans;
    }
};
```

<modify>

```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> hash;
        int ans = 0;
        int left = 0, right = 0;
        int n = s.size();
        while (right < n) {
            char c = s[right];
            if (hash.find(c) != hash.end() && hash[c] >= left) {
                left = hash[c] + 1;
            }
            hash[c] = right;
            ans = max(ans, right - left + 1);
            right++;
        }
        return ans;
    }
};
```

## 4.两个正序数组的中位数

### Thought

将nums1，nums2中的数按正序插入新的vector num中，根据num的元素个数求取中位数

### Doubts&Gains

>`if (nums1[i] == '\0')`使用了'\0'作为判断条件，这是错误的，因为'\0'表示的是字符串的结束符，在vector中是没有意义的。

### Code&Analysis

```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        vector<int> num = {};
        int m = nums1.size();
        int n = nums2.size();
        int size = m + n;
        int i = 0, j = 0;

        while (i < m && j < n) {
            if (nums1[i] < nums2[j]) {
                num.push_back(nums1[i]);
                i++;
            } else {
                num.push_back(nums2[j]);
                j++;
            }
        }

        while (i < m) {
            num.push_back(nums1[i]);
            i++;
        }

        while (j < n) {
            num.push_back(nums2[j]);
            j++;
        }

        double median = 0.0;
        if (size % 2 == 0) {
            median = (num[(size / 2)] + num[(size / 2 - 1)]) / 2.0;
        } else {
            median = num[(size + 1) / 2 - 1];
        }
        return median;
    }
};
```


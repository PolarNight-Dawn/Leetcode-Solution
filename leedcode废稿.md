# leedcode废稿

## 3.无重复字符的最长子串

### old

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

### new

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

## 10.正则表达式匹配

```c++
class Solution {
public:
    bool isMatch(string s, string p) {
        if (s[0] != p[0] && p[0] != '.') return false;

        if (p[0] == '*') return false;

        int i = 0;
        while (i < s.size() - 1 & p[i] != '\0') {
            if (p[i] >= 'a' & p[i] <= 'z') {
                if (s[i] == p[i]) {
                    int temp = i;
                    i++;
                    if (p[i] == '*') {
                        p[i] = p[temp];
                        if (s[i] == p[i]) {
                            i++;
                        } else {
                            return false;
                        }
                    } //! \不能处理：a*匹配“”，“a”，“aaa/.../”字符串的情况
                } else {
                    return false;
                }
            } else if (p[i] == '.') {
                i++;
                if (p[i] == '*') {
                    return true;
                }
            } else if (p[i] == '*') {
                return false;
            }
        }

        while (!p[i]) return false;

        return true;
    }
};
```

## 24.两两交换链表中的节点

```C++
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head == nullptr || head->next == nullptr) {
            return head;
        }

        ListNode* newHead = head->next;
        ListNode* cur = head;
        ListNode* pre = nullptr;
        ListNode* log = nullptr;

        int i = 1;
        while (cur != nullptr) {
            if (i % 2 != 0) {
                pre = cur;
                cur = cur->next;
                i++;
            } else {
                ListNode* temp = cur->next;
                cur->next = pre;
                pre->next = temp;
                if (pre != head) {
                    log->next = cur;
                }

                log = cur;
                pre = cur;
                cur = temp;
                i++;
            }
        }
        return newHead;
    }
};
```


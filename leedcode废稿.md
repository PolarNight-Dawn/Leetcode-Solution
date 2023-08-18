# LeedCode废稿

**废稿的作用在于找到代码未能实现的原因，重点理解，可以修改的就修改**

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

## 20.有效的括号

```C++
class Solution {
public:
    bool isValid(string s) {
        vector<int> vecSLeft, vecSRight, vecMLeft, vecMRight, vecLLeft, vecLRight;
        int len = s.length();

        for (int i = 0; i < len; i++) {
            if (s[i] == '{') {
                if (vecSLeft.empty() != 0 && vecMLeft.empty() != 0) {
                    return false;
                }
                vecLLeft.push_back(i);
            } else if (s[i] == '[') {
                if (vecSLeft.empty() != 0) {
                    return false;
                }
                if (vecLLeft.empty() != 0 && vecLLeft.back() < i) {
                    vecMLeft.push_back(i);
                }
                if (vecLLeft.empty() == 0) {
                    vecMLeft.push_back(i);
                }
                return false;
            } else if (s[i] == '(') {
                if (vecMLeft.empty() != 0 && vecMLeft.back() < i) {
                    vecSLeft.push_back(i);
                }
                if (vecMLeft.empty() == 0 && vecLLeft.empty() != 0 && vecLLeft.back() < i) {
                    vecSLeft.push_back(i);
                }
                if (vecMLeft.empty() == 0 && vecLLeft.empty() == 0) {
                    vecSLeft.push_back(i);
                }
                return false;
            } else if (s[i] == ')') {
                if (vecLRight.empty() != 0 && vecMRight.empty() != 0 && vecSLeft.empty() != 0) {
                    return false;
                }
                if (vecSLeft.back() < i) {
                    vecSRight.push_back(i);
                }
                return false;
            } else if (s[i] == ']') {
                if (vecLRight.empty() != 0 && vecMLeft.empty() != 0)  {
                    return false;
                }
                if (vecSRight.empty() != 0 && vecSRight.back() < i) {
                    vecMRight.push_back(i);
                }
                if (vecSRight.empty() == 0) {
                    vecMRight.push_back(i);
                }
                return false;
            } else if (s[i] == '}') {
                if (vecLLeft.empty() != 0)  {
                    return false;
                }
                if (vecMLeft.empty() != 0 && vecMRight.back() < i) {
                    vecLRight.push_back(i);
                }
                if (vecMLeft.empty() == 0 && vecSLeft.empty() != 0 && vecSRight.back() < i) {
                    vecLRight.push_back(i);
                }
                if (vecMLeft.empty() == 0 && vecSLeft.empty() == 0) {
                    vecLRight.push_back(i);
                }
                return false;
            }
        }
        return true;
    }
};
```

## 22.括号生成

递归调用 `backTracking` 函数可能导致栈溢出的问题。

当递归调用次数过多时，函数的局部变量和调用栈的信息会占用大量的栈空间，超过了系统所分配给程序的栈空间大小，从而导致栈溢出错误。

这是由于错误的限定条件，会总是选择走`'{'`导致一直调用 `backTracking` 函数

```c++
class Solution {
public:
    unordered_map<int, char> map = {{0, '{'},
                                    {1, '}'}};
    vector<string> res;

    string str = "{";

    void backTracking(string str, int left, int right, int index, int n) {
        int balance = left - right;
        if (index == 2 * n && balance == 0) {
            res.push_back(str);
            return;
        }

        if (balance < 0) {
            return;
        }

        for (int i = 0; i < 2; i++) {
            str.push_back(map[i]);
            if (i == 0) {
                left++;
                backTracking(str, left, right, index + 1, n);
                left--;
            }
            if (i == 1) {
                right++;
                backTracking(str, left, right, index + 1, n);
                right--;
            }
            str.pop_back();
        }
    }

    vector<string> generateParenthesis(int n) {
        str.clear();
        res.clear();
        backTracking(str, 0, 0, 1, n);
        return res;
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

## 54.螺旋矩阵

`WA`

```c++
class Solution {
public:
    std::vector<int> spiralOrder(std::vector<std::vector<int>>& matrix) {
        std::vector<int> res;
        int m = matrix.size();
        int n = matrix[0].size();
        int numTurn = (m + 1) / 2;

        int cnt = 0;
        while (cnt <= numTurn) {
            if (m - cnt == 1 + cnt) {
                for (int j = cnt; j < n - cnt; j++) res.push_back(matrix[cnt][j]);
                break;
            }
            if (cnt == m - 2 - cnt) {
                for (int i = 1 + cnt; i < m - cnt; i++) res.push_back(matrix[i][n - 1 - 2 * cnt]);
                break;
            }
            for (int j = cnt; j < n - cnt; j++) res.push_back(matrix[cnt][j]);
            for (int i = 1 + cnt; i < m - cnt; i++) res.push_back(matrix[i][n - 1 - 2 * cnt]);
            for (int j = n - 2 - cnt; j >= cnt; j--) res.push_back(matrix[m - 1 - 2 * cnt][j]);
            for (int i = m - 2 - cnt; i > cnt; i--) res.push_back(matrix[i][cnt]);
            cnt++;
        }

        return  res;
    }
};
```

## 80.删除有序数组中的重复项 II

 ```C++
 class Solution {
 public:
     int removeDuplicates02(std::vector<int>& nums) {
         int cnt = 0;
         for (int i = 0; i < nums.size(); i++) {
             for (int j = i + 1; j < nums.size(); j++) {
                 while (nums[i] == nums[j++]) {
                     cnt++;
                     if (cnt >= 2) {
                         nums.erase(nums.begin() + j - 1);
                         cnt--;
                     }
                 }
                 i = j - 1;
             }
         }
 
         return nums.size();
     }
 };
 ```

## 82.删除排序链表中的重复元素 II

```c++
class Solution {
public:
    ListNode *deleteDuplicates(ListNode *head) {
        ListNode *pre = nullptr;
        ListNode *cur = head;
        ListNode *newHead = new ListNode(0);
        ListNode *tmp = newHead;
        if (cur == nullptr || cur->next == nullptr) return head;

        while (cur != nullptr && cur->next != nullptr) {
            if (cur->val == cur->next->val) while (cur->next != nullptr && cur->val == cur->next->val) cur = cur->next;
            else {
                pre = cur;
                tmp->next = pre;
                tmp = tmp->next;
            }
            if (pre != nullptr) pre->next = cur->next;
            cur = cur->next;
        }

        return newHead->next;
    }
};
```

## 85.最大矩阵

```c++
class Solution {
public:
    int maximalRectangle(std::vector<std::vector<char>> &matrix) {
        int m = matrix.size();
        int n = matrix[0].size();

        int maxSize = 0, matrixLenth = 0, matrixWidth = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == '1') {
                    bool flag = true;
                    matrixLenth = i + 1, matrixWidth = j + 1;
                    while (matrixLenth < m && matrix[matrixLenth][j] == '1') matrixLenth++;
                    while (matrixWidth < n && matrix[i][matrixWidth] == '1') matrixWidth++;
                    if (matrixWidth - j != 1) {
                        for (int x = i; x < matrixLenth; x++) {
                            if (std::find(matrix[x].begin() + j, matrix[x].begin() + j + matrixLenth, '0') != matrix[x].begin() + j + matrixLenth) {
                                flag = false;
                                break;
                            }
                        }
                    } else {
                        for (int x = i; x < matrixLenth; x++) {
                            if (matrix[x][j] == '0') {
                                flag = false;
                                break;
                            }
                        }
                    }
                    if (!flag) break;
                    int size = (matrixLenth - i) * (matrixWidth - j);
                    maxSize = std::max(size, maxSize);
                }
            }
        }

        return maxSize;
    }
};
```


# LeedCode

## 1.两数之和

### Thought

遍历数组查找是否有两个元素的和等于给定的目标值target，如果找到了这样的一对元素，就返回它们的下标

### Doubts&Gains

>`std::distance()` 是 C++ 标准库中的一个函数，它用于计算两个迭代器之间的距离（即它们之间的元素数）。它通常被用于计算容器中元素的数量或者用于迭代器算法中。`std::distance()` 接受两个迭代器作为参数，这两个迭代器标识了要计算距离的元素范围。函数的返回值是一个整数，表示这两个迭代器之间的元素数。

> C++11 引入了一个新的规则，称为 “收窄规则”（narrowing rule），该规则禁止将一个精度更高的整数类型强制转换为一个精度更低的整数类型，除非显式地进行强制类型转换。

### Code&Analysis

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::size_t count = std::distance(std::begin(nums), std::end(nums));
        for (size_t i = 0; i < count; i++) {
            for (size_t j = 0; j < count; j++) {
                if (i != j) {
                    if(target == nums[i] + nums[j]) {
                        std::cout << i << "," << j << std::endl;
                        return {static_cast<int>(i), static_cast<int>(j)};
                        break;
                    }
                }
            }
        }
        return {};
    }
};
```

#### advanced stage

一个时间复杂度小于 `O(n2)` 的算法

- 为什么会想到用哈希表
- 哈希表为什么用map
- 本题map是用来存什么的
- map中的key和value用来存什么的

## 2.两数相加

### Thought

#### old：

分别遍历链表中l1，l2中的值，各自取出后还原成一个计算数，然后两数相加，令结果逆序插入新的链表

#### new：

同时遍历两个链表，逐位计算它们的和，并与当前位置的进位值相加，顺序插入新的链表，将链表翻转逆序输出

### Doubts&Gains

> 插入 将新节点插入到已有头部的链表ListNode* cur = new ListNode(val);cur->next = l3;l3 = cur;`
> 创建一个新的链表，并将新节点作为链表的头节点`ListNode *head = new ListNode(val);`

```C++
ListNode* cur = l1;
    for (int i = 0; i < 5; i++) {
        cur->next = new ListNode(0);
        cur = cur->next;
    }
    cur->next = new ListNode(1);
//对于这个插入的操作有些不解，不是把l1复制到cur了吗，为什么最后的传参还是l1
//l1是个指针，指向首个节点，这里是将了l1存储的地址复制给指针cur，这样操作的目的是为了保存l1存储的地址
```

### Code&Analysis

#### old

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

#### new

这里将链表翻转的操作时间和空间开销都很大

## 3.无重复字符的最长子串

### Thought

#### old：

找到每个重复字符的在字符串中的位置，两两比较，找出最大间隔的相邻重复字符，从字符串中取出不含重复字符间的子串，计算最长子串长度

#### new：

用哈希表来记录每个字符最后出现的位置，当遍历到某个字符时，通过哈希表来快速确定这个字符上一次出现的位置。然后，根据上一次出现的位置和当前位置的距离来计算当前的无重复子串的长度

### Doubts&Gains

<old>

> `interrupted by signal 11: SIGSEGV`是一个错误信息，它通常出现在程序崩溃的时候，提示了一个由操作系统发送给进程的信号。其中，SIGSEGV 表示“段错误”，它是由程序试图访问不属于它的内存区域时出现的错误。

通常情况下，这种错误是由以下几种情况引起的：

1. 空指针引用：当程序试图访问一个空指针时，就会发生 SIGSEGV 错误。
2. 访问已经释放的内存：当程序试图访问已经释放的内存时，就会发生 SIGSEGV 错误。
3. 访问越界：当程序试图访问超出了数组或指针所指内存空间的范围时，就会发生 SIGSEGV 错误。
4. 栈溢出：当程序使用了太多的栈空间时，就会发生栈溢出错误，也会产生 SIGSEGV 错误。

>`unordered_map<char, int> hash`是一个用来存储字符及其对应的出现次数的哈希表。在这个哈希表中，每个字符都被映射到一个整数值，表示该字符在字符串中出现的次数。

这个哈希表可以用来解决许多字符串相关的问题，例如：

- 统计一个字符串中每个字符出现的次数
- 判断两个字符串是否为变位词（即每个字符出现的次数都相同）
- 找到一个字符串中第一个不重复的字符
- 找到一个字符串中最长的没有重复字符的子串

等等。通过使用哈希表，我们可以在常数时间内访问、插入、删除键值对，因此哈希表是一种非常高效的数据结构，尤其是在处理大规模数据时。

> `unordered_set` 是 C++ 中的一个容器，它可以存储不重复的元素，并且可以快速地进行插入、查找和删除等操作。它的实现方式是哈希表，所以插入、查找和删除的平均时间复杂度都是 O(1)。

### Code&Analysis

#### old

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

#### new

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

#### modify

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

## 5.最长回文子串😭

### Thought

使用动态规划（Dynamic Programming）的思想，来减少运行时间。

具体来说，我们可以使用一个二维数组$dp$，其中 $dp[i][j]$ 表示字符串 $s$ 中从 $i$ 到 $j$ 的子串是否为回文串。对于任意一个子串 $s[i, j]$，如果该子串是回文串，那么它的左右两端分别减少一个字符仍然是回文串，即 $dp[i + 1][j - 1]$ 为真，且 $s[i] = s[j]$。

根据这个思路，我们可以使用一个变量 $maxLen$ 来记录当前找到的最长回文子串的长度，以及最长回文子串的起始位置 $start$。在更新 $dp$ 数组时，如果当前子串是回文串并且长度大于 $maxLen$，那么就更新 $maxLen$ 和 $start$。

### Doubts&Gains

> `interrupted by signal 9: SIGKILL`这个错误信息通常表示进程被操作系统强制终止了。SIGKILL是一种用于向进程发送强制终止信号的系统信号。当进程消耗过多的系统资源或者执行时间过长时，操作系统可能会发送SIGKILL信号，以避免系统崩溃或出现其他问题。因此，如果程序收到了这个信号，就说明它已经被操作系统强制终止了。

> 动态规划是一种算法思想，通常用于解决具有重叠子问题和最优子结构性质的问题。动态规划算法将原问题分解成若干个子问题，先求解子问题，再由子问题的解得到原问题的解。
>
> 动态规划算法的基本思想是利用子问题的最优解来推导出原问题的最优解。它通常采用自底向上的方式，先求解最小的子问题，然后逐步求解更大的子问题，最终得到原问题的解。
>
> 动态规划算法常常使用一个表格来存储已经解决的子问题的最优解，以避免重复计算。这种表格通常被称为动态规划表或者状态转移表。

动态规划算法可以用于求解一些著名的问题，例如背包问题、最长公共子序列问题、最短路径问题、最优二叉搜索树问题等。

### Code&Analysis

```C++
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.length();
        if (n < 2) {
            return s;
        }

        int start = 0, maxLen = 1;
        vector<vector<bool>> dp(n, vector<bool>(n, false));

        for (int i = n-1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (s[i] == s[j]) {
                    if (j - i < 2) {
                        dp[i][j] = true;
                    } else {
                        dp[i][j] = dp[i+1][j-1];
                    }
                } else {
                    dp[i][j] = false;
                }

                if (dp[i][j] && j - i + 1 > maxLen) {
                    start = i;
                    maxLen = j - i + 1;
                }
            }
        }

        return s.substr(start, maxLen);
    }
};
```

## 6.N字形变换

### Thought

使用二维数组将字符串按Z字形储存，顺序遍历

### Doubts&Gains

> vector二维数组

### Code&Analysis

执行时间太长了，消耗内存过多

```c++
class Solution {
public:
    string convert(string s, int numRows) {
        int n = s.length();
        vector<vector<char>> buffer (numRows, vector<char>(n, '\0'));

        if (numRows == 1) {
            return s;
        }
        //Z字储存
        int cnt = 0;
        bool eof = false;
        while (cnt <= n - 1) {
            for (int i = 0; i < n; i++) {
                if (eof) {
                    break;
                }
                int jud = i % (numRows - 1);
                if (jud == 0) {
                    for (int j = 0; j < numRows; j++) {
                        if (cnt > n - 1) {
                            eof = true;
                            break;
                        }
                        buffer[j][i] = s[cnt];
                        cnt++;
                    }
                } else {
                    if (cnt > n - 1) {
                        eof = true;
                        break;
                    }
                    buffer[numRows - jud - 1][i] = s[cnt];
                    cnt++;
                }
            }
        }

        //顺序遍历
        string str = "";
        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j < n; j++) {
                if (buffer[i][j] != '\0') {
                    str.push_back(buffer[i][j]);
                }
            }
        }
        return str;
    }
};
```

## 7.整数翻转

### Thought

将一个整数拆分成数字，存入vector digit中，按某种顺序取出反转整数

### Doubts&Gains

> 在 C++ 中，`abs()` 是一个数学函数，用于计算一个整数或浮点数的绝对值。在 C++ 中，`abs()` 是一个数学函数，用于计算一个整数或浮点数的绝对值。`abs()` 函数返回的结果为一个非负数，即输入参数的绝对值。如果输入参数为负数，则返回其相反数。

需要注意的是，`abs()` 函数的参数必须是一个数字类型，否则会导致编译错误。如果需要计算复数或其他类型的数值的模或大小，请使用适当的数学库函数。

> 在C++中，`pow()`是一个数学函数，用于计算一个数的指定次幂。

- 需要注意的是，`pow()` 函数的参数和返回值都是浮点数类型。
- 需要注意的是，如果底数或指数非常大，`pow()` 函数可能会产生精度问题。如果需要高精度计算，可以使用其他的数学库函数或者自行实现高精度计算。

>`digits.insert()` 是 C++ 中向容器中插入元素的方法之一，用于向一个容器中插入元素。`digits.insert(digits.begin(), digit)`中`digits.begin()` 作为 `insert()` 函数的第一个参数，表示要在容器开头位置插入元素；`digit` 作为 `insert()` 函数的第二个参数，表示要插入的元素值。

```C++
// 在指定位置插入一个元素
iterator insert(const_iterator pos, const T& value);

// 在指定位置插入 n 个元素，元素值都为 value
void insert(const_iterator pos, size_type n, const T& value);

// 在指定位置插入区间 [first, last) 中的所有元素
template <class InputIt>
void insert(const_iterator pos, InputIt first, InputIt last);

// 在容器末尾插入一个元素
void push_back(const T& value);
```



>`static_cast<目标类型>(要转换的值)`是 C++ 中的一种类型转换方式，用于将一种类型的值转换为另一种类型。

需要注意的是，在进行类型转换时，需要确保转换是安全和合法的。如果类型转换不合法，编译器可能会给出警告或错误提示。

### Code&Analysis

执行时间还行，这段时间最高了--击败了44.66%，而消耗内存一如既往的大。。。

```C++
class Solution {
public:
    int reverse(int x) {
        int num = 0;
        int res = 0;
        double virRes = 0.0;
        bool jud = false;
        if (x > 0) {
            num = x;
        } else if (x == 0 | x == -2147483648) {
            return res;
        } else {
            num = abs(x);
            jud = true;
        }

        vector<int> digits;
        while (num > 0) {
            int digit = num % 10;
            digits.insert(digits.begin(), digit);
            num /= 10;
        }

        int len = digits.size();
        if (digits[len - 1] == 0) {
            for (int i = len - 2; i >= 0; i--) {
                double result = pow(10, i);
                virRes += digits[i] * result;
                if (virRes > 2147483647) {
                    return 0;
                }
                res = static_cast<int>(virRes);
            }
        } else {
            for (int i = len - 1; i >= 0; i--) {
                double result = pow(10, i);
                virRes += digits[i] * result;
                if (virRes > 2147483647) {
                    return 0;
                }
                res = static_cast<int>(virRes);
            }
        }

        if (res > 2147483647 | res < -2147483648) {
            return 0;
        } else {
            if (jud) {
                return -res;
            }
            return res;
        }
    }
};
```

## 8.字符串转换整数💕 

### Thought

1. 读入字符串并丢弃无用的前导空格
2. 检查下一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。 如果两者都不存在，则假定结果为正。
3. 读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
4. 将前面步骤读入的这些数字转换为整数（即，"123" -> 123， "0032" -> 32）。如果没有读入数字，则整数为 0 。必要时更改符号（从步骤 2 开始）。
5. 如果整数数超过 32 位有符号整数范围 [−2^31^, 2^31^ − 1]，需要截断这个整数，使其保持在这个范围内。具体来说，小于 −2^31^ 的整数应该被固定为 −2^31^ ，大于 2^31^ − 1 的整数应该被固定为 2^31^ − 1 。
6. 返回整数作为最终结果。

### Doubts&Gains

> `std::isspace()` 是 C++ 标准库中的一个函数，用于判断给定字符是否为空格字符。空格字符包括空格、制表符、换行符等空白符号。

需要注意的是，`std::isspace()` 函数对于非 ASCII 字符集中的字符可能会返回错误的结果。如果需要处理中文字符，请使用支持 Unicode 字符集的库或函数。

> 在 C++ 中，`at()` 是 std::vector 和 std::string 类的成员函数之一，用于访问容器中指定位置的元素。
>
> `at()` 函数会检查索引是否越界，如果索引越界，则会抛出 `std::out_of_range` 异常。与 `[]` 操作符不同，`[]` 操作符不进行越界检查，因此如果尝试访问不存在的元素，可能会导致程序出现未定义的行为。

> `size_t` 和 `int` 是 C++ 中常见的两种数据类型，它们有以下区别：

1. 数据范围：`size_t` 是无符号整数类型，用于表示对象的大小或索引值等非负整数。它的取值范围通常是和机器的指针大小相同，也就是说在 32 位系统中为 4 字节，在 64 位系统中为 8 字节，具体取值范围取决于编译器和机器架构。而 `int` 是带符号整数类型，通常使用 4 个字节表示，取值范围在 -2147483648 到 2147483647 之间。
2. 内存占用：`size_t` 通常需要更多的内存空间来存储，因为它是一个无符号整数类型，不需要存储符号位，所以会有更多的位数用于存储数值，而 `int` 是带符号整数类型，需要存储符号位，因此会有一个比 `size_t` 更小的位数用于存储数值。
3. 使用场景：`size_t` 通常用于表示数组或字符串的长度、内存块的大小、容器的大小等等。在 C++ 标准库中，很多容器和算法都使用 `size_t` 类型来表示元素数量或索引值。而 `int` 则更适合表示一般的整数值，比如计数器、标志位等。



### Code&Analysis

运行时间击败百分百，消耗内存击败24.89%，取得新高😍😍😍

```C++
class Solution {
public:
    int myAtoi(string s) {
        size_t i = 0;
        size_t len = s.length();
        while (i < len && isspace((s.at(i)))) {
            ++i;
        }

        bool flag = false;
        if (s[i] == '-') {
            ++i;
            flag = true;
        } else if (s[i] == '+') {
            ++i;
        }

        s = s.substr(i) ;
        int64_t result = 0;
        for (char c : s) {
            if (c >= '0' && c <= '9') {
                result = result * 10 + (c - '0');
                if (result > 2147483648) {
                    result = 2147483648;
                    break;
                }
            } else {
                break;
            }
        }

        if (flag) {
            return int(0 - result);
        }
        if (result == 2147483648) {
            result = 2147483647;
        }
        return int(result);
    }
};
```

## 9.回文数

### Thought

#### old：

使用string类型来存储数字的字符表示，然后将该字符串反转，最后比较两个字符串是否相等

#### new:

数字的一半反转，然后将反转后的一半与原始数字的另一半进行比较，如果它们是相同的，那么这个数字就是回文

### Doubts&Gains

>`reverse()` 是 C++ 标准库中的一个函数，用于翻转容器（如字符串、数组、向量等）中元素的顺序。该函数定义在头文件 `<algorithm>` 中，可以接受两个迭代器作为参数，表示容器的起始位置和结束位置，对这段区间内的元素进行翻转操作。

### Code&Analysis

#### old

```C++
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0) {
            return false;
        }

        string s = to_string(x);
        string r = s;
        reverse(r.begin(), r.end());

        return s == r;
    }
};
```

#### new

```C++
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }

        int revertedNumber = 0;
        while (x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }

        return x == revertedNumber || x == revertedNumber / 10;
    }
};
```

#### advanced stage

不将整数转为字符串来解决这个问题

```C++
class Solution {
public:
    bool isPalindrome(int x) {
        vector<char> buffer;
        if (x < 0) {
            return false;
        }
        if (x == 0) {
            return true;
        }

        int cnt = 0;
        while (x > 0) {
            int result = x % 10;
            char c = static_cast<char>(result + '0');
            buffer.push_back(c);
            x /= 10;
            cnt++;
        }

        int len = buffer.size();
        int i = 0, j = len - 1;
        bool flag = (cnt % 2 == 0) ? true : false;
        while (i != j) {
            if (flag) {
                if (buffer[i] != buffer[j]) {
                    return false;
                }
                if (i + 1 != j) {
                    i++;
                    j--;
                } else {
                    i = j;
                }
            } else {
                if (buffer[i] != buffer[j]) {
                    return false;
                }
                i++;
                j--;
            }
        }
        return true;
    }
};
```

## 10.正则表达式匹配😭

### Thought

#### old：



#### new：



### Doubts&Gains



### Code&Analysis

#### old

```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        // 保证每次出现字符*时，前面都匹配到有效字符
        if (p[0] == '*') {
            return false;
        }

        //字符规律中没有*
        int sLen = s.length();
        int pLen = p.length();
        if (p.find('*') == string::npos) {
            if (sLen != pLen) { // p与s不等长，则不匹配
                return false;
            }

            // 字符规律中没有.
            int cnt = p.find('.');
            if (cnt == string::npos) {
                if (p != s) { // p与s不相等，则不匹配
                    return false;
                } else {
                    return true;
                }
            }

            // 字符规律中有.
            p.erase(cnt, 1);
            s.erase(cnt, 1);
            if (p != s) { // 删去.与对应的字符，p与s不相等，则不匹配
                return false;
            } else {
                return true;
            }
        }

        // 字符规律中有*
        // 字符规律中有.
        int stamp1 = p.find('*');
        int stamp2 = p.find('.');
        if (stamp2 != string::npos && stamp2 + 1 == stamp1) { // 字符规律.*，表示除换行符以外的任何字符
            return true;
        }

        // .在*前面
        if (stamp2 != string::npos && stamp2 < stamp1) {
            // 删去.与对应的字符
            s.erase(stamp2, 1);
            p.erase(stamp2, 1);

            // 取p与s的*前面部分
            string s1 = s.substr(0, stamp1);
            string p1 = p.substr(0, stamp1);
            if (s1 != p1) { // p与s的*前面部分不相等，则不匹配
                return false;
            } else {
                if (stamp1 == sLen) {
                    return true;
                }
                for (int i = stamp1; i < sLen; i++) {
                    if (s[i] != p[stamp1 - 1]) {
                        return false;
                    }
                }
            }
        }

        // .在*后面
        if (stamp2 != string::npos && stamp2 > stamp1) {
            // 取p与s的*前面部分
            string s1 = s.substr(0, stamp1);
            string p1 = p.substr(0, stamp1);
            if (s1 != p1) { // p与s的*前面部分不相等，则不匹配
                return false;
            } else {
                for (int i = stamp1; i < sLen; i++) {
                    if (s[i] != p[stamp1 - 1]) {
                        if (s[i] == p[stamp1 + 1]) {
                            string s2 = s.substr(i);
                            string p2 = p.substr(stamp1 + 1);

                            int stamp3 = p2.find('.');
                            s2.erase(stamp3, 1);
                            p2.erase(stamp3, 1);
                            if (s2 == p2) {
                                return true;
                            } else {
                                return false;
                            }
                        }
                        return false;
                    }
                }
            }
        }

        // 字符规律中没有.
        if (stamp2 == string::npos) {
            // 取p与s的*前面部分
            string s1 = s.substr(0, stamp1);
            string p1 = p.substr(0, stamp1);
            if (s1 != p1) { // p与s的*前面部分不相等，则不匹配
                return false;
            } else {
                for (int i = stamp1; i < sLen; i++) {
                    if (s[i] != p[stamp1 - 1]) {
                        if (s[i] == p[stamp1 + 1]) {
                            string s2 = s.substr(i);
                            string p2 = p.substr(stamp1 + 1);
                            if (s2 == p2) {
                                return true;
                            } else {
                                return false;
                            }
                        }
                        return false;
                    }
                }
            }
        }
        return true;
    }
};
```

#### new

```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size();
        int n = p.size();

        auto matches = [&](int i, int j) {
            if (i == 0) {
                return false;
            }
            if (p[j - 1] == '.') {
                return true;
            }
            return s[i - 1] == p[j - 1];
        };

        vector<vector<int>> f(m + 1, vector<int>(n + 1));
        f[0][0] = true;
        for (int i = 0; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (p[j - 1] == '*') {
                    f[i][j] |= f[i][j - 2];
                    if (matches(i, j - 1)) {
                        f[i][j] |= f[i - 1][j];
                    }
                }
                else {
                    if (matches(i, j)) {
                        f[i][j] |= f[i - 1][j - 1];
                    }
                }
            }
        }
        return f[m][n];
    }
};
```

## 11.盛最多水的容器❤️

### Thought

#### old：

暴力枚举，将每两条垂直线与X轴的构成面积进行比较，取最大值

#### new：

双指针法，初始化双指针分别指向水槽左右两端，选定两板高度中的短板，向中间收窄一格，循环收窄，直至双指针相遇时跳出，更新面积最大值

### Code&Analysis

#### old

超出时间限制，双重for循环，时间复杂度O(n^2^)

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n = height.size();

        int size = 0;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (height[i] <= height[j]) {
                    size = max((j - i) * height[i], size);
                } else {
                    size = max((j - i) * height[j], size);
                }
            }
        }
        return size;
    }
};
```

#### new

执行时间击败52.24%，消耗内存击败84.97%😘 😘 😘 

```C++
class Solution {
public:
    int maxArea(vector<int> &height) {
        int n = height.size();

        int size = 0;
        int i = 0, j = n - 1;
        while (i < j) {
            if (height[i] <= height[j]) {
                size = max((j - i) * height[i], size);
                i++;
            } else {
                size = max((j - i) * height[j], size);
                j--;
            }
        }
        return size;
    }
};
```

## 12.整数转罗马数字

### Thought

哈希表，将六个罗马数字存入unordered_map，整数不同的情况与表中对比并输出相应字符，最后组成字符串并返回

### Doubts&Gains

>unordered_map是C++ STL提供的关联容器之一，用于实现快速查找键值对。它的初始化可以使用以下几种方式：

1. 默认初始化：可以直接创建一个空的unordered_map，例如：

   ```c
   std::unordered_map<int, std::string> myMap;
   ```

2. 初始化列表初始化：可以使用花括号{}来初始化unordered_map，例如：

   ```c
   std::unordered_map<int, std::string> myMap = {{1, "one"}, {2, "two"}, {3, "three"}};
   ```

3. 拷贝构造函数初始化：可以使用另一个已有的unordered_map来初始化新的unordered_map，例如：

   ```c
   std::unordered_map<int, std::string> myMap1 = {{1, "one"}, {2, "two"}, {3, "three"}};
   std::unordered_map<int, std::string> myMap2(myMap1);
   ```

4. 范围初始化：可以使用一个迭代器范围内的元素来初始化unordered_map，例如：

   ```c
   std::vector<std::pair<int, std::string>> vec = {{1, "one"}, {2, "two"}, {3, "three"}};
   std::unordered_map<int, std::string> myMap(vec.begin(), vec.end());
   ```

5. emplace函数初始化：可以使用emplace函数向unordered_map中添加元素，例如：

   ```c
   std::unordered_map<int, std::string> myMap;
   myMap.emplace(1, "one");
   myMap.emplace(2, "two");
   myMap.emplace(3, "three");
   ```

> `operator[]`函数：如果要查找的键存在于unordered_map中，则返回与该键关联的值，否则会自动创建一个具有默认值的元素并将其插入到unordered_map中。

`operator[]`函数还可以用于修改unordered_map中的元素的值。例如：

```c
std::unordered_map<std::string, int> myMap = {{"apple", 2}, {"banana", 3}, {"orange", 4}};
myMap["banana"] = 5;          // 修改键值为"banana"的元素的值为5
```

> `at()`函数：如果要查找的键存在于unordered_map中，则返回与该键关联的值，否则会抛出一个`std::out_of_range`异常。

`at()`函数还可以用于修改unordered_map中的元素的值。例如：

```c
std::unordered_map<std::string, int> myMap = {{"apple", 2}, {"banana", 3}, {"orange", 4}};
myMap.at("banana") = 5;          // 修改键值为"banana"的元素的值为5
```

### Code&Analysis

执行时间超过25.58%，消耗内存超过21.1%，这是我首次主动使用数据结构改善代码，还行👍👍👍

#### modify

这道题直接将阿拉伯数字对应的罗马数字输出就行了，没有必要单独去开辟空间存放罗马数字与阿拉伯数字的映射，改进之后执行时间击败82.58%，消耗内存击败67.95%，大道至简。就是代码行数有点多👀 

```C++
class Solution {
public:
    string intToRoman(int num) {
        string str = "";
        while (num > 0) {
            if (num >= 1000) {
                char c = 'M';
                str = str + c;
                num -= 1000;
            } else if (num >= 500) {
                if (num >= 900) {
                    string str1 = "CM";
                    str = str + str1;
                    num -= 900;
                } else {
                    char c = 'D';
                    str = str + c;
                    num -= 500;
                }
            } else if (num >= 100) {
                if (num >= 400) {
                    string str1 = "CD";
                    str = str + str1;
                    num -= 400;
                } else {
                    char c = 'C';
                    str = str + c;
                    num -= 100;
                }
            } else if (num >= 50) {
                if (num >= 90) {
                    string str1 = "XC";
                    str = str + str1;
                    num -= 90;
                } else {
                    char c = 'L';
                    str = str + c;
                    num -= 50;
                }
            } else if (num >= 10) {
                if (num >= 40) {
                    string str1 = "XL";
                    str = str + str1;
                    num -= 40;
                } else {
                    char c = 'X';
                    str = str + c;
                    num -= 10;
                }
            } else if (num >= 5) {
                if (num >= 9) {
                    string str1 = "IX";
                    str = str + str1;
                    num -= 9;
                } else {
                    char c = 'V';
                    str = str + c;
                    num -= 5;
                }
            } else if (num >= 1) {
                if (num >= 4) {
                    string str1 = "IV";
                    str = str + str1;
                    num -= 4;
                } else {
                    char c = 'I';
                    str = str + c;
                    num -= 1;
                }
            }
        }
        return str;
    }
};
```

## 13.罗马数字转整数😋

### Thought

思路与12题相似，运用函数分段的思想，好像这里用哈希表是多余的

### Doubts&Gains

> `len = max(len, (int)strs[i + 1].size());`

>`unordered_set` 和 `unordered_map` 都是 C++ STL 中的关联容器，它们的底层实现都是哈希表。

它们之间的区别在于它们存储的数据类型不同：

- `unordered_set` 存储的是一组不重复的元素，不提供访问和修改元素的方式，因此可以用来快速判断一个元素是否在集合中出现过。
- `unordered_map` 存储的是一组键值对，每个键唯一对应一个值，提供了访问和修改元素的方式，因此可以用来快速查找某个键对应的值。

另外，`unordered_set` 和 `unordered_map` 在使用上还有一些细节的区别：

1. `unordered_set` 和 `unordered_map` 的声明方式不同。`unordered_set` 的声明方式如下：

   ```
   unordered_set<int> mySet;
   ```

   `unordered_map` 的声明方式如下：

   ```
   unordered_map<int, string> myMap;
   ```

   注意，`unordered_map` 存储的是一组键值对，因此需要同时指定键类型和值类型。

2. `unordered_set` 和 `unordered_map` 的元素访问方式不同。`unordered_set` 的元素只能被访问，不能被修改。可以使用迭代器来遍历集合中的元素，例如：

   ```
   for (auto it = mySet.begin(); it != mySet.end(); ++it) {
       cout << *it << " ";
   }
   ```

   `unordered_map` 的元素可以被访问和修改。可以使用键来访问值，例如：

   ```
   myMap[1] = "one";  // 插入一个键值对
   cout << myMap[1] << endl;  // 输出值 "one"
   ```

   上面的代码使用键值对 `[1, "one"]` 插入了一个元素，然后使用键 `1` 来访问该元素的值。

3. `unordered_set` 和 `unordered_map` 的查找方式不同。`unordered_set` 可以使用 `count()` 函数来判断一个元素是否在集合中出现过，例如：

   ```
   if (mySet.count(10)) {
       cout << "The element 10 is in the set" << endl;
   }
   ```

   `unordered_map` 可以使用 `find()` 函数来查找一个键对应的值，例如：

   ```
   auto it = myMap.find(1);
   if (it != myMap.end()) {
       cout << "The value of key 1 is " << it->second << endl;
   }
   ```

   上面的代码使用键 `1` 来查找 `myMap` 中的一个元素，并输出该元素的值。

综上所述，`unordered_set` 和 `unordered_map` 之间的主要区别在于存储的数据类型和元素的访问方式。`unordered_set` 存储的是一组不重复的元素，不能修改元素

### Code&Analysis

执行时间击败94.35%，消耗内存击败67.5%😋

```c++
class Solution {
public:
    int romanToInt(string s) {
        /*unordered_map<char, int> hash_map = {{'I', 1},
                                             {'V', 5},
                                             {'X', 10},
                                             {'L', 50},
                                             {'C', 100},
                                             {'D', 500},
                                             {'M', 1000}};
*/
        int len = s.size();
        int sum = 0;
        for (int i = 0; i < len; i++) {
            if (s[i] == 'I') {
                if (s[i + 1] == 'V') {
                    sum += 4;
                    i = i + 1;
                } else if (s[i + 1] == 'X') {
                    sum += 9;
                    i = i + 1;
                } else {
                    sum += 1;
                }
            } else if (s[i] == 'V') {
                sum += 5;
            } else if (s[i] == 'X') {
                if (s[i + 1] == 'L') {
                    sum += 40;
                    i = i + 1;
                } else if (s[i + 1] == 'C') {
                    sum += 90;
                    i = i + 1;
                } else {
                    sum += 10;
                }
            } else if (s[i] == 'L') {
                sum += 50;
            } else if (s[i] == 'C') {
                if (s[i + 1] == 'D') {
                    sum += 400;
                    i = i + 1;
                } else if (s[i + 1] == 'M') {
                    sum += 900;
                    i = i + 1;
                } else {
                    sum += 100;
                }
            } else if (s[i] == 'D') {
                sum += 500;
            } else if (s[i] == 'M') {
                sum += 1000;
            }
        }
        return sum;
    }
};
```



## 14.最长公共前缀😥

### Thought



### Doubts&Gains



### Code&Analysis



## 15.三数之和😥

### Thought



### Doubts&Gains



### Code&Analysis



## 16.最接近的三数之和❤️

### Thought

将数组排序，使用双指针指向边界上的两个元素，根据三数之和与target的值的关系，选择「抛弃」左边界的元素还是右边界的元素，更新差值的值，返回最小差值的三数

### Doubts&Gains

> `INT_MAX` 使用 INT_MAX 需要包含 <climits> 头文件。该头文件定义了整数类型的最大值和最小值，以及其他一些常量和宏定义。其中 INT_MAX 是一个宏定义，表示 int 类型的最大值。

### Code&Analysis

执行时间击败93.68%，消耗内存击败37.4%❤️

```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end()); // 排序
        int n = nums.size();
        int diff = INT_MAX; // 初始差值为无穷大
        int result = 0; // 记录结果
        for (int i = 0; i < n - 2; i++) {
            int left = i + 1;
            int right = n - 1;
            while (left < right) {
                int threeSum = nums[i] + nums[left] + nums[right];
                int curDiff = abs(threeSum - target); // 计算当前差值
                if (curDiff < diff) {
                    diff = curDiff;
                    result = threeSum;
                }
                if (threeSum < target) {
                    left++;
                } else if (threeSum > target) {
                    right--;
                } else {
                    return result;
                }
            }
        }
        return result;
    }
};
```

## 17.电话号码的字母组合😩

### Thought

回溯

### Doubts&Gains

>`vector<pair<int, vector<char>>>`，它是一个向量，其中每个元素都是一个 `pair`，其中第一个元素是 `int` 类型，第二个元素是 `vector<char>` 类型。

而 `vector<vector<char>>` 是一个二维向量，其中每个元素都是一个向量，每个子向量都包含一组 `char` 类型的元素。

因此，两者之间的主要区别在于它们的元素类型和向量的结构不同。`vector<pair<int, vector<char>>>` 包含一组具有整数和字符向量的键值对，而 `vector<vector<char>>` 包含一组具有字符向量的向量。

### Code&Analysis

超时😩

```C++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<pair<int, vector<char>>> buffer = { {2, {'a', 'b', 'c'}},
                                                   {3, {'d', 'e', 'f'}},
                                                   {4, {'g', 'h', 'i'}},
                                                   {5, {'j', 'k', 'l'}},
                                                   {6, {'m', 'n', 'o'}},
                                                   {7, {'p', 'q', 'r', 's'}},
                                                   {8, {'t', 'u', 'v'}},
                                                   {9, {'w', 'x', 'y', 'z'}}
        };

        int len = digits.length();
        int cnt = 0;
        vector<string> res;
        while (cnt <= len) {
            int num = digits[cnt++] - '0';
            if (cnt == 1) {
                for (int i = 0; i < buffer[num - 2].second.size(); i++) {
                    string str = "";
                    str += buffer[num - 2].second[i];
                    res.push_back(str);
                }
            } else {
                for (int i = 0; i < res.size(); i++) {
                    for (int j = 0; j < buffer[num - 2].second.size(); j++) {
                        string str = "";
                        str += res[i] +  buffer[num - 2].second[j];
                        res.push_back(str);
                    }
                }
            }

            for (int i = 0; i < res.size(); i++) {
                if (sizeof(res[i]) != cnt) {
                    res.erase(res.begin() + i);
                }
            }
        }
        return res;
    }
};
```

<回溯>

执行时间击败34.1%，消耗内存击败70.35%

## 18.四数之和

### Thought

这道题与三数之和有异曲同工之妙，只需将其中两数之和看做第三数，即可套用三数之和的思路

### Doubts&Gains

> `res.push_back({nums[i], nums[j], nums[left], nums[right]});`

### Code&Analysis

执行时间击败5.2%，消耗内存击败6.97%

## 19.删除链表的倒数第 N 个结点

### Thought

这道题比较简单，先循环一次拿到链表的总长度，然后循环到要删除的结点的前一个结点开始删除操作.需要注意的特例是头结点的处理

### Doubts&Gains

对于链表的处理还是显得有些生涩，需要反复练习

>`ListNode *cur = head;
>int i = len - n;
>while (i > 1) {
>    cur = cur->next;
>    i--;
>}
>cur->next = cur->next->next;`
>
>对于cur的创建还是只是知其然，不知其所以然

### Code&Analysis

执行时间击败27.49%，消耗内存击败43.55%
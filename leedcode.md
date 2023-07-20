# LeedCode

## 1.两数之和

### Thought

#### old：

遍历数组查找是否有两个元素的和等于给定的目标值target，如果找到了这样的一对元素，就返回它们的下标

#### new：

在遍历数组的时候，只需要向map去查询是否有和目前遍历元素匹配的数值，如果有，就找到的匹配对，如果没有，就把目前遍历的元素放进map中，因为map存放的就是我们访问过的元素

### Doubts&Gains

>`std::distance()`是 C++ 标准库中的一个函数，它用于计算两个迭代器之间的距离（即它们之间的元素数）。它通常被用于计算容器中元素的数量或者用于迭代器算法中。`std::distance()` 接受两个迭代器作为参数，这两个迭代器标识了要计算距离的元素范围。函数的返回值是一个整数，表示这两个迭代器之间的元素数。

> C++11 引入了一个新的规则，称为`“收窄规则”`（narrowing rule），该规则禁止将一个精度更高的整数类型强制转换为一个精度更低的整数类型，除非显式地进行强制类型转换。

### Code&Analysis

#### old

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

#### new

执行时间击败91.77%，消耗内存击败45.45%

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

> `插入 `
>
> `将新节点插入到已有头部的链表`ListNode* cur = new ListNode(val);cur->next = l3;l3 = cur; 
> `创建一个新的链表，并将新节点作为链表的头节点`ListNode *head = new ListNode(val);`
>
> >```c++
> >ListNode* cur = l1;
> >    for (int i = 0; i < 5; i++) {
> >        cur->next = new ListNode(0);
> >        cur = cur->next;
> >    }
> >    cur->next = new ListNode(1);
> >//对于这个插入的操作有些不解，不是把l1复制到cur了吗，为什么最后的传参还是l1
> >//l1是个指针，指向首个节点，这里是将了l1存储的地址复制给指针cur，这样操作的目的是为了保存l1存储的地址
> >//指针存储某个内存空间的地址,指针是一把访问内存的钥匙
> >```

> `翻转链表`
>
> > ```c++
> > ListNode* prev = nullptr;
> > ListNode* curr = l3;
> > while (curr != nullptr) {
> >     ListNode* next = curr->next;
> >     curr->next = prev;
> >     prev = curr;
> >     curr = next;
> > }
> > return prev;
> > ```

>`(interrupted by signal 15: SIGTERM)`是一个错误或警告消息，通常在计算机程序中出现。它指示进程（程序）被发送了一个特殊的信号（信号15），即SIGTERM。SIGTERM是一个终止信号，用于请求进程（程序）正常退出。
>
>>当程序接收到SIGTERM信号时，它应该执行一些清理操作并优雅地退出。这通常涉及保存进度、关闭文件、释放资源等。如果程序没有正确处理SIGTERM信号，它可能会被强制终止，可能导致数据丢失或其他不良影响。

### Code&Analysis

#### old

由于使用int，导致当测试用例过大时，结果崩溃(已修改为size_t)

未考虑进位的情况

时间复杂度为o(n+m),应采用更为简单的算法

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

执行时间击败25.75%，消耗内存击败37.5%

##### :zero:modify



## 3.无重复字符的最长子串

### Thought

#### old：

找到每个重复字符的在字符串中的位置，两两比较，找出最大间隔的相邻重复字符，从字符串中取出不含重复字符间的子串，计算最长子串长度

#### new：

用`哈希表`来记录每个字符最后出现的位置，当遍历到某个字符时，通过哈希表来快速确定这个字符上一次出现的位置。然后，根据上一次出现的位置和当前位置的距离来计算当前的无重复子串的长度

### Doubts&Gains

> `interrupted by signal 11: SIGSEGV`是一个错误信息，它通常出现在程序崩溃的时候，提示了一个由操作系统发送给进程的信号。其中，SIGSEGV 表示“段错误”，它是由程序试图访问不属于它的内存区域时出现的错误。
>
> >通常情况下，这种错误是由以下几种情况引起的：
> >
> >1. 空指针引用：当程序试图访问一个空指针时，就会发生 SIGSEGV 错误。
> >2. 访问已经释放的内存：当程序试图访问已经释放的内存时，就会发生 SIGSEGV 错误。
> >3. 访问越界：当程序试图访问超出了数组或指针所指内存空间的范围时，就会发生 SIGSEGV 错误。
> >4. 栈溢出：当程序使用了太多的栈空间时，就会发生栈溢出错误，也会产生 SIGSEGV 错误。

>`unordered_map<char, int> hash`是一个用来存储字符及其对应的出现次数的哈希表。在这个哈希表中，每个字符都被映射到一个整数值，表示该字符在字符串中出现的次数。
>
>>这个哈希表可以用来解决许多字符串相关的问题，例如：
>>
>>- 统计一个字符串中每个字符出现的次数
>>- 判断两个字符串是否为变位词（即每个字符出现的次数都相同）
>>- 找到一个字符串中第一个不重复的字符
>>- 找到一个字符串中最长的没有重复字符的子串
>>
>>等等。通过使用哈希表，我们可以在常数时间内访问、插入、删除键值对，因此哈希表是一种非常高效的数据结构，尤其是在处理大规模数据时。

> `unordered_set` 是 C++ 中的一个容器，它可以存储不重复的元素，并且可以快速地进行插入、查找和删除等操作。它的实现方式是哈希表，所以插入、查找和删除的平均时间复杂度都是 O(1)。

### Code&Analysis

#### old

leedcode废稿

#### new

leedcode废稿

##### modify

执行时间击败79.97%，消耗内存击败60.31%

## 4.两个正序数组的中位数

### Thought

将nums1，nums2中的数按正序插入新的vector num中，根据num的元素个数求取中位数

### Doubts&Gains

>`if (nums1[i] == '\0')`使用了'\0'作为判断条件，这是错误的，因为'\0'表示的是字符串的结束符，在vector中是没有意义的。

>`时间复杂度`指的是***随着输入大小的增长，运行时间会以怎样的速度扩张。***
>
>`O(log(N))`指的是***该算法随着输入规模翻倍，操作次数只增加一。每操作一次，需要处理的规模就小一半的。***

### Code&Analysis

时间复杂度为O(n+m)

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

#### advanced stage

这个题要求时间复杂度为O(log(n+m))，也就意味着需要采用二分法

执行时间击败63.36%，消耗内存击败67.34%

## 5.😭最长回文子串(DP)

### Thought

`dp`五部曲：

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组

### Doubts&Gains

> `interrupted by signal 9: SIGKILL`这个错误信息通常表示进程被操作系统强制终止了。SIGKILL是一种用于向进程发送强制终止信号的系统信号。当进程消耗过多的系统资源或者执行时间过长时，操作系统可能会发送SIGKILL信号，以避免系统崩溃或出现其他问题。因此，如果程序收到了这个信号，就说明它已经被操作系统强制终止了。

> 动态规划是一种算法思想，通常用于解决具有重叠子问题和最优子结构性质的问题。动态规划算法将原问题分解成若干个子问题，先求解子问题，再由子问题的解得到原问题的解。
>
> 动态规划算法的基本思想是利用子问题的最优解来推导出原问题的最优解。它通常采用自底向上的方式，先求解最小的子问题，然后逐步求解更大的子问题，最终得到原问题的解。
>
> 动态规划算法常常使用一个表格来存储已经解决的子问题的最优解，以避免重复计算。这种表格通常被称为动态规划表或者状态转移表。
>
> > 动态规划算法可以用于求解一些著名的问题，例如背包问题、最长公共子序列问题、最短路径问题、最优二叉搜索树问题等。

### Code&Analysis

执行时间击败10.2%，消耗内存击败54.89%

#### :zero:modify



## 6.N字形变换

### Thought

#### old：

对于numRows==1或者s.size（）<= numRows的情况，直接返回s

对于其他情况，创建一个二维矩阵，然后在矩阵上按Z字形填充，最后顺序扫描矩阵中非空字符，组成答案

#### new：

将矩阵的每行初始化为一个空列表，每次向某一行添加字符时，直接添加到该行末尾即可

### Doubts&Gains

> 二维`vector`的访问
>
> ```C++
> std::vector<std::vector<int>> matrix = {
>     {1, 2, 3},
>     {4, 5, 6},
>     {7, 8, 9}
> };
> ```
>
> >使用双重索引
> >
> >```C++
> >int element = matrix[0][1];  
> >```
>
> > 使用`at()`函数
> >
> > ```C++
> > int element = matrix.at(1).at(2);
> > ```
>
> > 使用迭代器
> >
> > ```C++
> > auto iter = matrix.begin() + 1;
> > int element = iter->second;
> > ```

### Code&Analysis

#### old

矩阵中有大量空间没有被使用

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

#### new😋

执行时间击败100%，消耗内存击败61.16%😋😋😋

## 7.整数反转

### Thought

#### old:

将一个整数拆分成数字，存入vector digit中，按某种顺序取出反转整数

#### new:

记 rev 为翻转后的数字，为完成翻转，我们可以重复「弹出」x 的末尾数字，将其「推入」rev 的末尾，直至 x 为 0

### Doubts&Gains

> 在 C++ 中，`abs()` 是一个数学函数，用于计算一个整数或浮点数的绝对值。在 C++ 中，`abs()` 是一个数学函数，用于计算一个整数或浮点数的绝对值。`abs()` 函数返回的结果为一个非负数，即输入参数的绝对值。如果输入参数为负数，则返回其相反数。
>
> >- 需要注意的是，`abs()` 函数的参数必须是一个数字类型，否则会导致编译错误。如果需要计算复数或其他类型的数值的模或大小，请使用适当的数学库函数。

> 在C++中，`pow()`是一个数学函数，用于计算一个数的指定次幂。
>
> >- 需要注意的是，`pow()` 函数的参数和返回值都是浮点数类型。
> >- 需要注意的是，如果底数或指数非常大，`pow()` 函数可能会产生精度问题。如果需要高精度计算，可以使用其他的数学库函数或者自行实现高精度计算。

>`digits.insert()` 是 C++ 中向容器中插入元素的方法之一，用于向一个容器中插入元素。`digits.insert(digits.begin(), digit)`中`digits.begin()` 作为 `insert()` 函数的第一个参数，表示要在容器开头位置插入元素；`digit` 作为 `insert()` 函数的第二个参数，表示要插入的元素值。
>
>>```C
>>// 在指定位置插入一个元素
>>iterator insert(const_iterator pos, const T& value);
>>
>>// 在指定位置插入 n 个元素，元素值都为 value
>>void insert(const_iterator pos, size_type n, const T& value);
>>
>>// 在指定位置插入区间 [first, last) 中的所有元素
>>template <class InputIt>
>>void insert(const_iterator pos, InputIt first, InputIt last);
>>
>>// 在容器末尾插入一个元素
>>void push_back(const T& value);
>>```

>`static_cast<目标类型>(要转换的值)`是 C++ 中的一种类型转换方式，用于将一种类型的值转换为另一种类型。
>
>> - 需要注意的是，在进行类型转换时，需要确保转换是安全和合法的。如果类型转换不合法，编译器可能会给出警告或错误提示。

### Code&Analysis

#### old

执行时间还行，这段时间最高了--击败了44.66%，而消耗内存一如既往的大。。。

不太明白初做时的脑回路，刚学了vector就得用？

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

#### new💕

需要注意对于反转后的数字的范围的判断：[-2^{31}^,2^{31}-1^]

执行时间击败100%，消耗内存击败22.57%💕💕💕

## 8.字符串转换整数 

### Thought

1. 读入字符串并丢弃无用的前导空格
2. 检查下一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。 如果两者都不存在，则假定结果为正。
3. 读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
4. 将前面步骤读入的这些数字转换为整数（即，"123" -> 123， "0032" -> 32）。如果没有读入数字，则整数为 0 。必要时更改符号（从步骤 2 开始）。
5. 如果整数数超过 32 位有符号整数范围 [−2^31^, 2^31^ − 1]，需要截断这个整数，使其保持在这个范围内。具体来说，小于 −2^31^ 的整数应该被固定为 −2^31^ ，大于 2^31^ − 1 的整数应该被固定为 2^31^ − 1 。
6. 返回整数作为最终结果。

#### :zero:advanced stage

`状态机`

### Doubts&Gains

> `std::isspace()` 是 C++ 标准库中的一个函数，用于判断给定字符是否为空格字符。空格字符包括空格、制表符、换行符等空白符号。
>
> > 需要注意的是，`std::isspace()` 函数对于非 ASCII 字符集中的字符可能会返回错误的结果。如果需要处理中文字符，请使用支持 Unicode 字符集的库或函数。

> 在 C++ 中，`at()` 是 std::vector 和 std::string 类的成员函数之一，用于访问容器中指定位置的元素。
>
> > `at()` 函数会检查索引是否越界，如果索引越界，则会抛出 `std::out_of_range` 异常。与 `[]` 操作符不同，`[]` 操作符不进行越界检查，因此如果尝试访问不存在的元素，可能会导致程序出现未定义的行为。

> `size_t` 和 `int` 是 C++ 中常见的两种数据类型，它们有以下区别：
>
> >1. 数据范围：`size_t` 是无符号整数类型，用于表示对象的大小或索引值等非负整数。它的取值范围通常是和机器的指针大小相同，也就是说在 32 位系统中为 4 字节，在 64 位系统中为 8 字节，具体取值范围取决于编译器和机器架构。而 `int` 是带符号整数类型，通常使用 4 个字节表示，取值范围在 -2147483648 到 2147483647 之间。
> >2. 内存占用：`size_t` 通常需要更多的内存空间来存储，因为它是一个无符号整数类型，不需要存储符号位，所以会有更多的位数用于存储数值，而 `int` 是带符号整数类型，需要存储符号位，因此会有一个比 `size_t` 更小的位数用于存储数值。
> >3. 使用场景：`size_t` 通常用于表示数组或字符串的长度、内存块的大小、容器的大小等等。在 C++ 标准库中，很多容器和算法都使用 `size_t` 类型来表示元素数量或索引值。而 `int` 则更适合表示一般的整数值，比如计数器、标志位等。

### Code&Analysis💕

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

运行时间击败54.32%，消耗内存击败23.53%

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

##### modify😋

首先考虑临界条件，负数和10的倍数都不可能是回文数。鉴于回文数的特性，可以将整数反转后与原数比较，返回bool值

运行时间击败95.2，消耗内存击败72.69%😋😋😋

## 10.正则表达式匹配(Dp)😭

### Thought

其实对于字符的匹配就分为两种：从p串中取出一个字符或者取出一个字符+`*`

对于取出一个字符+`*`的匹配情况分为三种：

- 重复字符0次
- 重复字符1次
- 重复字符2次及以上

对于本题的理解从两个不同角度分析：

- 从左到右扫描
- 从右到左扫描

### :zero:Doubts&Gains

> `从左到右扫描`还未理解

### Code&Analysis

剪枝过多会增加执行时间？

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

##### modify💕

执行时间击败100%，消耗内存击败44.38%💕💕💕

#### advanced stage

为什么要会想到使用dp

## 11.盛最多水的容器

### Thought

#### old：

`暴力枚举`，将每两条垂直线与X轴的构成面积进行比较，取最大值

#### new：

`双指针` 初始化双指针分别指向水槽左右两端，选定两板高度中的短板，向中间收窄一格，循环收窄，直至双指针相遇时跳出，更新面积最大值

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

#### new❤️

执行时间击败53%，消耗内存击败90.58%❤️❤️❤️

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

#### old

`哈希表`，将六个罗马数字存入unordered_map，整数不同的情况与表中对比并输出相应字符，最后组成字符串并返回

#### new

`暴力枚举`

### Doubts&Gains

>unordered_map是C++ STL提供的关联容器之一，用于实现快速查找键值对。它的初始化可以使用以下几种方式：
>
>>1. 默认初始化：可以直接创建一个空的unordered_map，例如：
>>
>>   ```c
>>   std::unordered_map<int, std::string> myMap;
>>   ```
>>
>>2. 初始化列表初始化：可以使用花括号{}来初始化unordered_map，例如：
>>
>>   ```c
>>   std::unordered_map<int, std::string> myMap = {{1, "one"}, {2, "two"}, {3, "three"}};
>>   ```
>>
>>3. 拷贝构造函数初始化：可以使用另一个已有的unordered_map来初始化新的unordered_map，例如：
>>
>>   ```c
>>   std::unordered_map<int, std::string> myMap1 = {{1, "one"}, {2, "two"}, {3, "three"}};
>>   std::unordered_map<int, std::string> myMap2(myMap1);
>>   ```
>>
>>4. 范围初始化：可以使用一个迭代器范围内的元素来初始化unordered_map，例如：
>>
>>   ```c
>>   std::vector<std::pair<int, std::string>> vec = {{1, "one"}, {2, "two"}, {3, "three"}};
>>   std::unordered_map<int, std::string> myMap(vec.begin(), vec.end());
>>   ```
>>
>>5. emplace函数初始化：可以使用emplace函数向unordered_map中添加元素，例如：
>>
>>   ```c
>>   std::unordered_map<int, std::string> myMap;
>>   myMap.emplace(1, "one");
>>   myMap.emplace(2, "two");
>>   myMap.emplace(3, "three");
>>   ```
>
>>`operator[]`函数：如果要查找的键存在于unordered_map中，则返回与该键关联的`值`，否则会自动创建一个具有默认值的元素并将其插入到unordered_map中。
>>
>>- `operator[]`函数还可以用于修改unordered_map中的元素的值。例如：
>>
>>  ```C
>>  std::unordered_map<std::string, int> myMap = {{"apple", 2}, {"banana", 3}, {"orange", 4}};
>>  myMap["banana"] = 5;          // 修改键值为"banana"的元素的值为5
>>  ```
>
>>`at()`函数：如果要查找的键存在于unordered_map中，则返回与该键关联的`值`，否则会抛出一个`std::out_of_range`异常。
>>
>>- `at()`函数还可以用于修改unordered_map中的元素的值。例如：
>>
>>  ```c
>>  std::unordered_map<std::string, int> myMap = {{"apple", 2}, {"banana", 3}, {"orange", 4}};
>>  myMap.at("banana") = 5;          // 修改键值为"banana"的元素的值为5
>>  ```

### Code&Analysis

#### old

执行时间超过25.58%，消耗内存超过21.1%，这是我首次主动使用数据结构改善代码，还行👍👍👍

根本就没有改善代码，反而弄了四不像，运用枚举的思路，却用哈希表的操作去具体实现，空间和时间开销都增大了

```C++
class Solution {
public:
    string intToRoman(int num) {
        map<int, char> hash = {{1,    'I'},
                               {5,    'V'},
                               {10,   'X'},
                               {50,   'L'},
                               {100,  'C'},
                               {500,  'D'},
                               {1000, 'M'}};

        string str = "";
        while (num > 0) {
            if (num >= 1000) {
                char c = hash.at(1000);
                str = str + c;
                num -= 1000;
            } else if (num >= 500) {
                if (num >= 900) {
                    char c1,c2;
                    c1 = hash.at(100);
                    c2 = hash.at(1000);
                    str = str + c1 + c2;
                    num -= 900;
                } else {
                    char c;
                    c = hash.at(500);
                    str = str + c;
                    num -= 500;
                }
            } else if (num >= 100) {
                if (num >= 400) {
                    char c1, c2;
                    c1 = hash.at(100);
                    c2 = hash.at(500);
                    str = str + c1 + c2;
                    num -= 400;
                } else {
                    char c;
                    c = hash.at(100);
                    str = str + c;
                    num -= 100;
                }
            } else if (num >= 50) {
                if (num >= 90) {
                    char c1, c2;
                    c1 = hash.at(10);
                    c2 = hash.at(100);
                    str = str + c1 + c2;
                    num -= 90;
                } else {
                    char c;
                    c = hash.at(50);
                    str = str + c;
                    num -= 50;
                }
            } else if (num >= 10) {
                if (num >= 40) {
                    char c1, c2;
                    c1 = hash.at(10);
                    c2 = hash.at(50);
                    str = str + c1 + c2;
                    num -= 40;
                } else {
                    char c;
                    c = hash.at(10);
                    str = str + c;
                    num -= 10;
                }
            } else if (num >= 5) {
                if (num >= 9) {
                    char c1, c2;
                    c1 = hash.at(1);
                    c2 = hash.at(10);
                    str = str + c1 + c2;
                    num -= 9;
                } else {
                    char c;
                    c = hash.at(5);
                    str = str + c;
                    num -= 5;
                }
            } else if (num >= 1) {
                if (num >= 4) {
                    char c1, c2;
                    c1 = hash.at(1);
                    c2 = hash.at(5);
                    str = str + c1 + c2;
                    num -= 4;
                } else {
                    char c;
                    c = hash.at(1);
                    str = str + c;
                    num -= 1;
                }
            }
        }
        return str;
    }
};
```

#### new

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

#### modify❤️

执行时间击败82.80%，消耗内存击败52.24%❤️❤️❤️

## 13.罗马数字转整数

### Thought

#### old

思路与12题相似，运用函数分段的思想，好像这里用哈希表是多余的

思路错的也跟12题一样，明明用的是枚举法，却偏要强行加上哈希表

#### new

`哈希表`

### Doubts&Gains

> `len = max(len, (int)strs[i + 1].size());`

>`unordered_set` 和 `unordered_map` 都是 C++ STL 中的关联容器，它们的底层实现都是哈希表。
>
>>它们之间的区别在于它们存储的数据类型不同：
>>
>>- `unordered_set` 存储的是一组不重复的元素，不提供访问和修改元素的方式，因此可以用来快速判断一个元素是否在集合中出现过。
>>- `unordered_map` 存储的是一组键值对，每个键唯一对应一个值，提供了访问和修改元素的方式，因此可以用来快速查找某个键对应的值。
>>
>>另外，`unordered_set` 和 `unordered_map` 在使用上还有一些细节的区别：
>>
>>1. `unordered_set` 和 `unordered_map` 的声明方式不同。`unordered_set` 的声明方式如下：
>>
>>   ```
>>   unordered_set<int> mySet;
>>   ```
>>
>>   `unordered_map` 的声明方式如下：
>>
>>   ```
>>   unordered_map<int, string> myMap;
>>   ```
>>
>>   注意，`unordered_map` 存储的是一组键值对，因此需要同时指定键类型和值类型。
>>
>>2. `unordered_set` 和 `unordered_map` 的元素访问方式不同。`unordered_set` 的元素只能被访问，不能被修改。可以使用迭代器来遍历集合中的元素，例如：
>>
>>   ```
>>   for (auto it = mySet.begin(); it != mySet.end(); ++it) {
>>       cout << *it << " ";
>>   }
>>   ```
>>
>>   `unordered_map` 的元素可以被访问和修改。可以使用键来访问值，例如：
>>
>>   ```
>>   myMap[1] = "one";  // 插入一个键值对
>>   cout << myMap[1] << endl;  // 输出值 "one"
>>   ```
>>
>>   上面的代码使用键值对 `[1, "one"]` 插入了一个元素，然后使用键 `1` 来访问该元素的值。
>>
>>3. `unordered_set` 和 `unordered_map` 的查找方式不同。`unordered_set` 可以使用 `count()` 函数来判断一个元素是否在集合中出现过，例如：
>>
>>   ```
>>   if (mySet.count(10)) {
>>       cout << "The element 10 is in the set" << endl;
>>   }
>>   ```
>>
>>   `unordered_map` 可以使用 `find()` 函数来查找一个键对应的值，例如：
>>
>>   ```
>>   auto it = myMap.find(1);
>>   if (it != myMap.end()) {
>>       cout << "The value of key 1 is " << it->second << endl;
>>   }
>>   ```
>>
>>   上面的代码使用键 `1` 来查找 `myMap` 中的一个元素，并输出该元素的值。
>>
>>**综上所述，`unordered_set` 和 `unordered_map` 之间的主要区别在于存储的数据类型和元素的访问方式。`unordered_set` 存储的是一组不重复的元素，不能修改元素**

### Code&Analysis

#### old😋

执行时间击败94.35%，消耗内存击败67.5%😋😋😋

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

#### :zero:new

执行时间击败6.59%，消耗内存击败5.1%

这里因为开辟了哈希表，比之直接枚举内存开销过大，时间开销？

效率下降了，但是代码更为简洁明了

##### modify

没有仔细读题，题目给出了巧妙的解题方法

执行时间击败45.61%，消耗内存击败8.35%

```C++
class Solution {
public:
    int romanToInt(string s) {
        int result=0;
        unordered_map<char,int> map={
            {'I',1},
            {'V',5},
            {'X',10},
            {'L',50},
            {'C',100},
            {'D', 500},
            {'M', 1000}
        };
        for(int i=0;i<s.length();i++)
        {
            if(map[s[i]] < map[s[i+1]])
                result -= map[s[i]];
            else
            {
                result += map[s[i]];
            }
        }
        return result;
    }
};
```

## 14.😥最长公共前缀

### Thought

#### :zero:old

大体是根据抓住位置来解题，具体实现不了解，感觉是同一级复杂度的不同分支罢了，没什么提升不想看

#### new

对于字符串数组来说最长公共前缀必然由数组中最短的字符串决定，所以找到最短字符串的位置和长度，由最短字符串的第一个字符开始与其他字符串一一对比，直到找出不同返回结果

### Code&Analysis

#### old

执行时间击败75.31%，消耗内存击败28.87%

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string> &strs) {
        int n = strs.size();
        if (n == 0) {
            return "";
        }
        string prefix = strs[0];
        for (int i = 1; i < n; i++) {
            int j = 0;
            while (j < prefix.size() && j < strs[i].size() && prefix[j] == strs[i][j]) {
                j++;
            }
            prefix = prefix.substr(0, j);
            if (prefix.empty()) {
                return "";
            }
        }
        return prefix;
    }
};
```

#### new❤️

执行时间击败29.28%，消耗内存击败81.55%❤️❤️❤️

## 15.😥三数之和

### Thought

#### old：

`哈希表` 现将数据存入map中，在遍历数组的时候，只需要向map去查询是否有和目前遍历元素twoSum匹配的数值，如果有，就找到匹配对存入temp，进行去重；如果没有就继续循环，这里的去重是根据find函数判断res中是否存在temp，没有就将temp传入res，有继续循环

#### new：

`双指针` 双指针的难点在于该怎么移动左右指针，一般来说应先将要处理的数据排序，再根据所求结果进行判断，小于0就left右移，大于0就right左移，本题的难点在于去重的处理，由于已对数据排序，重复数据必然相邻，对相邻数据进行判断，重复跳过即可

### Doubts&Gains

>`sort`：**sort(first_pointer,first_pointer+n,cmp)**
>
>> `sort` 函数会以 `[first_pointer, first_pointer+n)` 范围内的元素作为输入，并使用 `cmp` 函数来比较元素的顺序
>>
>> 比较函数`cmp`的参数是两个通过迭代器指定的元素,这两个元素是通过迭代器 `first_pointer` 和 `first_pointer+n` 来确定的

>`lambda表达式`
>
>```C++
>std::sort(matrix.begin(), matrix.end(), [col](const std::vector<int>& row1, const std::vector<int>& row2) {
>            return row1[col] < row2[col];
>        });
>```
>
>> `[col]`：这是lambda表达式的捕获列表，用于指定在lambda函数体中要使用的变量。在这
>>
>> 种情况下，`col`是作为捕获的变量，表示当前要排序的列索引。
>>
>> `(const std::vector<int>& row1, const std::vector<int>& row2)`：这是lambda函数的参数列表，指定了lambda函数接受的参数类型和参数名称。在这里，`row1`和`row2`是两个二维`vector`的引用，表示要比较的两行。
>>
>> `return row1[col] < row2[col];`：这是lambda函数的返回语句，指定了排序的规则。它比较两行中指定列索引`col`位置的元素，并返回比较结果。如果`row1[col]`小于`row2[col]`，则返回`true`，表示`row1`应该在`row2`之前进行排序。
>
>使用`lambda`表达式只是提供了一种更简洁和内联的方式来定义比较函数，特别是在比较函数相对简单且只在特定上下文中使用的情况下。
>
>> 使用`lambda`表达式的好处包括：
>>
>> 1. 简洁性：使用`lambda`表达式可以避免在函数外部定义独立的比较函数，使得代码更加紧凑和可读性更高。比较函数的定义与其使用紧密相关，不需要额外的函数定义和命名。
>> 2. 内联性：`lambda`表达式通常被内联到调用的地方，而不会产生额外的函数调用开销。这有助于提高代码的性能，特别是在排序等频繁调用的场景下。
>> 3. 便捷性：`lambda`表达式可以捕获外部的变量，并在比较函数中直接使用。这使得可以在比较函数中使用来自外部作用域的参数、局部变量或对象，提供了更大的灵活性和方便性。
>
>总之，使用`lambda`表达式作为比较函数可以使代码更简洁、可读性更高，并提供了更好的灵活性和内联性。然而，对于更复杂的比较函数或需要在多个地方重用的情况，定义独立的比较函数可能更为合适。使用哪种方式取决于具体的情况和个人偏好。

>二维`vector`数组的排序操作
>
>```C++
>for (size_t col = 0; col < matrix[0].size(); ++col) {
>        std::sort(matrix.begin(), matrix.end(), [col](const std::vector<int>& row1, const std::vector<int>& row2) {
>            return row1[col] < row2[col];
>        });
>    }
>```

>`find`：**find(first, last, value)**
>
>```C++
> if (find(res.begin(), res.end(), temp) == res.end()) {
>                        res.push_back(temp);
>                    }
>```
>
>>这里是一些重要参数的说明：
>>
>>- `first`和`last`是表示范围的迭代器。`first`指向要搜索的范围的第一个元素，`last`指向要搜索的范围的最后一个元素的下一个位置。范围包括[first, last)。
>>- `value`是要查找的值。
>>
>>`std::find`函数返回一个迭代器，指向找到的第一个匹配元素的位置。如果没有找到匹配的元素，则返回`last`迭代器。

### Code&Analysis

#### old

算法超时了

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int> &nums) {
        unordered_map<int, int> map;
        vector<vector<int>> res;

        for (int i = 0; i < nums.size(); i++) {
            map.insert(pair<int, int>(0 - nums[i], i));
        }

        int twoSum = 0;
        for (int i = 0; i < nums.size(); i++) {
            for (int j = i + 1; j < nums.size(); j++) {
                twoSum = nums[i] + nums[j];
                auto iter = map.find(twoSum);
                if (iter != map.end() && iter->second != i && iter->second != j) {
                    vector<int> triplet = {nums[i], nums[j], nums[iter->second]};
                    sort(triplet.begin(), triplet.end());
                    if (find(res.begin(), res.end(), triplet) == res.end()) {
                        res.push_back(triplet);
                    }
                }
            }
        
        return res;
    }
};
```

#### new😋

执行时间击败93.40%，消耗内存击败66.2%😋😋😋

#### :zero:modify

采用对二维vector排序，且lambda表达式解决

## 16.最接近的三数之和❤️

### Thought

将数组排序，使用`双指针`指向边界上的两个元素，根据三数之和与target的值的关系，选择「抛弃」左边界的元素还是右边界的元素，更新差值的值，返回最小差值的三数

### Doubts&Gains

> `INT_MAX` 使用 INT_MAX 需要包含 <climits> 头文件。该头文件定义了整数类型的最大值和最小值，以及其他一些常量和宏定义。其中 INT_MAX 是一个宏定义，表示 int 类型的最大值。

>`interrupted by signal 15: SIGTERM`是一种在类Unix系统中常见的信号。它代表了一个终止请求，通常用于请求进程正常终止。
>
>>当进程接收到 SIGTERM 信号时，它意味着某个外部实体（通常是操作系统或其他进程）发出了一个请求，要求该进程终止执行并进行清理工作。接收到 SIGTERM 信号后，进程可以选择如何处理该信号，通常的处理方式是优雅地关闭进程并进行资源释放。
>>
>>SIGTERM 是一种默认的终止信号，可以通过命令行工具（如kill命令）或编程语言中的相关函数（如C语言中的kill函数）来发送给进程。在大多数情况下，这是一种正常的终止请求，允许进程有机会进行清理和释放资源，然后正常退出。
>>
>>需要注意的是，SIGTERM 是一种软件终止信号，与SIGKILL（信号 9）不同。SIGKILL 是一种强制终止信号，会立即终止进程的执行，不给予进程进行清理的机会。SIGTERM 通常被用于优雅地终止进程，而SIGKILL 则被用于强制终止无响应或无法正常终止的进程。

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

## 17.😩电话号码的字母组合(回溯)

### Thought

本题最直接的想法应该是一个字母对应一个for循环，但如果字母过多，时间复杂度就会大到让人难以接受，所以这里采取回溯将for循环的层数写出来，这里最好画出排列组合的树状图，方便进行`回溯`三部曲

![image-20230611171814121](/home/polarnight/.config/Typora/typora-user-images/image-20230611171814121.png)

### Doubts&Gains

>`回溯三部曲`
>
>- 回溯函数模板返回值以及参数
>
>  ```C++
>  vector<string> result;
>  string s;
>  void backtracking(const string& digits, int index)
>  ```
>
>- 回溯函数终止条件
>
>  ```C++
>  if (终止条件) {
>      存放结果;
>      return;
>  }
>  ```
>
>- 回溯搜索的遍历过程
>
>  ```c++
>  for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
>      处理节点;
>      backtracking(路径，选择列表); // 递归
>      回溯，撤销处理结果
>  }
>  ```

>`vector<pair<int, vector<char>>>`，它是一个向量，其中每个元素都是一个 `pair`，其中第一个元素是 `int` 类型，第二个元素是 `vector<char>` 类型。
>
>>而 `vector<vector<char>>` 是一个二维向量，其中每个元素都是一个向量，每个子向量都包含一组 `char` 类型的元素。
>>
>>因此，两者之间的主要区别在于它们的元素类型和向量的结构不同。`vector<pair<int, vector<char>>>` 包含一组具有整数和字符向量的键值对，而 `vector<vector<char>>` 包含一组具有字符向量的向量。
>>

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

#### new

执行时间击败34.1%，消耗内存击败70.35%

##### modify💕

将二维数组换成了哈希表unordered_map

执行时间击败100%，消耗内存击败29.23%💕💕💕

## 18.四数之和

### :zero:Thought

这道题与三数之和有异曲同工之妙，只需将其中两数之和看做第三数，即可套用三数之和的思路

根据去重方法的不同，运行效率也不同

- 创建一个容器，先将运行结果放入其中，再把容器中的内容与返回结果比较

- 将返回结果现在的项与上一项比较

  注：如果是将现在的项与下一项比较，会漏掉`don't know why`

### Doubts&Gains

> `res.push_back({nums[i], nums[j], nums[left], nums[right]});`

### Code&Analysis

#### old

执行时间击败5.2%，消耗内存击败6.97%

```C++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int> &nums, int target) {
        int len = nums.size();
        sort(nums.begin(), nums.end());
        vector<vector<int>> res;
        int cnt = 0;
        unordered_set<string> visited;

        for (int i = 0; i < len; i++) {
            for (int j = i + 1; j < len; j++) {
                int left = j + 1;
                int right = len - 1;
                while (left < right) {
                    long int twoSum = nums[i] + nums[j];
                    long int fourSum = twoSum + nums[left] + nums[right];
                    if (fourSum < target) {
                        left++;
                    } else if (fourSum > target) {
                        right--;
                    } else {
                        string key = to_string(nums[i]) + "," + to_string(nums[j]) + "," + to_string(nums[left]) + "," +
                                     to_string(nums[right]);
                        if (visited.find(key) == visited.end()) {
                            res.push_back({nums[i], nums[j], nums[left], nums[right]});
                            visited.insert(key);
                        }
                        left++;
                        right--;
                    }
                }
            }
        }

        return res;
    }
};
```

#### new😋

执行时间击败91.46%，消耗内存击败61.95%😋😋😋

## 19.删除链表的倒数第 N 个结点

### Thought

#### old

这道题比较简单，先循环一次拿到链表的总长度，然后循环到要删除的结点的前一个结点开始删除操作.需要注意的特例是头结点的处理

#### new

`双指针`

### Doubts&Gains

对于链表的处理还是显得有些生涩，需要反复练习

>`ListNode *cur = head;
>  int i = len - n;
>  while (i > 1) {
>      cur = cur->next;
>      i--;
>  }
>  cur->next = cur->next->next;`
>
>对于cur的创建还是只是知其然，不知其所以然

### Code&Analysis

#### old

执行时间击败27.49%，消耗内存击败43.55%

```C++
class Solution {
public:
    ListNode *removeNthFromEnd(ListNode *head, int n) {
        ListNode *cur = head;
        int len = 0;
        while (cur !=
               nullptr) {
            len++;
            cur = cur->next;
        }
        if (n > len) {
            return head;
        } else if (n == len) {
            ListNode *curr = head;
            head = head->next;
            curr->next = nullptr;
            return head;
        } else {
            ListNode *curr = head;
            int i = len - n;
            while (i > 1) {
                curr = curr->next;
                i--
            }
            curr->next = curr->next->next;
        }
        return head;
    }
}
```

#### new😋

执行时间击败77.40%，消耗内存击败96.45%😋😋😋

## 20.😭有效的括号(stack)

### Thought

由于`栈`结构的特殊性，非常适合做对称匹配类的题目。

### :zero:Doubts&Gains

>`stack`

### Code&Analysis

执行时间击败37%，消耗内存击败31.68%

```c++
class Solution {
public:
    bool isValid(string s) {
      unordered_map<char, char> map = {{'{', '}'},
                                       {'(', ')'},
                                       {'[', ']'}};
      stack<char> sta;

      for (auto & c : s) {
          if (map.count(c)) {
              sta.push(c);
          } else {
              if (sta.empty() || map[sta.top()] != c) {
                  return false;
              }
              sta.pop();
          }
      }
      return sta.empty();
    }
};
```

#### modify💞

执行时间击败100%，消耗内存击败84.45%💞💞💞

减少了套娃

## 21.合并两个有序链表

### Thought

#### old

将两个链表中的元素取出并放入事先准备的容器中，排序后插入新的链表中，返回新链表

#### new

`递归`或`迭代`

### Doubts&Gains

> 链表：`元素插入`
>
> > ```C++
> > ListNode *res = nullptr;
> > ListNode *temp = nullptr;
> > 
> > for (auto &val : vec) {
> >     if (res == nullptr) {
> >         res = new ListNode(0);
> >         temp = res;
> >     } else {
> >         temp->next = new ListNode(val);
> >         temp = temp->next;
> >     }
> > }
> > ```

### Code&Analysis

#### old

执行时间击败20.2%，消耗内存击败5.5%

```C++
class Solution {
public:
    ListNode *mergeTwoLists(ListNode *list1, ListNode *list2) {
        vector<int> vec;
        ListNode *a = list1;
        ListNode *b = list2;

        while (a != nullptr) {
            vec.push_back(a->val);
            a = a->next;
        }

        while (b != nullptr) {
            vec.push_back(b->val);
            b = b->next;
        }

        sort(vec.begin(), vec.end());

        ListNode *res = nullptr;
        ListNode* c = nullptr;

        for (int n : vec) {
            if (res == nullptr) {
                res = new ListNode(n);
                c = res;
            } else {
                c->next = new ListNode(n);
                c = c->next;
            }
        }

        return res;
    }
};
```

#### new💕

执行时间击败100%，消耗内存击败45.46%💕💕💕

## 22.😩括号生成

### Thought

#### old

这道题对于`回溯`的使用并不局限在回溯三部曲的固定格式，相当自由

#### :zero:new

`栈`

### Code&Analysis

#### old

执行时间击败100%，消耗内存击败58.67%

## 23.合并K个升序链表❤️

### Thought

#### old

21题的变种，将链表数组中每个链表的元素取出并放入容器，排序，插入升序链表

#### new

建立`优先队列`（min_heap），全部元素入队，然后不断弹出，构建链表

##### modify

依然建立`优先队列`（min_heap），不过只有链表头元素入队，弹出该元素后，链表后移

#### advanced stage

21题的变种，使用递归或迭，`两两合并`

##### modify

`分治` 找到中间点，一分为二，直到不能再拆分（规模为1时）时便返回。之后开始合并，合并的代码借用`两两合并`的代码。当两个规模最小的链表合并完后，其规模就变大了，然后不断重复这个合并过程，直到最终得到一个有序的链表

### :zero:Doubts&Gains

> `优先队列`

> 对链表和它本身进行合并，会导致无限递归，`栈溢出`
>
> ```cpp
> for (int i = 0; i < lists.size(); i++) {
>            if (lists[i]) head = merge(head, lists[i]);
>        }
> ```

>`分治`就是不断缩小其规模，在不断合并扩大的过程

### Code&Analysis❤️

#### old

执行时间击败81.36%，消耗内存击败18.72%

第一次靠自己一次过困难题，虽然解法暴力了点😂😂😂✌️✌️✌️

```cpp
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        vector<int> vec;
        int len = lists.size();

        for (int i = 0; i < len; i++) {
        ListNode *a = lists[i];

        while (a != nullptr) {
                vec.push_back(a->val);
                a = a->next;
            }
        }

        sort(vec.begin(), vec.end());

        ListNode *res = nullptr;
        ListNode* c = nullptr;

        for (int n : vec) {
            if (res == nullptr) {
                res = new ListNode(n);
                c = res;
            } else {
                c->next = new ListNode(n);
                c = c->next;
            }
        }

        return res;
    }
};
```

#### new

效率其实差不多，主要是学习新的数据结构-优先队列

```cpp
class Solution {
public:
    struct cmp {
        bool operator()(ListNode *a, ListNode *b) {
            return a->val > b->val;
        }
    };

    ListNode *mergeKLists(vector<ListNode *> &lists) {
        priority_queue <ListNode *, vector<ListNode *>, cmp> min_heap;

        for (int i = 0; i < lists.size(); ++i) {
            while (lists[i] != nullptr) {
                min_heap.push(lists[i]);
                lists[i] = lists[i]->next;
            }
        }

        ListNode dummy(-1);
        ListNode *p = &dummy;
        while (!min_heap.empty()) {
            p->next = min_heap.top();
            min_heap.pop();
            p = p->next;
        }
        p->next = nullptr;
        return dummy.next;
    }
};
```

##### modify

执行时间击败81.75%，消耗内存击败44.59%

#### advanced stage

执行时间击败11.44%，消耗内存击败44.59%

```C++
class Solution {
public:
    ListNode *merge(ListNode *a, ListNode *b) {
        if (!a) return b;
        if (!b) return a;

        if (a->val <= b->val) {
            a->next = merge(a->next, b);
            return a;
        } else {
            b->next = merge(a, b->next);
            return b;
        }
    }
    
    ListNode *mergeKLists(vector<ListNode *> &lists) {
       if (lists.size() == 0) return nullptr;
    
       ListNode *head = lists[0];
       for (int i = 1; i < lists.size(); i++) {
           if (lists[i]) head = merge(head, lists[i]);
       }
    
       return head;
    }

};
```

##### modify

执行时间击败81.75%，消耗内存击败37.54%

```C++
class Solution {
public:
    // 合并两个有序链表
    ListNode* merge(ListNode* p1, ListNode* p2){
        if(!p1) return p2;
        if(!p2) return p1;
        if(p1->val <= p2->val){
            p1->next = merge(p1->next, p2);
            return p1;
        }else{
            p2->next = merge(p1, p2->next);
            return p2;
        }
    }

    ListNode* merge(vector<ListNode*>& lists, int start, int end){
        if(start == end) return lists[start];
        int mid = (start + end) / 2;
        ListNode* l1 = merge(lists, start, mid);
        ListNode* l2 = merge(lists, mid+1, end);
        return merge(l1, l2);
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if(lists.size() == 0) return nullptr;
        return merge(lists, 0, lists.size()-1);
    }
};
```

## 24.两两交换链表中的节点💕

### Thought

#### old

`递归`（链表的基本操作）

#### new

`栈`（stack） 每次取出两个节点放入栈，再从栈取出两个节点，根据栈先进后出的特性实现两两交换节点，接着串联两个节点，重复至遍历完链表

### Doubts&Gains

>在交换链表中的两个节点时，我们需要考虑两个方面：
>
>1. 更新前一个节点的 `next` 指针，将其指向交换后的新节点。
>2. 更新当前节点的 `next` 指针，将其指向下一个待交换的节点。
>
>>这里的 `prev` 指针是用来追踪前一个节点的。当 `prev` 不为空时，表示当前节点不是链表的头节点，需要更新前一个节点的 `next` 指针。如果 `prev` 为空，说明当前节点是头节点，不需要更新前一个节点的 `next` 指针。
>>
>>因此，在交换节点时，我们需要判断 `prev` 是否为空。如果 `prev` 不为空，则将其 `next` 指针指向交换后的新节点 `nextNode`；如果 `prev` 为空，说明当前节点是头节点，将新节点 `nextNode` 设置为新的头节点。
>>
>>这样做是为了确保交换后的链表能够正确连接，保持链表的完整性。
>>

### Code&Analysis💕

#### old

执行时间击败100%，消耗内存击败49.5%

```C++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
       if (head == nullptr || head->next == nullptr) {
           return head;
       }

       ListNode *newHead = head->next;
       ListNode *curr = head;
       ListNode *prev = nullptr;

       int i = 1;
       while (curr != nullptr) {
           if (i % 2 == 0) {
               ListNode *temp = curr->next;
               curr->next = temp->next;
               temp->next = curr;

               if (prev != nullptr) {
                   prev->next = temp;
               }

               prev = curr;
               curr = curr->next;
               ++i;
           } else {
               ++i;
           }
       }
       return newHead;
    }
};
```

#### new

执行时间击败100%，消耗内存击败24.64%

---

## 25.K个一组翻转链表

### Thought

`栈` 套用24题的思路，将k个一组的节点放入栈中，根据栈先进后出的特性推出节点，将k个节点一一串联，重复至遍历完链表的k的倍数的节点数，最后串联剩下未翻转的链表

### Double&Gains

> `realLen -= k;` 相当于 `realLen = realLen - k;`

### Code&Analysis

执行时间击败53.97%，消耗内存击败10.80%

第二次靠自己一次过困难题了✌️✌️✌️

## 26.删除有序数组中的重复项

### Thought

逆序遍历，判断去重，返回处理后的数组长度

### Double&Gains

> 这里`顺序遍历`和`逆序遍历`的选择会产生很有趣的现象

### Code&Analysis

执行时间击败23.39%，消耗内存击败55.73%

## 27.移除元素

### Thought

`std::remove()`和`vector.erase()`结合

### Double&Gains

>`std::remove()`

>`vector.erase()`

### Code&Analysis

执行时间击败100%，消耗内存击败55.9%

## 28.找出字符串中第一个匹配项的下标

### Thought

怎么说呢，这道题就是纯思路，想清楚就行了，先遍历`haystack`找到与`needle`第一个字符匹配`n`的位置，如果找不到就直接返回-1。找到了就开始一一匹配，如果不匹配就得回到`haysatck`的`n+1`的位置重新跟`needle`第一个字符匹配。匹配就返回`n`

### Code&Analysis❤️

执行时间击败33.29%，消耗内存击败95.36%❤️❤️❤️

## 29.😭两数相除

### Thought

#### old

用减法循环替代除法

#### new

递归

### Double&Gains

> `abs()`

> `long`

### Code&Analysis

超时

```C++
class Solution {
public:
    int divide(int dividend, int divisor) {
        if (std::abs(static_cast<long long >(dividend)) < std::abs(static_cast<long long>(divisor)) || divisor == 0) return 0;

        long cnt = 0;
        int left = dividend, right = divisor;
        if (dividend >= 0 && divisor > 0) {
            while (left >= right) {
                left -= right;
                cnt++;
            }
        }
        if (dividend < 0 && divisor < 0) {
            while (left <= right) {
                left -= right;
                cnt++;
            }
        }
        if (dividend < 0 && divisor > 0) {
            while (left <= -right) {
                left += right;
                cnt++;
            }
            cnt = -cnt;
        }
        if (dividend >= 0 && divisor < 0) {
            while (left >= -right) {
                left += right;
                cnt++;
            }
            cnt = -cnt;
        }

        if (cnt > INT32_MAX) return INT32_MAX;
        if (cnt < INT32_MIN) return INT32_MIN;
        int res = cnt;
        return res;
    }
};
```

## 30.😭串联所有单词的子串

### Thought

#### old

回溯

#### new

滑动窗口

### Double&Gains

>

### Code&Analysis

#### new

执行时间击败86.1%，消耗内存击败66.43%

## 31.下一个排列

### Thought

思维题，枚举出来找规律

### Code&Analysis

执行时间击败25.75%，消耗内存击败53.70%

## 32.😥最长有效括号

### Thought

`栈（stack）`

### Code&Analysis

执行时间击败77.42%，消耗内存击败63.81%

## 33.搜索旋转排列数组

### Thought

题目要求时间复杂度为0（log n），也就是采用`二分法`

另类的二分法，设置左右指针`left `和`right`指向数据左右边界，取序列中值为`mid`

- 如果`[mid]`小于`[right]`，`mid`处于右边区域，对`target`是否处在`[[mid], [right]]`区域内进行判断，根据结果改变左右边界，进行递归调用
- 如果`[mid]`大于`[left]`，`mid`处于左边区域，对`target`是否处在`[[left], [mid]]`区域内进行判断，根据结果改变左右边界，进行递归调用

### Double&Gains

> `std::find()` 在指定范围内查找第一个等于给定值的元素，并返回一个指向该元素的迭代器；如果没找到，则返回一个指向该范围末尾的迭代器。`std::find` 函数是一个通用的算法函数，适用于各种容器类型和迭代器。使用时需要包含 `<algorithm>` 头文件。
>
> >`时间复杂度`取决于容器的类型和大小。对于线性序列容器（如 `std::vector`），它需要遍历整个范围来查找目标值。因此，最坏情况下的时间复杂度为 O(n)，其中 n 是范围的大小。对于关联容器（如 `std::set` 或 `std::map`），`std::find` 的时间复杂度为 O(log n)，其中 n 是容器中的元素个数，因为这些容器使用了二叉搜索树或红黑树等数据结构来实现快速查找。

> `std::sort` 默认使用 `<` 运算符来进行元素的比较。但对于自定义的类型，或者需要按照特定的比较规则进行排序时，可以通过提供自定义的比较函数（也称为比较谓词或函数对象）来定义排序的方式。：`std::sort` 是一个通用的算法函数，适用于各种容器类型和迭代器。使用时需要包含 `<algorithm>` 头文件。
>
> >`cmp`应满足以下条件：
> >
> >1. 返回一个布尔值，表示元素的相对顺序。
> >2. 如果第一个参数应排在第二个参数之前，则返回 true；否则返回 false。
> >
> >`cmp`可以有多种方式来定义，包括函数指针、函数对象、`lambda`表达式等。例如，可以按照元素的降序排列来定义一个比较函数：
> >
> >```C++
> >bool cmp(int a, int b) {
> >    return a > b;
> >}
> >```
>
> > `时间复杂度`取决于排序算法的实现。在 C++ 标准库中，通常使用的是`快速排序（Quicksort）`或`归并排序（Mergesort）`算法。快速排序的平均时间复杂度为 O(n log n)，最坏情况下为 O(n^2^)。归并排序的时间复杂度稳定在 O(n log n)。因此，可以将 `std::sort` 的时间复杂度视为 O(n log n)，其中 n 是容器中的元素个数。

### Code&Analysis💕

执行时间击败100%，消耗内存击败50.25%💕💕💕

## 34.在排序数组中查找元素的第一个和最后一个位置

### Thought

题目要求时间复杂度为0（log n），也就是采用`二分法`

两次`二分`，第一次二分查找第一个一个>= target 的位置，第二次二分查找最后一个<=     target 的位置，难点在于细节和边界处理上

### Code&Analysis

执行时间击败21.79%，消耗内存击败26.5%

## 35.搜索插入位置

### Thought

题目要求时间复杂度为0（log n），也就是采用`二分`法

常规的`二分`使用，需要额外处理超过数组范围的值

### Code&Analysis😋

执行时间击败79.79%，消耗内存击败78.00%😋😋😋

## 36.有效的数独

### Thought

没有什么技巧，直接暴力遍历就是了，外加三次判断：行，列，3*3区域内是否有重复

### Code&Analysis

执行时间击败13.70%，消耗内存击败18.33%

## 37.😭解数独

### Thought



### Code&Analysis

## 38.外观数列

### Thought

`递归` 将字符串分割成多个不同的组，组由连续的最多相同字符组成

### Code&Analysis💕

执行时间击败100%，消耗内存击败27.57%💕💕💕

## 39.组合总和

### Thought

`回溯`

### Double&Gains

> 如何快速判断一道题是否应该使用`DFS`+`回溯`算法来爆搜。
>
> 总的来说，你可以从两个方面来考虑：
>
> 1. 求的是所有的方案，而不是方案数。 由于求的是所有方案，不可能有什么特别的优化，我们只能进行枚举。这时候可能的解法有动态规划、记忆化搜索、DFS + 回溯算法。
>
> 2. 通常数据范围不会太大，只有几十。 如果是动态规划或是记忆化搜索的题的话，由于它们的特点在于低重复/不重复枚举，所以一般数据范围可以出到 10^4^-10^5^，而`DFS`+`回溯`的话，通常会限制在`30`以内。
>
> 这道题数据范围是`30`以内，而且是求所有方案，因此我们使用`DFS`+`回溯`来求解。
>
> 作者：宫水三叶
> 链接：https://leetcode.cn/problems/combination-sum/solutions/585352/dfs-hui-su-suan-fa-yi-ji-ru-he-que-ding-wpbo5/
> 来源：力扣（LeetCode）
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### Code&Analysis😋

执行时间击败89.13%，消耗内存击败87.97%😋😋😋

## 40.组合总和 II

### Thought

`回溯` 跟39题的区别：主要是每个组合只能使用一次`candidates`里的数字；`candidates`里包含重复数字。由此引来的重点就是去重：`candidates`排序，添加一个初始全为0（0代表未使用）的标志数组`used`，当`candidates[i] == candidates[i - 1]`且`used[i - 1] == 0`时，同一树层有重复元素，跳过

### Double&Gains

> `vector去重`
>
> ```c++
>     std::sort(numbers.begin(), numbers.end());
>     numbers.erase(std::unique(numbers.begin(), numbers.end()), numbers.end());
> ```

>申请一个与 `std::vector<int> candidates` 长度相同且所有元素都为 0 的 `std::vector<int>` 数组，可以使用 `std::vector` 的构造函数或 `resize()` 函数来实现。
>
>>- 方法1：使用构造函数进行初始化
>>
>>  ```c++
>>      std::vector<int> zeros(candidates.size(), 0);
>>  ```
>>
>>  在上述代码中，我们使用了 `std::vector` 的构造函数，将长度设置为 `candidates.size()`，并将所有元素初始化为 0。
>>
>>- 方法2：使用 resize() 函数进行初始化
>>
>>  ```c++
>>      std::vector<int> zeros;
>>      zeros.resize(candidates.size(), 0);
>>  ```
>>
>>  在上述代码中，我们首先创建了一个空的 `std::vector<int>`，然后使用 `resize()` 函数将其大小设置为 `candidates.size()`，并将所有元素初始化为 0。

> ```C++
>     std::unordered_set<std::vector<int>> ans; 
>     //引发Call to implicitly-deleted default constructor of 'std::unordered_set<std::vector<int>>'错误
> ```
>
> 这个错误是因为 `std::unordered_set` 不支持直接使用 `std::vector<int>` 作为键类型，因为 `std::vector` 默认情况下没有哈希函数定义。
>
> 要解决这个问题，您需要提供一个自定义的哈希函数和相等比较函数来处理 `std::vector<int>`。
>
> ```c++
>     std::unordered_set<std::vector<int>, VectorHash, VectorEqual> ans;
> ```

### Code&Analysis❤️

执行时间击败26.28%，消耗内存击败80.73%❤️❤️❤️

## 41.缺失的第一个正数

### Thought

`桶排序` `nums`长度为`n`，那么答案必然在`[1, n + 1]`范围内。进行预处理：保证`1`出现在`nums[0]`的位置上，`2`出现在 `nums[1]`的位置上，…，`n`出现在`nums[n - 1]`的位置上。不在`[1, n]`范围内的数不用动。遍历`nums`，找到第一个不在应在位置上的`[1, n]`的数。如果没有找到，说明数据连续，答案为`n + 1`

### Double&Gains

> `桶排序`

> `原地哈希`

### Code&Analysis

执行时间击败45.78%，消耗内存击败12.84%

## 42.😭接雨水

### Thought



### Code&Analysis

执行时间击败54.90%，消耗内存击败18.12%

## 43.字符串相乘

### Thought

`模拟` 本质是模拟手算乘法的过程，重点在于一个数学定理：两个长度分别为`n`和`m`的数相乘，长度不会超过`n + m`。

### Code&Analysis

执行时间击败63.75%，消耗内存击败46.79%

## 44.通配符匹配

### Thought

`动态规划` 

- 状态定义：`dp(i, j)`表示字符串`s`中第i个字符跟字符串`p`中第`j`个字符匹配，最终结果是返回`dp[len1][len]`

- 状态转移：`p[j - 1]`有三种情况，重点在于`*`的情况。可以匹配0个、1个、2个...字符

  1. 0个字符：`dp(i,j) = dp(i, j - 1)`

  2. 1个字符：`dp(i,j) = dp(i - 1, j - 1)`

  3. 2个字符：`dp(i,j) = dp(i - 2, j - 1)`

     ...

​       n. n个字符：`dp(i,j) = dp(i - n, j - 1)`

因此对于`p[j - 1] = '*'`的情况，想要`dp(i, j) = true`，只需要其中一种情况为`true`即可。也就是状态之间是「或」的关系：

​    `dp[i][j]=dp[i][j - 1]∣∣dp[i−1][j−1]∣∣...∣∣dp[i−k][j−1] (i>=k)` 式1
这意味着我们要对 k 种情况进行枚举检查吗？

其实并不用，对于这类问题，我们通常可以通过「代数」进简化，将`i - 1`代入上述的式子：

​    `dp[i−1][j]=dp[i−1][j−1]∣∣dp[i−2][j−1]∣∣...∣∣dp[i−k][j−1] (i>=k)` 式2
可以发现，式2：`dp[i - 1][j]`与式1：`dp[i][j]`中的从`dp[i][j - 1]`开始的后半部分是一样的，因此有：

​     `dp[i][j]=dp[i][j−1]∣∣dp[i−1][j] (i>=1)`

作者：宫水三叶
链接：https://leetcode.cn/problems/wildcard-matching/solutions/783463/gong-shui-san-xie-xiang-jie-dong-tai-gui-ifyx/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### Double&Gains

> `回退检查`

> `哨兵技巧`

### Code&Analysis

执行时间击败55.15%，消耗内存击败59.9%


# leedcode

## 1.两数之和

## 2.两数相加

### thought

#### old：

分别遍历链表中l1，l2中的值，各自取出后还原成一个计算数，然后两数相加，令结果逆序插入新的链表

#### new：

同时遍历两个链表，逐位计算它们的和，并与当前位置的进位值相加，顺序插入新的链表，将链表翻转逆序输出

### knowledge

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
```



### error

<old>由于使用int，导致当测试用例过大时，结果崩溃

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

<new>这里将链表翻转的操作时间和空间开销都很大

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


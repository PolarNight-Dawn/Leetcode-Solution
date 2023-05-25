# leedcode废稿

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


# Lists (homework 1)

Definition for singly-linked list:

```C++ 
struct ListNode {
     int val;
     ListNode *next;
     ListNode(int x) : val(x), next(NULL) {}  
 };
 ```
## Reorder List
### Cozy slow way
```C++
class Solution {
public:
    void reorderList(ListNode* head) {
        ListNode* junBuf = head, *senBuf, *newTailBuf;
        if (head) {
            while (junBuf->next) {
                senBuf = junBuf;
                while (senBuf->next) {
                    newTailBuf = senBuf;
                    senBuf = senBuf->next;
                }
                if (junBuf->next == senBuf || junBuf == senBuf)
                    break;
                senBuf->next = junBuf->next;
                junBuf->next = senBuf;
                newTailBuf->next = NULL;
                junBuf = senBuf->next;
            }
        }
    }
};
```
### Little bit faster
```C++
class Solution {
public:
    void reorderList(ListNode* head) {
        ListNode* middle, *buf = head;
        if (head) {
            middle = GetMiddle(head);
            if (middle == head)
                return;
            while (buf->next != middle) {
                buf = buf->next;
            }
            buf->next = NULL;
            middle = reverseList(middle);
            while (middle && head) {
                buf = middle->next;
                middle->next = head->next;
                head->next = middle;
                head = middle->next ? middle->next : middle;
                middle = buf;
            }
        }
    }
    
private:
    ListNode* GetMiddle(ListNode* head) {
        ListNode* p = head, *t = head;
        while (p && p->next) {
            p = p->next->next;
            t = t->next;
        }
        return t;
    }
    
    ListNode* reverseList(ListNode* head) {
        ListNode* buf = head;
        if (buf) {
            while (buf->next)
                buf = buf->next;
            RecursiveThing(head);
        }
        return buf;
    }
    
    void RecursiveThing(ListNode* node) {
        if (!node->next)
            return;
        else 
            RecursiveThing(node->next);
        node->next->next = node;
        node->next = NULL;
    }
};
```

## Linked List Cycle
### Dumb way
```C++
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* p = head, *t = head;
        int i, j;
        if (p) {
            for (i = 0; p->next; p = p->next, i++) {
                for (j = 0, t = head; p != t; t = t->next, j++);
                    if (i != j)
                        return true;
            }
        }
        return false;
    }
};
```

### Knuth's way
``` C++
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(!head)
            return false;
        ListNode* slowPointer = head;
        ListNode* fastPointer = head;
        do {
            slowPointer = slowPointer->next;
            fastPointer = fastPointer->next;
            if(!fastPointer)
                return false;
            fastPointer = fastPointer->next;
        } while(fastPointer && fastPointer != slowPointer);
        return fastPointer && slowPointer;
    }
};
```

## Linked List Cycle II
```C++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* p = head, *t = head;
        int i, j;
        if (p) {
            for (i = 0; p->next; p = p->next, i++) {
                for (j = 0, t = head; p != t; t = t->next, j++);
                    if (i != j)
                        return t;
            }
        }
        return NULL;
    }
};
```

## Merge Two Sorted Lists
```C++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* result = NULL, *buf = NULL;
        if (l1 && l2) {
            if (l1->val < l2->val) {
                result = l1;
                l1 = l1->next;
            }
            else {
                result = l2;
                l2 = l2->next;
            }
            buf = result;
            while (l1 && l2) {
                if (l1->val < l2->val) {
                    buf->next = l1;
                    l1 = l1->next;
                }
                else {
                    buf->next = l2;
                    l2 = l2->next;
                }
                buf = buf->next;
            }
        }
        if (!(l1 || l2))
            return result;
        if (!l2) {
            if (!buf) {
                buf = l1;
                l1 = l1->next;
                result = buf;
            }
            while (l1) {
                buf->next = l1;
                buf = buf->next;
                l1 = l1->next;
            }
        }
        else if (!l1) {
            if (!buf) {
                buf = l2;
                l2 = l2->next;
                result = buf;
            }
            while (l2) {
                buf->next = l2;
                buf = buf->next;
                l2 = l2->next;
            }
        }
        return result;
    }
};
```

## Remove Nth Node From End of List
```C++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* p = head, *t = head;
        if (n == 1) {
            if (p->next) {
                while (p->next->next) {
                    p = p->next;
                }
                p->next = NULL;
                return head;
            }
            return NULL;
        }
        while (p->next) {
            n--;
            if (n < 0)
                t = t->next;
            p = p->next;
        }
        if (n > 0) {
            while (n > 0) {
                head = head->next;
                n--;
            }
        }
        else if (t->next)
            t->next = t->next->next;
        else 
            t = NULL;
        return head;
    }
};
```

## Middle of the Linked List
### Version 1:
```C++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* buf = head;
        int i;
        for (i = 0; buf->next; i++)
          buf = buf->next;
        i = i / 2 + i % 2;
        for (buf = head; i > 0; i--) 
            buf = buf->next;
        return buf;
    }
};
```
### Version 2:
```C++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* p = head, *t = head;
        while (p && p->next) {
            p = p->next->next;
            t = t->next;
        }
        return t;
    }
};
```

## Delete Node in a Linked List
```C++
class Solution {
public:
    void deleteNode(ListNode* node) {
        node->val = node->next->val;
        node->next = node->next->next;
    }
};
```

## Palindrome Linked List
```C++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        ListNode* middle;
        if (!head)
            return true;
        if (!head->next)
            return true;
        middle = GetMiddle(head);
        middle = ReverseList(middle);
        while (head && middle) {
            if (head->val != middle->val)
                return false;
            head = head->next;
            middle = middle->next;
        }
        return true;
    }
private:
    ListNode* GetMiddle(ListNode* head) {
        ListNode* p = head, *t = head;
        while (p && p->next) {
            p = p->next->next;
            t = t->next;
        }
        return t;
    }
    ListNode* ReverseList(ListNode* head) {
        ListNode* buf = head;
        if (buf) {
            while (buf->next)
                buf = buf->next;
            RecursiveThing(head);
        }
        return buf;
    }
    void RecursiveThing(ListNode* node) {
        if (!node->next)
            return;
        else 
            RecursiveThing(node->next);
        node->next->next = node;
        node->next = NULL;
    }
};
```

## Reverse Linked List
```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* buf = head;
        if (buf) {
            while (buf->next)
                buf = buf->next;
            RecursiveThing(head);
        }
        return buf;
    }
private:
    void RecursiveThing(ListNode* node) {
        if (!node->next)
            return;
        else 
            RecursiveThing(node->next);
        node->next->next = node;
        node->next = NULL;
    }
};
```

## Remove Linked List Elements
```C++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* buf, *oneMoreBuf = head;
        if (head) {
            buf = head->next;
            if (buf) {
                while (head && head->val == val) {
                    head = head->next;
                }
                buf = oneMoreBuf = head;
                while (buf) {
                    if (buf->val == val) {
                        oneMoreBuf->next = buf->next;
                        buf = buf->next;
                        continue;
                    }
                    oneMoreBuf = buf;
                    buf = buf->next;
                }
            }
            else if (head->val == val)
                return NULL;
        }
        return head;
    }
};
```

## Intersection of Two Linked Lists
```C++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int counterA, counterB;
        ListNode* bufA = headA, *bufB = headB;
        for (counterA = 0; bufA; counterA++)
            bufA = bufA->next;
        for (counterB = 0; bufB; counterB++)
            bufB = bufB->next;
        counterA = counterA - counterB;
        counterB = counterA < 0 ? -counterA : 0;
        counterA = counterB ? 0 : counterA;
        for (;counterA > 0; counterA--) {
            headA = headA->next;
        }
        for (;counterB > 0; counterB--) {
            headB = headB->next;
        }
        while (headA != headB) {
            headA = headA->next;
            headB = headB->next;
        }
        return headA;
    }
};
```

## Sort List
```C++
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if (!(head && head->next))
            return head;
        return mergeTwoLists(sortList(head), sortList(SplitList(head)));
    }
    
private:
    ListNode* SplitList(ListNode* head) {
        ListNode* p = head, *t = head, *annihilator;
        while (p && p->next) {
            p = p->next->next;
            annihilator = t;
            t = t->next;
        }
        annihilator->next = NULL;
        return t;
    }
    
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* result = NULL, *buf = NULL;
        if (l1 && l2) {
            if (l1->val < l2->val) {
                result = l1;
                l1 = l1->next;
            }
            else {
                result = l2;
                l2 = l2->next;
            }
            buf = result;
            while (l1 && l2) {
                if (l1->val < l2->val) {
                    buf->next = l1;
                    l1 = l1->next;
                }
                else {
                    buf->next = l2;
                    l2 = l2->next;
                }
                buf = buf->next;
            }
        }
        if (!(l1 || l2))
            return result;
        if (!l2) {
            if (!buf) {
                buf = l1;
                l1 = l1->next;
                result = buf;
            }
            while (l1) {
                buf->next = l1;
                buf = buf->next;
                l1 = l1->next;
            }
        }
        else if (!l1) {
            if (!buf) {
                buf = l2;
                l2 = l2->next;
                result = buf;
            }
            while (l2) {
                buf->next = l2;
                buf = buf->next;
                l2 = l2->next;
            }
        }
        return result;
    }
};
```

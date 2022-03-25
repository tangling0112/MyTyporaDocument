# LeetCode刷题笔记

## 1 两数之和

### 1.1 题目描述

​		给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

### 1.2 代码

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        hashtable = dict()
        for index,num in enumerate(nums):
            if target-num in hashtable:
                return [hashtable[target-num],index]
            hashtable[nums[index]]=index
        return []
```

### 1.3核心思想

### 1.4知识点

## 2 两数相加

### 2.1 题目描述

​		给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

### 2.2 代码

```c
struct ListNode* addTwoNumbers(struct ListNode* l1, struct ListNode* l2){
     struct ListNode* Head = NULL;struct ListNode* Tail=NULL;
     int carry = 0;
     int x = 0;
     while(l1 || l2){
         int num1 = l1?l1->val:0;
         int num2 = l2?l2->val:0;
         if(!Head){
             Head = (struct ListNode*)malloc(sizeof(struct ListNode));
             Tail = Head;
             x = num1 + num2 + carry;
             Tail->val = x%10;
             carry = (x-Tail->val)/10;
             Tail->next = NULL;
             //Tail->next = (struct ListNode*)malloc(sizeof(struct ListNode));
             //Tail = Tail->next;
         }
         else{
             Tail->next = (struct ListNode*)malloc(sizeof(struct ListNode));
             x = num1 + num2 + carry;
             Tail->next->val = x%10;
             carry = (x-Tail->next->val)/10;
             Tail = Tail->next;
             Tail->next = NULL;
         }
         if (l1){
             l1 = l1->next;
         }
         if (l2){
             l2 = l2->next;
         }
     }
     if (carry>0){
         Tail->next = (struct ListNode*)malloc(sizeof(struct ListNode));
         Tail->next->val = carry;
         Tail = Tail->next;
         Tail->next = NULL;
     }
     return Head;
}
```

### 2.3 核心思想

### 2.4知识点
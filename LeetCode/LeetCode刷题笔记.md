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

### 1.3 核心思想

- 使用哈希表来存储遍历过的数组中的数据
- 使用$target-num$的方法来得到当前数字需要的另一个数字
- 使用$target-num$在哈希表中找是否有需要的

### 1.4 知识点

#### 1.4.1 C语言中哈希表的实现

```c
import uthash//引入uthash库
    创建哈希表中的每一个项目的结构
struct HashTable{
	int key;
    int value;
    UT_hash_handle hh;//添加哈希句柄
}
```

- 在哈希表中添加项目

  ```c
  HASH_ADD_INT(<hashTableObject>,<NameOFkey>,<StructPoint>)
  ```

- 在哈希表中查找项目

  ```c
  HASH_FIND_INT(<hashTable>,<SiteOFkey>,<StructPoint>)
  ```

- 在哈希表中将对应项目替换掉,并返回该被替换的项目

  ```c
  HASH_REPLACE_INT(<hashTable>,<NameOFkey>,<NewStructPoint>,<OldStructPoint>)
  ```

- 删除哈希表中的指定项目

  ```c
  HASH_DEL(<hashTable>,<StrcutPoint>)
  ```

  

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

使用链表进行迭代计算

### 2.4 知识点

- 由于我们必须让链表的最后一个节点的`next`指针指向`NULL`因此我们的`Tail`指针必须通过`Tail->next->val;Tail=Tail->next`的方式进行赋值,这样才能保证结束计算时,`Tail`指针指向的是链表的最后一个数字对应的节点,而不是一个空结点
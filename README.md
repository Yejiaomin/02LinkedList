# 02LinkedList
203.移除链表元素

题意：删除链表中等于给定值 val 的所有节点。

示例 1： 输入：head = [1,2,6,3,4,5,6], val = 6 输出：[1,2,3,4,5]

解题思路：
链表的删除，主要把删除节点的前一个节点.next 指向 next.next。因此要找到前一个节点的话，可以引入dummy的虚拟头结点，总是在cur节点前面，这样即使删除的时候head也不受影响。

代码：
public ListNode removeElements(ListNode head, int val) {
    if (head == null) {
        return head;
    }
    // 因为删除可能涉及到头节点，所以设置dummy节点，统一操作
    ListNode dummy = new ListNode(-1, head);
    ListNode pre = dummy;
    ListNode cur = head;
    while (cur != null) {
        if (cur.val == val) {
            pre.next = cur.next;
        } else {
            pre = cur;
        }
        cur = cur.next;
    }
    return dummy.next;
}

206.反转链表

题意：反转一个单链表。

示例: 输入: 1->2->3->4->5->NULL 输出: 5->4->3->2->1->NULL

解题思路：设置一直虚拟头dummy结点在head之前（dummy = new Node())，1.用一个next节点来接管保存要翻转的当前cur 之后de节点,以免指针调头后丢失后续节点(next = cur.next)。2.把cur节点.next指向dummy（cur.next = dummy完成翻转）3.移动dummy向后一位(dummy.next = cur).移动cur向后一位（cur.next = next)。

代码：
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode cur = head;
        ListNode temp = null;
        while (cur != null) {
            temp = cur.next;// 保存下一个节点
            cur.next = prev;
            prev = cur;
            cur = temp;
        }
        return prev;
    }
}

24. 两两交换链表中的节点

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

示例: 输入: 1->2->3->4->5->NULL 输出: 2->1->4->3->5->NULL

解题思路：
1.与翻转链表相似，需要一个next指针保存被翻转的两个节点之后的节点，还需要一个temp指针来存被翻转节点的第一个，因为是被翻转节点的第二个先翻转。需要一个dummy虚拟头结点，并且cur从dummy开始，最后范围dummy.next。
1. next = cur.next.next.next(保存3); 2. temp = cur.next.next(保存2)；3.先把虚拟头结点指向被翻转的第一个节点cur.next = cur.next.next(dummy ——>2).4.第二个节点指向第一个节点：cur.next.next = temp(2 ——> 1),temp = next(1 ——> 3); 5.cur 后移两位: cur = cur.next.next(移到2处，作为后面两个节点的dummy）.
2. 因为指针多比较乱，可以设置cur 从dummy开始，temp保存被翻转的两个节点之后的节点， first来保存原来的第一个节点，second来保存原来的第二个节点，1.temp = cur.next.next.next(3); 2.cur ——> second(cur.next = secondNode);3.second ——> first(secondNode.next = firstNode); 4.first ——> temp(firstNode.next = temp); 5.移动cur两位：cur = cur.next.next（cur 又作为下面两个等待翻转的节点的dummy）。

代码：
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode cur = dummy;
        ListNode temp =null;
        ListNode firstNode =null;
        ListNode secondNode =null;
        while(cur.next != null && cur.next.next != null){
            // store the third node
            temp = cur.next.next.next;
            //orignal first node
            firstNode = cur.next;
            //orignal second node
            secondNode = cur.next.next;
            //swap the first and second nodes
            cur.next = secondNode;
            secondNode.next = firstNode;
            //link the swaped node to the third node
            firstNode.next = temp;
            //cur node moves two steps
            cur = cur.next.next;
        }
        return dummy.next;
    }

19.删除链表的倒数第N个节点

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

示例： 输入：head = [1,2,3,4,5], n = 2 输出：[1,2,3,5] 

解题思路：
链表遍历是从前往后，可以走size - n 来到达删除节点，或者用快慢指针来走，区间设置为n,快指针先走n步，然后两个指针一起走, 这样等快指针走到null，慢指针也走到删除节点了（因为快慢指针相差n);但因为是删除，所以我们实际是要让操作（慢）指针最终提留在删除节点的前一个，即：最开始fast先走n + 1 或者是后面一起走的时候只走到null的前一个就停下（fast.next= null）。 然后slow.next = slow.next.next；

代码实现：
public ListNode removeNthFromEnd(ListNode head, int n){
    ListNode dummyNode = new ListNode(0);
    dummyNode.next = head;

    ListNode fastIndex = dummyNode;
    ListNode slowIndex = dummyNode;

    // 只要快慢指针相差 n 个结点即可
    for (int i = 0; i <= n  ; i++){ 
        fastIndex = fastIndex.next;
    }

    while (fastIndex != null){
        fastIndex = fastIndex.next;
        slowIndex = slowIndex.next;
    }

    //此时 slowIndex 的位置就是待删除元素的前一个位置。
    //具体情况可自己画一个链表长度为 3 的图来模拟代码来理解
    slowIndex.next = slowIndex.next.next;
    return dummyNode.next;
}


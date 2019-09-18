# 链表和数组

1.https://leetcode.com/problems/reverse-linked-list/ 
2.https://leetcode.com/problems/linked-list-cycle 
3.https://leetcode.com/problems/swap-nodes-in-pairs 
4.https://leetcode.com/problems/linked-list-cycle-ii 
5.https://leetcode.com/problems/reverse-nodes-in-k-group/

### **环形链表**

[Leetcode :141.环形链表 (Easy)](https://leetcode-cn.com/problems/linked-list-cycle/)

解题思路：

**快慢指针，如果有环，必然相遇**

```Java
       public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        //fast.next不能为空是因为fast要向后移动两位,所以next不能为空，为空的话会出现空指针错误
        while (slow.next != null && fast.next != null && fast.next.next != null){
            slow = slow.next;
            fast = fast.next.next;
            if (slow==fast){
                return true;
            }

        }
        return false;
        
    }

```

**（2）遍历链表，使用set保存，如果有重复的节点，则添加方法返回false。**

```java
    public boolean hasCycle(ListNode head) {
        Set<ListNode> set = new HashSet<>();
        while(head != null){
            boolean flag = set.add(head);
            if (flag == false){
                return true;
            }
            head = head.next;
        }
        return false;
        
    }
```



### **环形链表2**

[Leetcode :142.环形链表 2(medium)](https://leetcode-cn.com/problems/linked-list-cycle-ii/submissions/)

解题思路：

1）首先通过快慢指针的方法判断链表是否有环；接下来如果有环，则寻找入环的第一个节点。具体的方法为，首先假定链表起点到入环的第一个节点A的长度为a【未知】，到快慢指针相遇的节点B的长度为（a + b）【这个长度是已知的】。现在我们想知道a的值，注意到快指针p2始终是慢指针p走过长度的2倍，所以慢指针p从B继续走（a + b）又能回到B点，如果只走a个长度就能回到节点A。但是a的值是不知道的，解决思路是曲线救国，注意到起点到A的长度是a，那么可以用一个从起点开始的新指针q和从节点B开始的慢指针p同步走，相遇的地方必然是入环的第一个节点A。

```Java
     public ListNode detectCycle(ListNode head) {
       ListNode fast = head;
       ListNode slow = head;
        boolean flag = false;
        ListNode  newNode = head;
        while(slow != null && fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            if(slow==fast){
                flag = true;
                break;
            }
        }
         //如果有环，从head开始一步一步出发的的指针和从快慢指针相遇的地方节点一步一步出发，相遇必然为开始入环的节点
        while(flag){
            if(newNode==fast){
                return newNode;
            }
            newNode = newNode.next;
            fast = fast.next;
            
        }
        return null;
    }

```

2）使用Set来保存，然后判断Set里面是否有重复的，如果有重复，使用add方法时返回false。

```java
    public ListNode detectCycle(ListNode head) {
        Set<ListNode> set = new HashSet<>();
        while(head != null){
            boolean flag = set.add(head);
            if (flag == false){
        
                return head;
            }
            head = head.next;
        }
        return null;
    }
```

### **链表反转**

[Leetcode : 206. Reverse Linked List (Easy)](https://leetcode.com/problems/reverse-linked-list/description/)

解题思路：

主要要记录前继节点，即每遍历一个节点，把后继节点指向前继节点，要把当前节点变为前继节点，把后继节点变为当前节点，直到遍历完毕

注意：新的前继节点应该为null。因为第一个节点翻转变成最后一个节点的next节点应该为null。

```Java
     public ListNode reverseList(ListNode head) {   
		//迭代写法
        ListNode prev = null;//新的头结点
        ListNode nowNode = head;//当前节点
        while(nowNode != null){//只要当前节点不为空
            //记录当前节点的下一个next
            ListNode next = nowNode.next;
            //把当前节点的前继节点赋值给当前节点的下一个节点,进行翻转
            nowNode.next = prev;
            //把当前节点变为下一个节点next的前继节点
            prev = nowNode;
            //把next节点变为当前节点
            nowNode = next;
            
        }
        return prev;
    }

```



```java
    public ListNode reverseList(ListNode head) {
        return reverse(null, head);
    }
    //递归，传前继节点和后继节点
    public ListNode reverse(ListNode pre, ListNode current){
        if(current == null){//如果当前节点为null，则返回前继节点
            return pre;
        }
        //记录下一个next节点
        ListNode next = current.next;
        //把当前节点的下个节点变为当前节点的前继节点
        current.next = pre;
        //循环，当前节点变为前继节点，next变为当前节点
        return reverse(current, next);
        
    }

```



### **两两交换链表中的节点**

[Leetcode : 24. 两两交换链表中的节点(medium)](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

~~~htm
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

 

示例:

给定 1->2->3->4, 你应该返回 2->1->4->3.
~~~



**解题思路**

使用一个前继节点来**记录两两**交换之后的**第二个节点**，这样在每次交换之前就可以将**上一次交换之后的节点**指向**第二对要交换的节点**的**第二节点**。

注意边界：如果没有下一个节点，直接返回head。

​		下一对节点没有2个节点，则不用交换（cur ！= null && cur.next != null）。

```Java
    public ListNode swapPairs(ListNode head) {
        //使用一个前继节点来指向一对节点交换之后的指向
        if(head == null){
            return head;
        }
        ListNode pre= head;//前继节点
        ListNode cur = head;//当前节点
        if(cur.next != null){//即有下一个节点
            head = cur.next;//把当前节点的下一个节点变成头节点,确定好头结点
            while(cur != null && cur.next != null){//有两个节点可以进行交换
                pre.next = cur.next;//pre.next指向交换节点节点之后的第二个节点
                ListNode next = cur.next.next;//保存下一对节点的开始节点
                //交换节点
                cur.next.next = cur;//把当前节点的下一个节点的next指向当前节点
                cur.next = next;
                //交换完毕，开始下一对节点的赋值
                pre = cur;
                cur = next;
                
            }
        }
        return head;
        
    }

```

### k个一组翻转链表

[Leetcode : 25. k个一组翻转链表[hard]](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

~~~html
给出一个链表，每 k 个节点一组进行翻转，并返回翻转后的链表。

k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k 的整数倍，那么将最后剩余节点保持原有顺序。

示例 :

给定这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5

说明 :

你的算法只能使用常数的额外空间。
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
~~~



解题思路：和前面的翻转链表有些相似，多了K个节点的限制。分组进行翻转，是不是可以想到分治？每个组的结果互相不影响，最后再拼接在一起。

1）要计算出链表的长度(通过遍历一次链表)

2）pre前继节点（当完成一组的翻转之后，该前继节点就会变成头结点。为什么呢？因为最后的pre = cur；）

​     next后继节点

   cur当前节点

3）如何拼接每一组翻转之后的情况呢？先观察第一组翻转之后的情况，你会发现head节点变成了最后一个节点，所以递归循环返回的节点就拼接在head.next下。

~~~Java
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode pre = null;
        ListNode cur = head;//当前节点
        ListNode next = head;//后继节点
        ListNode sum = head;//用来计算是否有K个节点的遍历
        int count = 0;
        int i = 0;
        while (sum != null){
            sum = sum.next;
            count++;
        }
        if(count < k){
            return head;
        }else {
            //对每一组进行翻转
            while (i < k && cur != null){
                next = cur.next;//记录下一个节点
                //翻转节点
                cur.next = pre;//当前节点的下一个节点指向前继节点
                pre = cur;//当前节点变成前继节点
                cur = next;//下一个节点变成当前节点
                //该while完成后，该组的翻转就完成了，pre就会指向原本的最后一个节点，翻转之后的第一个节点。head节点就变成了翻转之后的最后一个节点
                i++;
            }
            //如果next节点不为空，即后面还有节点
            if(next != null){
                //递归
                head.next =  reverseKGroup(next, k);
            }
            return pre;
        }
    }
~~~



# 堆栈

1.https://leetcode.com/problems/implement-queue-using-stacks/solution/  
2.https://leetcode.com/problems/implement-stack-using-queues/description/
3.https://leetcode.com/problems/valid-parentheses/description/

#### [leetcode 20. 有效的括号.easy](https://leetcode-cn.com/problems/valid-parentheses/)

~~~HTML
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:

输入: "()"
输出: true
示例 2:

输入: "()[]{}"
输出: true
示例 3:

输入: "(]"
输出: false
示例 4:

输入: "([)]"
输出: false
~~~

解题思路：

**使用栈。**判断的是左右括号是否匹配，那么就把遇到的左括号**入栈**，如果遇到了**右括号**，那么就**判断栈顶元素**是否对应的右括号的左括号，是就**pop出栈**并删除。因为要判断是否为有效括号，当遇到一个右括号的时候，它的上一个在栈顶的元素一定得是其对应的左括号。

时间复杂度：每个元素**入栈和出栈**都是**O(1)**，每个元素进栈且进一次，所以是**O(1) * N = O(N)**的时间复杂度

空间复杂度：最坏的情况是全部的元素都压在栈里，所以空间复杂度为**O(N）**。

~~~Java
    public boolean isValid(String s) {

        if (s.length() == 0) {
            return true;
        }

        Stack<Character> stack = new Stack<>();
        char[] chars = s.toCharArray();
        for (int i = 0; i < chars.length; i++){
            char temp = chars[i];
            if (temp == '(' || temp =='[' || temp =='{'){
                stack.push(temp);//如果是左括号就入栈
            }else {//判断右括号
                if (temp == ')') {//如果右括号是‘）’
                    if (stack.empty() || stack.peek() != '('){//判断当前栈是否为空，且栈顶是否和其对应的左括号‘（’相等
                        return false;
                    }
                } else if (temp == ']') {
                    if (stack.empty() || stack.peek() != '['){
                        return false;
                    }
                }else if (temp == '}') {
                    if (stack.empty() || stack.peek() != '{') {
                        return false;
                    }
                }
                stack.pop();
            }
        }
        return stack.isEmpty();
    }
~~~

![1557193899300](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1557193899300.png)

在使用上面的解法的时候，会发现写起来判断的时候有些许不是很方便。有什么办法呢？map！可以使用map来保持左右括号的对应关系。

右括号作为key，左括号作为value

遇到右括号则通过key 查询其对应的value左括号。

~~~Java
    public boolean isValid(String s) {

        if (s.length() == 0) {
            return true;
        }
        Map<Character,Character> map = new HashMap<>();
        map.put(')','(');
        map.put('}','{');
        map.put(']','[');
        Stack<Character> stack = new Stack<>();
        char[] chars = s.toCharArray();
        for (int i = 0; i < chars.length; i++){
            //map的key不包含左括号，即左括号就push
            if (!map.containsKey(chars[i])){
                stack.push(chars[i]);
            }else if (stack.isEmpty() || map.get(chars[i]) != stack.pop()){
                return false;
            }
        }
        return stack.isEmpty();
    }
~~~



![1557232304809](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1557232304809.png)

#### [leetcode 232. 用栈实现队列.easy](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

~~~HTML
使用栈实现队列的下列操作：

push(x) -- 将一个元素放入队列的尾部。
pop() -- 从队列首部移除元素。
peek() -- 返回队列首部的元素。
empty() -- 返回队列是否为空。
示例:

MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // 返回 1
queue.pop();   // 返回 1
queue.empty(); // 返回 false
说明:

你只能使用标准的栈操作 -- 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）。
~~~



解题思路：使用**两个栈**，一个作为**输出**，一个作为**输入栈**。既然已知两个栈，要如何实现两个栈之间的数据传输呢？

一开始我以为一有数据输入就该将其输入栈全部到输出栈，发现这样无法把队列实现。

重点：抓住队列的特性：**先进先出**。

1）**push**进来的数据**全部**放在**输入栈**。

2）当期需要**pop或者peek**的时候，**判断输出**栈为不为**空**，**为空**则将**输入**栈所有的数据**转移**到**输出栈**。不为空直接返回输出的栈顶。

3）两个栈都为空，则空

~~~Java
class MyQueue {
    private Stack inputStack;
    private Stack outputStack;

    /** Initialize your data structure here. */
    public MyQueue() {
        inputStack = new Stack();
        outputStack = new Stack();

    }
    /** Push element x to the back of queue. */
    public void push(int x) {
        inputStack.push(x);

    }
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
      if (outputStack.empty()){//如果输出栈为空，就把输入栈的全部移到输出栈
            while (!inputStack.empty()){
                outputStack.push(inputStack.pop());
            }
        }
        return (int) outputStack.pop();
    }

    /** Get the front element. */
    public int peek() {
        if (outputStack.empty()){//如果输出栈为空，就把输入栈的全部移到输出栈
            while (!inputStack.empty()){
                outputStack.push(inputStack.pop());
            }
        }
        return (int) outputStack.peek();
    }

    /** Returns whether the queue is empty. */
    public boolean empty() {
        return outputStack.empty() && inputStack.empty();
    }

}

~~~


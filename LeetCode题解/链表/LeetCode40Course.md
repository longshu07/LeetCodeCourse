[TOC]





# 链表和数组

1.https://leetcode.com/problems/reverse-linked-list/ 
2.https://leetcode.com/problems/linked-list-cycle 
3.https://leetcode.com/problems/swap-nodes-in-pairs 
4.https://leetcode.com/problems/linked-list-cycle-ii 
5.https://leetcode.com/problems/reverse-nodes-in-k-group/

### **环形链表**

[Leetcode :141.环形链表 (Easy)](https://leetcode-cn.com/problems/linked-list-cycle/)

#### 解题思路：

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

### [leetcode 20. 有效的括号.easy](https://leetcode-cn.com/problems/valid-parentheses/)

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

### [leetcode 232. 用栈实现队列.easy](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

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



### [LeetCode 225. 用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

~~~Html
使用队列实现栈的下列操作：

push(x) -- 元素 x 入栈
pop() -- 移除栈顶元素
top() -- 获取栈顶元素
empty() -- 返回栈是否为空
注意:

你只能使用队列的基本操作-- 也就是 push to back, peek/pop from front, size, 和 is empty 这些操作是合法的。
你所使用的语言也许不支持队列。 你可以使用 list 或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。
你可以假设所有操作都是有效的（例如, 对一个空的栈不会调用 pop 或者 top 操作）。
~~~



解题思路：

栈：**先进先出**

在push的时候将其进行翻转。

使用一个临时变量，来进行翻转。

1）在push的时候，将queue里面的数据全部poll到tempQueue 里面。

2）然后将temp里面的数据push到queue里面

3）再将temp里面的全部数据poll到queue里面，即**翻转**。

~~~Java
class MyStack {

    private Queue<Integer> queue;

    /** Initialize your data structure here. */
    public MyStack() {
        queue = new LinkedList<>();

    }
    /**
     * 在添加数据的时候进行翻转
     */
    /** Push element x onto stack. */
    public void push(int x) {
        Queue<Integer> temp = new LinkedList<>();
        //如果队列中不为空
        while (!queue.isEmpty()){
            //将队列中所有的数据转移到临时队列中temp
            temp.add(queue.poll());
        }
        //将新加入的添加到队列中
        queue.add(x);
        //将temp中的翻转到queue中
        while (!temp.isEmpty()){
            queue.add(temp.poll());
        }
    }

    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return queue.poll();
    }

    /** Get the top element. */
    public int top() {
        return queue.peek();
    }

    /** Returns whether the stack is empty. */
    public boolean empty() {
        return  queue.isEmpty();
    }
}
~~~

# 优先队列

1.https://leetcode.com/problems/kth-largest-element-in-astream/discuss/149050/Java-Priority-Queue 

2.https://leetcode.com/problems/sliding-window-maximum/



正常入，按优先等级出

实现机制：

1)堆（Heap）（最大堆、最小堆、斐波那契堆...）

![1557497670354](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1557497670354.png)

2）二叉搜索树



## [LeetCode 703. 数据流中的第K大元素 easy](https://leetcode-cn.com/problems/kth-largest-element-in-a-stream/)

~~~HTML
设计一个找到数据流中第K大元素的类（class）。注意是排序后的第K大元素，不是第K个不同的元素。

你的 KthLargest 类需要一个同时接收整数 k 和整数数组nums 的构造器，它包含数据流中的初始元素。每次调用 KthLargest.add，返回当前数据流中第K大的元素。

示例:

int k = 3;
int[] arr = [4,5,8,2];
KthLargest kthLargest = new KthLargest(3, arr);
kthLargest.add(3);   // returns 4
kthLargest.add(5);   // returns 5
kthLargest.add(10);  // returns 5
kthLargest.add(9);   // returns 8
kthLargest.add(4);   // returns 8
说明: 
你可以假设 nums 的长度≥ k-1 且k ≥ 1。
~~~



**解题思路：**

**方法一、**

保存前K个最大值。需要用到排序，快排的时间复杂度为**O(nlogn)**

**方法二、**

优先队列（PriorityQueue）

PriorityQueue默认维护一个最小堆实现。

​	1）添加数据的时候判断是否大于优先队列的队列头，大于则移出队列头，把新的数据添加进去。

添加数据入优先队列中的时间复杂度一般为**logK**。

~~~Java
class KthLargest {
    private int k;
    private PriorityQueue<Integer> queue;

    public KthLargest(int k, int[] nums) {
        this.k = k;
        queue = new PriorityQueue<>(k);//指定创建一个大小为3的优先队列
        //将数组中的数添加入queue中
        for (int a : nums){
            add(a);
        }
    }

    public int add(int val) {
        //如果优先队列中的个数小于k，说明需要往里面添加
        if (queue.size() < k){

            queue.offer(val);
            //如果优先队列中的最小堆小于新添加的数，则将最小的数出优先队列
        }else if (queue.peek() < val){
            queue.poll();
            queue.offer(val);//优先队列自动维护最小堆
        }
        return queue.peek();
    }
}

~~~



## [LeetCode 239.滑动窗口最大值  hard](https://leetcode-cn.com/problems/sliding-window-maximum/)

~~~html
给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

 

示例:

输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
 

提示：

你可以假设 k 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sliding-window-maximum
~~~

**解题思路：**

解法 一：

```java
/**
 * 删除滑动窗口最左边的元素，加入滑动窗口的新元素
 * 1,通过优先队列获得维持大顶堆，删除滑动窗口最左边的元素，加入滑动窗口的新元素
 *2，让最大值位于大顶堆上面
 * 
 */
```

1）使用优先队列维护一个大顶堆，问题是如何保证下一次滑动之后的是最新的K个数字？

​	(1) 在遍历过程中，i < k - 1,说明未达到K个数，不满足滑动窗口满了

​	(2) 继续添加

​	(3) 当刚好 i = k -1时，此时的queue的顶部就是当前滑动窗口的最大值，获取

​	(4) 剩下的情况就已满，要加入，则先要移除，然后再获取peek（）

~~~Java
class Solution {
 
    public int[] maxSlidingWindow(int[] nums, int k) {
       if(nums==null || nums.length<0 || k<=0 || k==1)
            return nums;
 //		priorityQueue默认的是从最小值到最大值（小顶堆），通过改写其comparator,变成大顶堆
        queue = new PriorityQueue<>(k, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        //当前可以获取的最大个数
        int[] max = new int[nums.length - k  + 1];
        
        for (int i = 0; i < nums.length; i++){
//          如果是第K个数之前和第K个数，就说明优先队列没有满，继续添加
            if(i  <  k - 1){
                queue.add(nums[i]);
            }else if(i == k- 1){
                queue.add(nums[i]);
                max[0] = queue.peek();
            }else {
//              优先队列已满，删除滑动窗口最左边的数[i - k],添加新的数
                queue.remove(nums[i - k]);
                queue.add(nums[i]);
                max[i - k + 1] = queue.peek();
            }
        }
 
 
 
        return max;
 
    }
}

————————————————
版权声明：本文为CSDN博主「励志编程小能手」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_38765867/article/details/84197314
~~~

**解法二：**

使用双端队列。java中的双端队列deque（支持在两端插入和移除元素）。deque是一个接口，实现它的类有ArrayDeque，LinkedBlockingDeque,LinkedList.

我们用双向队列，在遇到新的数的时候，将**新的数**和双向队列的**末尾**进行比较，如果末尾的数比新数小，则把**末尾的数扔掉**，直到该队列的**末尾数比新数大或者队列为空的时候**才停止。这样，我们可以保证队列里的元素是**从头到尾的降序**。由于队列中只有窗口里的数，就是窗口里的第一大数，第二大数，第三数...。

如何**保持队列**呢。每当滑动窗口的k已满，想要新进来一个数，就需要**把滑动窗口最左边的数移出队列**，**添加新的数**。

我们在添加新的数的时候，就已经移出了一些数，这样队列头部的数不一定是窗口最左边的数。

技巧：我们队列中**只存储那个数在原数组的下标**。这样可以判断该数是否为最滑动窗口的最左边的数。

~~~Java
   /**
     * 使用双端队列
     *1 保存下标
     *2 双端队列已满，有新的数进来的时候，要移除滑动窗口最左边的数
     *3 头部一直保持最大值（通过将要加入的新的值和队列尾部的最后一个值进行比较，如果最新的值比较大，则把旧数      *移除出队列，直到队列为空或者旧值比新值大。
     */


    public int[] maxSlidingWindow1(int[] nums, int k) {

        if(nums==null || nums.length<0 || k<=0 || k==1)
            return nums;

        Deque<Integer> deque = new ArrayDeque<Integer>(k);
        int[] max = new int[nums.length - k + 1];
        for (int  i = 0; i < nums.length; i++){
//          双端队列已满，有新的数进来的时候，要移除滑动窗口最左边的数
            if(!deque.isEmpty() && deque.peekFirst() == i - k){
                deque.removeFirst();

            }
//          头部一直保持最大值（通过将要加入的新的值和队列尾部的最后一个值进行比较，如果最新的值比较大，则把旧数移除出队列，直到队列为空或者旧值比新值大。
            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]){
                deque.removeLast();
            }
//          添加
            deque.offerLast(i);

//          判断当前滑动窗口中的最大值，获取最大值
//          当 i 大于等于 k - 1时滑动窗口才满，才有最大值
            if ( i >= k - 1){
                max[i - k + 1] = nums[deque.peekFirst()];
            }
        }
        return max;
    }

~~~

![1567254062069](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1567254062069.png)



# 哈希表



## 二数之和

解法一：

暴力解决，时间复杂度是O(N的平方)

~~~Java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if( nums.length == 0 )
            return new int[0];
        int[] result = new int[2];
        for(int i = 0; i < nums.length; i++){
            for(int j = i + 1; j < nums.length; j++){
                if(nums[i] + nums[j] == target ){
                    result[0] = i;
                    result[1] = j;
                    return result;
                }

            }
        }
        return new int[0];
        
    }
}
~~~





## 242有效的字母异位

~~~HTML
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。
 * 
 * 示例 1:
 * 
 * 输入: s = "anagram", t = "nagaram"
 * 输出: true
 * 
 * 
 * 示例 2:
 * 
 * 输入: s = "rat", t = "car"
 * 输出: false
 * 
 * 说明:
 * 你可以假设字符串只包含小写字母。
 * 
 * 进阶:
 * 如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？
~~~

**解题思路**

**解法一：**

对字符串变化成字符数组，然后排序，然后判断是否相等

~~~Java
    public static void main(String[] args){


        String s = "bca";
        String t = "abc";
        System.out.println(isAnagram(s, t));
    }
    public static boolean isAnagram(String s, String t) {
        char[] stringsS = s.toCharArray();
        char[] stringT = t.toCharArray();
        Arrays.sort(stringsS);
        Arrays.sort(stringT);

        if (Arrays.equals(stringsS,stringT)){
            return true;
        }
        return false;
    }
~~~



**解法二：**

使用map来存储记录

使用两个map来记录，如果两个map相等，则是。

map的key记录其字符，value记录他的值

~~~Java
class Solution {
    public  boolean isAnagram(String s, String t) {
        if(s.length() != t.length()){
            return false;
        }
        Map<Character, Integer> sMap =  new HashMap<>();
        Map<Character, Integer> tMap = new HashMap<>();
        sMap = isMap(sMap, s);
        tMap = isMap(tMap, t);
		//判断Map是否相等
        if (sMap.equals(tMap)){
            return true;
        }
        return false;


    }
	//将两个字符串每一个字符存在map里，记录其个数
    public Map<Character, Integer> isMap(Map<Character, Integer> map, String s){
        for(int i = 0; i < s.length(); i ++){
//           如何已经存在，则加一
            if (map.containsKey(s.charAt(i))){
                map.put(s.charAt(i), map.get(s.charAt(i)) + 1);
            }else {
                map.put(s.charAt(i), 0);
            }

        }
        return map;

    }
}
~~~



## 三数之和



# 二叉树

## 验证二叉树搜索树



解题思路

~~~html
递归解决，通过引入两个数判断最大值和最小值
左子树只有最大值，右子树只有最小值

~~~



~~~java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if(root == null){
            return true;
        }
        return isValid(root, null, null);
    }

    public boolean isValid(TreeNode root, Integer min, Integer max) {
        if(root == null ){
            return true;
        }
        //判断右子树是否符合大于最小值，如果小于等于则返回false，对右子树来说，max一直为null
        if(min != null && root.val <= min)
            return false;
        //判断左子树是否符合小于最大值，如果大于等于则返回false，对于左子树来说，min一直为null
        if(max != null && root.val >= max){
            return false;
        }

        return isValid(root.left, min, root.val) && isValid(root.right, root.val, max);

    }
}
~~~

## 236 二叉树的最近祖先



题目

```html
 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
 * 
 * 百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x
 * 的深度尽可能大（一个节点也可以是它自己的祖先）。”
 * 
 * 例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]
 * 
 * 
 * 
 * 
 * 
 * 示例 1:
 * 
 * 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
 * 输出: 3
 * 解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
 * 
 * 
 * 示例 2:
 * 
 * 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
 * 输出: 5
 * 解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
 * 
 * 
 * 
 * 
 * 说明:
 * 
 * 
 * 所有节点的值都是唯一的。
 * p、q 为不同节点且均存在于给定的二叉树中。
 * 
```



解题思路

思路1：path：找到最先相遇的公共节点，变成两个链表，但是没有父亲节点
思路2：从root往下找，找到P，然后找到P，然后查询两个链表里面P、q 最后相遇的节点，即最近祖先时间复杂度O(N)
**思路3：Recursion递归**
如果是在根节点，那么立即返回根节点
如果pq都在左子树，这样只需要寻找左子树，右子树返回的为null
如果pq都在右子树，左子树返回的就是为null，这样只需要寻找右子树
每个节点都要访问且访问一次
	辅助函数 -findPOrQ（root， p， q）：找到P或q都返回
		if（root == p Or root == q）
			return root
		递归
		findPOrQ（root.left,p,q)
		findPOrQ（root.right,p,q)



~~~Java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 递归终止条件
        // root为null或者找到p或q中的一个，返回root
        if(root == null || root == p || root == q){
            return root;
        }

        // 左子树递归
        TreeNode left  = lowestCommonAncestor(root.left, p, q);

        // 右子树递归
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        if(left == null){
            // 说明在左子树找不到p或者q，返回右子树
            return right;
        }
        if(right == null){
            // 说明在右子树找不到p或者q，返回左子树
            return left;
        }
        //左右子树都能找到p或者q，则说明root才是他们的最近祖先节点
        return root;
    }
}
~~~



## 265 搜索二叉树的最近祖先

**题目**

~~~html
给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
 * 
 * 百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x
 * 的深度尽可能大（一个节点也可以是它自己的祖先）。”
 * 
 * 例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]
 * 示例 1:
 * 
 * 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
 * 输出: 6 
 * 解释: 节点 2 和节点 8 的最近公共祖先是 6。

 * 示例 2:
 * 
 * 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
 * 输出: 2
 * 解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
 * 
 * 
 * 
 * 说明:
 * 
 * 
 * 所有节点的值都是唯一的。
 * p、q 为不同节点且均存在于给定的二叉搜索树中。
~~~



**解题思路**

​       分成三部分：

​        左子树： root > q && root > p 说明他们的最近祖先在左子树这边

​        右子树：root < q && root < p 说明他们的最近祖先在右子树这边

​        **剩下的情况**

​        root：如果不符合上面的条件，即 root == p 或者root == q亦或pq在左右子树两边说明当前节点就是最近公共祖先

​     

~~~Java

//递归方式
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        /*
        分成三部分：
        左子树： root > q && root > p 说明他们的最近祖先在左子树这边
        右子树：root < q && root < p 说明他们的最近祖先在右子树这边
        剩下的情况
        root：如果不符合上面的条件，即 root == p 或者root == q说明当前节点就是最近公共祖先
        */

        if(root.val > q.val && root.val > p.val){
            return lowestCommonAncestor(root.left, p, q);
        }
        if(root.val < q.val && root.val < p.val){
            return lowestCommonAncestor(root.right, p, q);
        }
        return root;
        
    }
}
~~~

~~~Java
//非递归方式
//易错点，if else要囊括全部的可能性
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        /*
        分成三部分：
        左子树： root > q && root > p 说明他们的最近祖先在左子树这边
        右子树：root < q && root < p 说明他们的最近祖先在右子树这边
        剩下的情况
        root：如果不符合上面的条件，即 root == p 或者root == q亦或pq在左右子树两边说明当前节点就是最近公共祖先
        */

        while (root != null) {
            //左子树： root > q && root > p 说明他们的最近祖先在左子树这边
            if(root.val > q.val && root.val > p.val){
                root = root.left;
            }else if(root.val < q.val && root.val < p.val){
                //右子树：root < q && root < p 说明他们的最近祖先在右子树这边
                root = root.right;
            }else{
                //剩下的情况
                //root：如果不符合上面的条件，即 root == p 或者root == q亦或pq在左右子树两边 , 说明当前节点就是最近公共祖先
                return  root;
            }
        }
        return null;
    
        
    }
}
~~~

## 

# 递归模板

~~~Java
//level就是递归的层数，盗梦空间的梦境层数
recursion(level, param1, param2,...){
    //递归终止条件
    if(level > MAX_LEVEL){
        return
    }
    //递归之前的数据处理
    process_data(level, data);
    
    //进入下一层梦境，参数变化
    recusion(level + 1, p1...);
    //收尾，翻转状态
    reverse_state(level);
}
~~~

# 分治 Divde & Conquer

~~~Java
divide_conquer(problem, param1, param2...){
 //递归终止条件
    if(problem == null){
        return;
    }
 
    //数据的准备
    data = prepare_data(problem);
    subproblems = split_problem(problem, data);
    
    //分解子问题
    subResult1= divide_conquer(subprblems[0], p1...);
    subResult2= divide_conquer(subprblems[1], p1...);
    subResult3= divide_conquer(subprblems[2], p1...);
    ...
    //对子结构合并
    result = process_result(subResult1, subResult2,subResult3...)
        
    
}
~~~



## 50 实现 pow(x, n) ，即计算 x 的 n 次幂函数。

题目

~~~html
实现 pow(x, n) ，即计算 x 的 n 次幂函数。
 * 
 * 示例 1:
 * 
 * 输入: 2.00000, 10
 * 输出: 1024.00000
 * 
 * 
 * 示例 2:
 * 
 * 输入: 2.10000, 3
 * 输出: 9.26100
 * 
 * 
 * 示例 3:
 * 
 * 输入: 2.00000, -2
 * 输出: 0.25000
 * 解释: 2^-2 = 1/2^2 = 1/4 = 0.25
 * 
 * 说明:
 * 
 * 
 * -100.0 < x < 100.0
 * n 是 32 位有符号整数，其数值范围是 [−2^31, 2^31 − 1] 。
~~~



**解题思路**

思路1：调用函数库O(1）

思路2：暴力，for循环每一个O(N)

**思路3：有多少个N，N的奇数或者偶数， N的减半次数O(logn)**

![1568451476443](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568451476443.png)

N的减半次数O(logn)

解法一

暴力解法

~~~Java
/**
计算超时
*/
class Solution {
    public double myPow(double x, int n) {
        boolean flag = false;
        // 如果n是负数 
        if( n < 0){
            n = Math.abs(n);
            flag = true;
        }

        double result = 1.0;
        for(int i = 0; i < n; i++){
            result *= x;
        }
        if(flag){
            result = 1.0 / result;
        }

        return result;
    }
}
~~~



解法二

思路3：有多少个N，N的奇数或者偶数， N的减半次数O(logn)

![1568451476443](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568451476443.png)

N的减半次数O(logn)

对N的次数每次减半，直到n=0，即递归

~~~Java
/**
return 1 / myPow(x, Math.abs(n));
会溢出
因为int的最小值是2的-31次幂，int的最大值是2的-31次幂减1
所以这里会有案例无法通过
*/
class Solution {
    public double myPow(double x, int n) {
        if(n == 0){
            return 1.0;
        }
        if(n < 0){
            return 1 / myPow(x, Math.abs(n));
        }else{
            // 解决n的奇数偶数
            if( n % 2 != 0){
                // 奇数
                double r = myPow(x, (n-1) /2);
                return r * r * x;
            }else{
                double r = myPow(x, n /2);
                return r * r;
            }
        }

    
    }
    
}
~~~

~~~Java
/**
没有对n进行取整操作，不会有溢出
而是改变了终止条件
当n == 1时，说明是正整数，不需要1 /x
当 n == -1时，说明是负整数，返回一个 1 / x
*/

class Solution {
    public double myPow(double x, int n) {

        if (n == 0) {
            return (double) 1;
        }
        //使用 n等于1或者-1为终止条件
        if (n == 1) {
            return x;
        } else if (n == -1) {
            return 1 / x;
        }
        //为偶数时
        if ((n % 2) == 0) {
            double r = myPow(x, n / 2);
            return r * r;
        } else {
            //为奇数时
            double r = myPow(x, n / 2);
            //因为为奇数时，还需要乘上一个本身，如果是负数时，需要将本身取反
            if( n < 0){
                x = 1 / x;
            }
            return r * r * x;
        }
    }
    
}
~~~

## 169 求众数

**题目**

~~~html
给定一个大小为 n 的数组，找到其中的众数。众数是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在众数。

示例 1:

输入: [3,2,3]
输出: 3
示例 2:

输入: [2,2,1,1,1,2,2]
输出: 2
~~~

**解题思路**

思路一：双重for循环，找寻其count的个数大于 n/2 的数， 时间复杂度O(N^2)

思路二：使用map存储当前的数字，key存数字，map存个数，只用需要一次for循环，而map的操作是o（1),整体的时间复杂度为O(N)

思路三：先排序，再找其中间值，即是众数 时间复杂度为O(nlogn）

思路四：分治，通过分成两部分，然后寻找其中一部分的count最大值，如果左边的等于右边的说明是同一个数字，如果是左边的大，那就是左边的为众数，同理右边的也是

 * 分治
 * 一、递归终止条件，左右两边为同一个数
 * 
 * 二、数据准备：找寻中间值，从新赋予首尾值
 * 
 * 三、分解子问题（递归）
 * 
    *四、 对子结构合并
 * 查询当前返回的左边的数是否等于右边，等于说明这就是众数
 * 不等于，就遍历当前的首尾节点，找出谁的数多，多的就返回

**解法一**

~~~Java

//思路一：双重for循环，找寻其count的个数大于 n/2 的数， 时间复杂度O(N^2)
//超出时间限制
//注意当数组为1时，应该直接返回当前数组的数
class Solution {
    public int majorityElement(int[] nums) {
        if(nums.length == 0){
            return -1;
        }
        if(nums.length == 1){
            return nums[0];
        }
        for(int i = 0; i < nums.length - 1; i++){
            int count = 1;
            for(int j = i + 1; j < nums.length; j++){
                
                if(nums[i] == nums[j]){
                    count++;
                    if(count > nums.length / 2){
                        return nums[i];
                    }
                }
                
            }
        }
        return -1;
    }
}
~~~



**解法二**

思路二：使用map存储当前的数字，key存数字，map存个数，只用需要一次for循环，而map的操作是o（1),整体的时间复杂度为O(N)

~~~Java
/**
 * Map中的<key,value>存当前数，当前出现的个数
 * 存在于map中则替换加1
 */
class Solution {
    public int majorityElement(int[] nums) {

        if(nums.length == 0)
            return -1;
        //只有一个数的时候，直接返回
        if(nums.length == 1){
            return nums[0];
        }
        Map<Integer,Integer> map = new HashMap<>();
       for(int i = 0; i < nums.length; i++){
           if(map.containsKey(nums[i])){
           
                map.replace(nums[i], map.get(nums[i]) + 1);
              
                if(map.get(nums[i]) > nums.length / 2){
                    return nums[i];
                }

           }else
                map.put(nums[i], 1);

           
       }
       return -1;
    }
}
~~~

**解法三**

思路三：先排序，再找其中间值，即是众数 时间复杂度为O(nlogn）

~~~Java
/**
 * 先排序，然后找大于 n/2的数
 * 
 */
class Solution {
    public int majorityElement(int[] nums) {

        if(nums.length == 0)
            return -1;
        //只有一个数的时候，直接返回
        if(nums.length == 1){
            return nums[0];
        }
        Arrays.sort(nums);
        
       
       return nums[nums.length / 2];
    }
}
~~~



**解法四**

思路四：分治，通过分成两部分，然后寻找其中一部分的count最大值，如果左边的等于右边的说明是同一个数字，如果是左边的大，那就是左边的为众数，同理右边的也是

 * 分治
 * 一、递归终止条件，左右两边为同一个数
 * 
 * 二、数据准备：找寻中间值，从新赋予首尾值
 * 
 * 三、分解子问题（递归）
 * 
    *四、 对子结构合并
 * 查询当前返回的左边的数是否等于右边，等于说明这就是众数
 * 不等于，就遍历当前的首尾节点，找出谁的数多，多的就返回

~~~Java
/**
 * 分治
 * 一、递归终止条件，左右两边为同一个数
 * 
 * 二、数据准备：找寻中间值，从新赋予首尾值
 * 
 * 三、分解子问题（递归）
 * 
 *四、 对子结构合并
 * 查询当前返回的左边的数是否等于右边，等于说明这就是众数
 * 不等于，就遍历当前的首尾节点，找出谁的数多，多的就返回
 * 
 * 
 */
class Solution {
    public int majorityElement(int[] nums) {
        return divide_conquer(nums, 0, nums.length - 1);
    }

    public int divide_conquer(int[] nums, int begin, int end){
        //终止条件,返回的是数值，不是下标
        if(begin == end){
            return nums[begin];
        }

        //数据准备
        int mid = (begin + end) / 2;

        //分解子问题
        int left = divide_conquer(nums, begin, mid);
        int right = divide_conquer(nums, mid + 1, end);

        //对子结构合并
        if(left == right){
            //如果左右子问题的解相同，则返回
            return left;
        }else{
            //不同，则遍历当前的数组长度，找寻左右问题的解谁更多

            //左右两部分的众数不相同 则这两个数都有可能是这部分的众数
            //那么遍历这个数组 看一下哪个数字的出现频率高
            int countLeft = 0;
            int countRight = 0;

            //这里遍历需要包括end
            for(int i = begin; i <= end; i++){

                if(nums[i] == left){
                    countLeft++;
                }else if(nums[i] == right){
                    countRight++;
                }

            }

             //判断谁更多
             if(countLeft > countRight){
                return left;
            }else{
                return right;
            }

        }
    }
}
~~~

## 122 买卖股票最佳时机

**题目**

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格

 \* 设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

 \* 注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

 \* 示例 1

 \* 输入: [7,1,5,3,6,4]

 \* 输出: 7

 \* 解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。

 \* 随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。

 \* 示例 2

 \* 输入: [1,2,3,4,5]

 \* 输出: 4

 \* 解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4

 \* 注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。

 \* 因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。

~~~Java
class Solution {
    /**
     * 贪心解法
     * 因为可以无限交易，所以只要发现明天的比今天的大，就可以抛出
     * @param prices
     * @return
     */
    public int maxProfit(int[] prices) {
        int result = 0;
        for(int i = 0; i < prices.length - 1; i++){
            if(prices[i] < prices[ i+ 1]){
                result += prices[i + 1] - prices[i];
            }
        }
        return result;
       
        
    }
}
~~~



# BFS代码模板

~~~Java
BFS(graph, start, end){
    Queue<> queue = new LinkedList<>();
    queue.add([start]);
    //盼重数组
    visited.add(start);
    while(queue.isEmpty()){
        //取出头结点
        node = queue.pop();
        //添加标记
        visited.add(node);
        
        process(node);
        //判断是否被访问过
        nodes = generate_related_nodes(node);
        queue.add(nodes);
    }
}
~~~

# DFS代码模板

~~~Java
//递归写法
visited = new HashSet<>();
DFS(node, visited){
    visited.add(node);
    
    //找寻node的子节点
    for(next_node in node.children()){
        //如果该子节点没有被访问，则DFS
        if(next_node not in visitd)
            DFS（next_noed, visited)
    }
}
~~~

~~~Java 
/**
非递归写法，手动维护一个stack栈
*/
DFS(Tree root){
    if(root == null){
        return;
    }
    visited = new HashSet<>();
    stack.push(root);
    while(stack.isEmpty){
        node = stack.pop();
        visited.add(node);
        
        process(node);
        node = generate_related_nodes(node);
        stack.push(nodes);
    }
    
}

~~~

## 102 二叉树的层次遍历

**题目**

~~~HTML
给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）。

例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]
~~~



**解题思路**

**解法一：**使用BFS

~~~Java
 /**
  * BFS
  遍历每一层
  要点：如何判断已经遍历完了这一层？
  使用一个队列，每一层的大小通过上一层的遍历加入到队列中，然后循环队列中的剩下的数，就是当前层的数量
  */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        //判断是否符合二叉树
        if(root == null){
            return new ArrayList<>();
        }

        return BFS(root);
        
    }

    //BFS
    public List<List<Integer>> BFS(TreeNode root){
        //定义结果集
        List<List<Integer>> result = new ArrayList<>();

        //定义队列
        Queue<TreeNode> queue = new LinkedList<>();

        // 头结点进队列
        queue.add(root);

        while(!queue.isEmpty()){
            //获取当前层的节点数
            int currentLevelSize = queue.size();
           //当前层的list
            List<Integer> currentList = new ArrayList<>();
            //遍历当前层的个数，即队列中有多少个就是当前层的个数
            for(int i = 0; i < currentLevelSize; i++){
                 //头结点出队列
                TreeNode currentNode = queue.poll();
                //加入当前层的list
                currentList.add(currentNode.val);
			  //如果当前节点存在左子树，加入队列
                if(currentNode.left != null){
                    queue.add(currentNode.left);
                }

                //如果当前节点存在右子树，加入队列
                if(currentNode.right != null){
                    queue.add(currentNode.right);
                }
            }
            
            result.add(currentList);
        }
        return result;
        

    }
}
~~~

**解法二：使用DFS**

~~~Java
/**
 * 使用DFS
 * 是先添加列，而不是行
 * 例子：
 * 第一次：
 *  3
 *  9
 * 第二次：
 *  3
 *  9
 *  15
 * 要点：根据上面的做法，重点落在了如何判断当前的数是哪一层的数并添加
 * 解决办法：添加一个level参数，level从0开始，如果result的size和level相等，说明需要新加一个子list。
 * 例子：当root经入result中，他的大小为1，那么下一个level也为1，这样就需要添加一个新的list了
 */
class Solution {
        public List<List<Integer>> levelOrder(TreeNode root) {
            
            if(root == null){
                return new ArrayList<>();
            }
    
            return DFS(root, 0, new ArrayList<>());
            
        }
    
        public List<List<Integer>> DFS(TreeNode root, int level, List<List<Integer>> result){
           //终结条件
           if(root == null){
               return result;
           }

           List<Integer> list;
           //判断level
           //level与result的大小相等，则需要新增子list
           if(level == result.size()){
                list = new ArrayList<>();
                list.add(root.val);
                result.add(list);
           }else{
               //不相等，则需要从result中拿去数据，添加
               
               list = result.get(level);
               list.add(root.val);

               result.set(level, list);
           }

           DFS(root.left, level + 1, result);
           DFS(root.right, level + 1, result);

           return result;
    
        }
}
~~~

## 104 二叉树的最大深度

题目

~~~HTML
给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

示例：
给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。
~~~

使用递归

**解法一：DFS**

~~~Java 
/**
使用了DFS
*/
class Solution {
    public int maxDepth(TreeNode root) {
        return DFS(root, 0, 0);
        
    }
    public int DFS(TreeNode root, int level, int max){
        // end condition
        if(root == null){
            //如果当前层大于最大深度max，则将当前level赋给最大值
            if(level > max) 
                max = level;
               return max;
        }

        int left = DFS(root.left, level + 1, max);
        int right = DFS(root.right, level + 1, max);

        //最大值要么在左子树这边，要么在右子树这边
        return left > right ? left : right;
    }
}
~~~

DFS简单写法

~~~Java
class Solution {
    public int maxDepth(TreeNode root) {
        //只要root等于null就返回0，否则就返回递归左子树和右子树的最大值  + 1
        return root == null ? 0 :
        1 + Math.max(maxDepth(root.left), maxDepth(root.right));       
    }
}
~~~





**解法二：使用BFS**

~~~Java 
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        return BFS(root);
        

        
    }

    public int BFS(TreeNode root){
        //定义队列
        Queue<TreeNode> queue = new LinkedList<>();
        //定义最大值
        int max = 0;
        //头结点入队列
        queue.add(root);

        //只要不为空就循环
        while( !queue.isEmpty() ){
            int currentLevelSize = queue.size();
            //每一层的循环就  max +  1
            max++;
            
            for(int i = 0; i < currentLevelSize; i++){
                //在循环里面出队列，是要把当前层的数字全部出队列，这样下一层的个数就不会有影响了
                TreeNode curentTreeNode = queue.poll();
          
                if(curentTreeNode.left != null){
                    queue.add(curentTreeNode.left);
                }
                if(curentTreeNode.right != null){
                    queue.add(curentTreeNode.right);
                }
            }
        }
        return max;

    }
   
}
~~~

## 111 二叉树的最小深度

题目

~~~html
给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

说明: 叶子节点是指没有子节点的节点。

示例:

给定二叉树 [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回它的最小深度  2.
~~~



解法一：DFS

要点：特殊情况的处理

**如果没有左子树，则只需要查询右子树**

**如果没有右子树，则只需要查询左子树**

~~~Java
class Solution {
    public int minDepth(TreeNode root) {
        return DFS(root, 0, Integer.MAX_VALUE);
        
    }
    public int DFS(TreeNode root, int level, int min){
        // end condition
        if(root == null){
            //如果当前层小于min，则将当前level赋给最小值
            if(level < min) 
                min = level;
               return min;
        }

        //特殊情况

        //如果左子树为空，则在右子树中查找
        if(root.left == null){
            return DFS(root.right, level + 1, min);
        }
        //如果右子树为空，则在左子树中查找
        if(root.right == null){
            return DFS(root.left, level + 1, min);
        }

        //分治
        int left = DFS(root.left, level + 1, min);
        int right = DFS(root.right, level + 1, min);

        //最小值要么在左边，要么在右边
        return left < right ? left : right;
    }
}
~~~

~~~Java

class Solution {
    public int minDepth(TreeNode root) {
        if(root == null)
            return 0;
        //特殊情况处理
        //如果左子树为空，则在右子树中查找
        if(root.left == null){
            return  1 + minDepth(root.right);
        }
        //如果右子树为空，则在左子树中查找
        if(root.right == null){
            return 1 + minDepth(root.left);
        }

        //分治
        int left = minDepth(root.left);
        int right = minDepth(root.right);

        return  1 + Math.min(left, right);

        
    }

}
~~~



## 22 括号生成

**题目**

~~~html
给出 n 代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且有效的括号组合。

例如，给出 n = 3，生成结果为：

[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
~~~



**解题思路**

思路一：数学归纳法

**思路二：Recursion  2 X n**

将其抽象出一个个格子，n个括号对，即有2n个格子，每个格子可以放左括号或者右括号，遍历2^2n次，把全部情况都判断，然后判断合法情况

**剪枝：**

（1）局部不合法，不再递归

（2）记住左括号和右括号的使用次数

（3）递归终止条件，左括号使用次数和右括号使用次数都等于n

时间复杂度：O(2^n)

已经使用过的右括号一定会比使用过的左括号少，因为如果一上来就是右括号，这是不合法的

~~~Java
class Solution {
   
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
         DFSJianZhi(n, 0, 0, "", result);

         return result;
    }

    public void DFSJianZhi(int n, int usedLeft, int usedRight, String subResult, List<String> result){
        //终止条件,左右括号都用完了
        if(usedLeft == n && usedRight == n){
            //终止时的子结果就是当前的字符串
            result.add(subResult);
            return ;
        }

        //局部优化，剪枝

        //使用过的左括号要小于n
        if(usedLeft < n){
            DFSJianZhi(n, usedLeft + 1, usedRight, subResult + "(", result);

        }

        //右边使用过的括号要小于左边括号，因为左边要先使用，右边再用才合法
        if(usedRight < n && usedRight < usedLeft){
           DFSJianZhi(n, usedLeft, usedRight + 1, subResult + ")", result);
        }
    }
}
~~~



## [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

**题目**

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/original_images/17_telephone_keypad.png)

示例:

输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].

**解题思路**

和22的括号生成很相似，需要找到**组合的全部可能性，使用DFS**

要点：开始的时候没有找准终止条件，这里的终止条件就是**输入的数据的长度等于index时**。

~~~Java
class Solution {

    public List<String> letterCombinations(String digits) {

        if(digits.isEmpty()){
            return new ArrayList<>();
        }
        Map<Character,String> map= new HashMap<>();

        List<String> result = new ArrayList<>();
        map.put('2', "abc");
        map.put('3', "def");
        map.put('4', "ghi");
        map.put('5', "jkl");
        map.put('6', "mno");
        map.put('7', "pqrs");
        map.put('8', "tuv");
        map.put('9', "wxyz");

        DFS(digits, 0, "", result);
        
        return result;
    }
        /**
     * 
     * @param digits 输入的数字字符串
     * @param index 输入的数字字符串的下标索引
     * @param subString 子字符串
     * @param result 最终结果集
     */
    public void DFS(String digits, int index, String subString, List<String> result){
        //如果输入的数字已经遍历完毕，就返回
        if(index == digits.length()){
            result.add(subString);
            return;
        }

        //获取digits中的数字
        Character digitsIndex = digits.charAt(index);
        //获取数字对应的字母
        String currentString =  map.get(digitsIndex);
        //遍历当前数字对应字符串，分别进行递归，而子字符串的也要添加当前的字母
        for(int i = 0; i < currentString.length(); i++){
            DFS(digits, index + 1, subString + "" + currentString.charAt(i), result);
        }
    }
}
~~~



## [？438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

题目



## 169 求众数

~~~HTML
给定一个大小为 n 的数组，找到其中的众数。众数是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。
 * 
 * 你可以假设数组是非空的，并且给定的数组总是存在众数。
 * 
 * 示例 1:
 * 
 * 输入: [3,2,3]
 * 输出: 3
 * 
 * 示例 2:
 * 
 * 输入: [2,2,1,1,1,2,2]
 * 输出: 2
~~~

解题思路

思路一：**递归分治**

思路二：排序，取中间值



**解法**

**解法一**

 \* 分治

 \* 递归终止条件，左右两边为同一个数

 \* 数据准备：找寻中间值，从新赋予首尾值

 \* 分解子问题（递归）

 \* 对子结构合并

 \* 查询当前返回的左边的数是否等于右边，等于说明这就是众数

 \* 不等于，就遍历当前的首尾节点，找出谁的数多，多的就返回

~~~Java
class Solution {
    public int majorityElement(int[] nums) {
        return divide_conquer(nums, 0, nums.length - 1);
    }

    public int divide_conquer(int[] nums, int begin, int end){
        //终止条件,返回的是数值，不是下标
        if(begin == end){
            return nums[begin];
        }

        //数据准备
        int mid = (begin + end) / 2;

        //分解子问题
        int left = divide_conquer(nums, begin, mid);
        int right = divide_conquer(nums, mid + 1, end);

        //对子结构合并
        if(left == right){
            //如果左右子问题的解相同，则返回
            return left;
        }else{
            //不同，则遍历当前的数组长度，找寻左右问题的解谁更多

            //左右两部分的众数不相同 则这两个数都有可能是这部分的众数
            //那么遍历这个数组 看一下哪个数字的出现频率高
            int countLeft = 0;
            int countRight = 0;

            //这里遍历需要包括end
            for(int i = begin; i <= end; i++){

                if(nums[i] == left){
                    countLeft++;
                }else if(nums[i] == right){
                    countRight++;
                }

            }

             //判断谁更多
             if(countLeft > countRight){
                return left;
            }else{
                return right;
            }

        }
    }
}
~~~



# 剪枝

## N皇后问题

# 动态规划（Dynamic Program）

**1.递归 + 记忆化 ------>递推**，重点就是**递推（从尾部往头部递推）**，递归+记忆化只是过渡

**2.状态的定义：opt[n], dp[n], fib[n]**

**3.状态的转移方程：opt[n] = best_of(opt[n - 1], opt[n - 2],...)**

**4.最优子结构**

### 斐波拉契：

![1568546813658](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568546813658.png)

### 找路径

![1568546969927](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568546969927.png)

![1568547148928](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568547148928.png)

上述会有重复计算，时间复杂度为**O(2^n）**添加记忆化

递推：从end 往 start 推，下面的一表示只能往下走或者只能往右走

![1568547694270](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568547694270.png)

递推出状态方程，时间复杂度为**O(M*N)**

~~~Java 
opt[i, j] = opt[i - 1, j] + opt[i, j - 1];
//          表示往下走的步数     往右走的步数
~~~

~~~Java
if(a[i, j] == "空地"){
	opt[i, j] = opt[i - 1, j] + opt[i, j - 1];
}else{
    opt[i, j] = 0;
}
~~~



![1568547966695](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568547966695.png)

3表示这一格有3种走法走到终点， 可以往右走，也可以往下走。往下走的话就只有一种走法，往右走有2种走法





![1568548526816](C:\Users\龙叔\AppData\Roaming\Typora\typora-user-images\1568548526816.png)



## 70 爬楼梯

**题目**

~~~HTML
 假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
 * 
 * 每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
 * 
 * 注意：给定 n 是一个正整数。
 * 
 * 示例 1：
 * 
 * 输入： 2
 * 输出： 2
 * 解释： 有两种方法可以爬到楼顶。
 * 1.  1 阶 + 1 阶
 * 2.  2 阶
 * 
 * 示例 2：
 * 
 * 输入： 3
 * 输出： 3
 * 解释： 有三种方法可以爬到楼顶。
 * 1.  1 阶 + 1 阶 + 1 阶
 * 2.  1 阶 + 2 阶
 * 3.  2 阶 + 1 阶
~~~



**思路**

**思路一：回溯**

递推公式：

`f(n) = f(n - 1) + f(n - 2)`

`f(0) = f(1) = 1`



**思路二：动态规划**

**DP状态的定义**

**DP的状态方程**

`for: i = 2; i <= n:`

​	`f[n] = f[n - 1] + f[n -2]`



**解法一：递归 分治 回溯**

~~~Java
//提交超出时间限制
class Solution {
    public int climbStairs(int n) {
        if( n == 0){
            return 0;
        }
        if( n == 1){
            return 1;
        }
        if( n == 2){
            return 2;
        }
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
}
~~~



**解法二：动态规划**

**DP状态的定义**

**DP的状态方程**

~~~Java
/**
 * 时间复杂度O(N)
 */
class Solution {
    public int climbStairs(int n) {
        if(n <= 2){
            return n; 
        }
       
        int[] opt = new int[n];
        // 只有一阶梯
        opt[0] = 1;
        //只有两个阶梯
        opt[1] = 2;
        for(int i = 2; i < n; i++){
            opt[i] = opt[i - 1] + opt[i - 2];
        }
        return opt[n - 1];
    }
}
~~~



只用两个变量来存储前一步和前两步的操作，时间复杂度一样为O(N)

~~~Java
class Solution {
    public int climbStairs(int n) {
        if(n <= 2){
            return n; 
        }
       
        int oneStepBefore = 2;
        int towStepBefore = 1;
        int allCount = 0;
        for(int i = 2; i < n; i++){
            allCount = oneStepBefore + towStepBefore;
            // 更新前一步和前两步的数值
            // oneStepBefore变成了前两步
            towStepBefore = oneStepBefore;
            //前一步的操作更新为最新的allCount
            oneStepBefore = allCount;
        }
        return allCount;
    }
}
~~~



## 120  三角形最小路径和

**题目**

~~~html

 给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。
 * 
 * 例如，给定三角形：
 * 
 * [
 * ⁠    [2],
 * ⁠   [3,4],
 * ⁠  [6,5,7],
 * ⁠ [4,1,8,3]
 * ]
 * 
 * 
 * 自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
~~~



**解题思路**

**思路一：DFS**

时间复杂度O(2^n)

triargle（i, j){

triargle(i + 1, j);//向左下相邻

triargle(i + 1, j + 1);//向右下相邻

}

**思路二：动态方程**



**解法一：DFS**

~~~Java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
       if(triangle.isEmpty()){
           return 0;
       }
       return DFS(triangle, 0, 0);



    }
    //终止条件，i大于等于层数，j 小于等于 每一行的列数
    public int DFS(List<List<Integer>> triangle, int i, int j){
        if( i >= triangle.size() || j >= triangle.get(i).size()){
            return 0;
        }

        //分治
        int left = DFS(triangle, i + 1, j);
        int right = DFS(triangle, i + 1, j + 1);

        //返回结果 左边和右边的最小值 + 当前值
        return Math.min(left, right) + triangle.get(i).get(j);
    }
}
~~~



****解法二：动态方程**

动态方程

**自底向上的实现**，即最后一组数字应该作为初始值，反推

定义状态：使用一个二维数组，记录当前每个节点的路径和 int/[]/[] dp

定义状态方程：**dp/[i]/[j] = Min(dp/[i + 1]/[j] ,  dp/[i + 1]/[j + 1] ) + trigle/[i]/[j]**

​					 Min(当前节点左下相邻节点 ,  当前节点右下相邻节点 ) + 当前本身节点

时间复杂度O(N^2)

~~~Java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {


        //获取最后一行作为初始值,注意size要减一
        List<Integer> lastList = triangle.get(triangle.size() - 1);
        //定义状态
        int[][] dp = new int[triangle.size()][lastList.size()];
        //定义状态初始值
        for(int i = 0; i < lastList.size(); i++){

            dp[triangle.size() - 1][i] = lastList.get(i);

        }

        //从底部开始遍历
        for(int i = triangle.size() - 2; i >= 0; i--){
            for(int j = triangle.get(i).size() - 1; j >= 0; j--){
                //动态方程
                // Min(当前节点左下相邻节点 ,  当前节点右下相邻节点 ) + 当前本身节点
                dp[i][j] = Math.min(dp[i + 1][j], dp[i + 1][ j + 1]) + triangle.get(i).get(j);
            }
        }
        return dp[0][0];
    }
}
~~~



## [152. 乘积最大子序列](https://leetcode-cn.com/problems/maximum-product-subarray/)

**题目**

~~~html
给定一个整数数组 nums ，找出一个序列中乘积最大的连续子序列（该序列至少包含一个数）。

示例 1:

输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
示例 2:

输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-product-
K著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
~~~



**思路一**

状态的定义、状态方程

~~~HTML
DP【i】下标为0的位置到 I 的位置， 表示走到第i个时的最大值，包含当前的i的最大值，不包括前面的全部，可能是<Strong>0-i的部分，只是一定包含i。</Strong>dp【n - 1】， dp【n - 2】 。。。dp【0】

Dp【i + 1】 = dp【i】 * a【i + 1】（有正负，所以定义错误）

​                          Max   *     正

​                          min   *     负

改写：

DP【i】【2】（0Max,1min），二维数组保存，Dp【i】表示0 ---- i 当前之间的最大值或负的最大值，第二维表示正负，负的只是中间过程产物，最终的结果还是最大值

dp【i】【0】 = 

​			if（a【i】>= 0): dp【i - 1】【0】 * a【i】

​			else: dp【i - 1】【0】 * a【 i】

dp【i】【0】 = 

​			if（a【i】>= 0): dp【i - 1】【1】 * a【i】

​			else: dp【i - 1】【1】 * a【 i】

最大最小值还需要和自身做比较
~~~



**思路二:递归**

每次都带上最大和最小值两个状态，当递归到数组末尾，返回，返回过程中始终记录最大值



解法一：DP

~~~Java

class Solution {
    public int maxProduct(int[] nums) {
        if(nums.length == 0){
            return 0;
        }
        int[][] dp = new int[nums.length][2];//状态定义，第二维是表示当前状态是正的最大值还是负的最大值,1是负的最大值，0是正的最大值
        //初始化值
        dp[0][0] = nums[0];
        dp[0][1] = nums[0];
        //结果值
        int result = nums[0];
        
        for(int i = 1; i < nums.length; i++){
            //当前值大于0
            if(nums[i] >= 0){
                //状态方程
                dp[i][0] = Math.max(nums[i], dp[i - 1][0] * nums[i]);// 正的最大值 * 正的 = 正的最大值
                dp[i][1] = Math.min(0, dp[i -1][1] * nums[i]);//负的最大值 * 正的 = 负的最大值
            }else{
                dp[i][0] = Math.max(nums[i], dp[i - 1][1] * nums[i]);//负的最大值 * 负的 = 正的最大值
                dp[i][1] = Math.min(nums[i], dp[i -1][0] * nums[i]);//正的最大值 * 负的 = 负的最大值
            }
            //每一轮遍历都需要判断其最大值
            result = Math.max(dp[i][0], result);

        }

        return result;
    }
}
~~~



**解法二：**

每次都带上最大和最小值两个状态，当递归到数组末尾，返回，返回过程中始终记录最大值



~~~Java
    private int result  = Integer.MIN_VALUE;
    public int maxProduct(int[] nums) {
        if(nums.length == 0){
            return 0;
        }
        DFS(0, 1, 1, nums);
        return  result;
    }
    public void DFS(int level, int positive, int negative, int[] nums){
        //终止条件
        if(nums.length == level){
            return;
        }
        if(nums[level] >= 0){
            positive = positive * nums[level];//记录正的最大值
            negative = negative * nums[level];//记录最小值
            if(nums[level] > positive){
                positive = nums[level];//如果最大值比当前值小，记得交换
            }

        }else{
            int temp = positive * nums[level];//中间变量，下面要交换相乘
            positive = negative * nums[level];//负的最大值 * 负的 = 正的最大值
            negative = temp;
            if(nums[level] < negative){
                negative = nums[level];//如果最小值比当前值大，记得交换
            }
        }
        result = Math.max(result, positive);
        DFS(level + 1, positive, negative, nums);
    }
~~~



## 股票买卖系列

## [121] 买卖股票的最佳时机 （只能买卖一次）

**题目**

~~~HTML
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

 \* 如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

 \* 注意你不能在买入股票前卖出股票。



 \* 示例 1:

 \* 输入: [7,1,5,3,6,4]

 \* 输出: 5

 \* 解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。

 \* ⁠    注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。


示例 2:

 \* 输入: [7,6,4,3,1]

 \* 输出: 0

 \* 解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
~~~

**思路一：**

遍历一次，比较记录最小值，判断每一个数和最小值的相差值，如果比原来记录的大，则替换

**思路二：DP**



**解法一：**

~~~java
//遍历一次，比较记录最小值，判断每一个数和最小值的相差值，如果比原来记录的大，则替换
class Solution {
    public int maxProfit(int[] prices) {

      if(prices.length == 0){
        return 0;
      }
      int minI  = prices[0];//记录数组中的最小值
      int max = 0;//记录结果最大值
      for(int i = 1; i < prices.length; i++){
        
        if(prices[i] < minI){//如果当前的数字小于最小值，则设置为最小值

          minI = prices[i];
        }
        if(prices[i] - minI > max){//如果当前数 - 最小值 》 大于最大值，则赋值为最大值
          max = prices[i] - minI;
        }

      }

      return max;

    }
}

~~~

**解法二：****DP**



~~~Java
class Solution {
    public int maxProfit(int[] prices) {

      if(prices.length == 0){
        return 0;
      }

    //最多只能买卖一次，不能持有多分股票， 
    /**
     * 一、定义状态
     * MP[][]表示第 0 ---》i 天一共交易了 一次  获得的最大利益
     * MP[i][j]: i是当前数组的下标，j是表示当前状态手上是否持有股票 0 无股票，1 有股票  2完成一次交易
     * 
     * 二、状态方程：
     * MP[i][0] = MP[i - 1][0] 第i天，手上没有股票  = 昨天手上也没有股票
     * 
     * MP[i][1] = Max(MP[i - 1][1], MP[i - 1][0] - prices[i])  第i天，手上有股票 = 昨天手上有股票 亦或 昨天手上没有股票，今天买入今天的股票份额
     * 
     * 
     * MP[i][2] = Max( MP[i - 1][1] + prices[i], MP[i - 1][2])第i天，已经完成了一次交易（买入、卖出） 等于昨天完成一次交易  和  昨天手上有股票，并且在今天卖出
     * 
     * 
     * 三、初始值
     * MP[0][0] = 0;//第一天，并且没有股票，利润为0
       MP[0][1] = -prices[0];//第一天，手上有股票，说明买了第一个股票
       MP[0][2] = 0;//第一天，表示当天买了股票，并且卖了出去，完成一次交易
     */

     int result = 0;
     //state
     int[][] MP = new int[prices.length][3];

     //init
     MP[0][0] = 0;
     MP[0][1] = -prices[0];
     MP[0][2] = 0;
     
     //for循环遍历
     for(int i = 1; i < prices.length; i++){
      MP[i][0] = MP[i - 1][0];
 
       MP[i][1] = Math.max(MP[i - 1][1], MP[i -1][0] - prices[i]);

       MP[i][2] = Math.max( MP[i - 1][1] + prices[i], MP[i - 1][2]);

       //最后的结果肯定是手上无股票 或者是 股票完成一一次交易  的最大值
       result = Math.max(Math.max(MP[i][0], MP[i][2]), result);
     }
     return result;

    }


}
~~~



## 122 买卖股票的最佳时机  买卖无数次

**题目**

~~~HTML
 给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
 * 设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。
 * 注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
 * 示例 1:
 * 输入: [7,1,5,3,6,4]
 * 输出: 7
 * 解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
 * 随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。

 * 示例 2:
 * 输入: [1,2,3,4,5]
 * 输出: 4
 * 解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4
 * 。
 * 注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
 * 因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。

 * 示例 3:
 * 输入: [7,6,4,3,1]
 * 输出: 0
 * 解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
~~~



**解题思路**

思路一：贪心算法



**解法**

**解法一：贪心算法**

~~~Java
class Solution {
    /**
     * 贪心解法
     * 因为可以无限交易，所以只要发现明天的比今天的大，就可以抛出
     * @param prices
     * @return
     */
    public int maxProfit(int[] prices) {
        if(prices.length == 0){
            return 0;
          }
        int result = 0;
        
        for(int i = 0; i < prices.length - 1; i++){
            if(prices[i] < prices[ i+ 1]){
                result += prices[i + 1] - prices[i];
            }
        }
        return result;
       
        
    }
}
~~~



## 123 买卖股票的最佳时机 买卖2次

**题目**

~~~html 
给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。
 * 
 * 设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。
 * 
 * 注意: 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
 * 
 * 示例 1:
 * 
 * 输入: [3,3,5,0,0,3,1,4]
 * 输出: 6
 * 解释: 在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
 * 随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。
 * 
 * 示例 2:
 * 
 * 输入: [1,2,3,4,5]
 * 输出: 4
 * 解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4
 * 。   
 * 注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。   
 * 因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
 * 
 * 
 * 示例 3:
 * 
 * 输入: [7,6,4,3,1] 
 * 输出: 0 
 * 解释: 在这个情况下, 没有交易完成, 所以最大利润为 0。
~~~



**解题思路**

思路一：DP动态规划

1，定义状态

//MP【i】【k】【j】 k表示交易次数， j表示当前的手上的股票状态(0无股票，1有股票，2完成一次交易)

  int[][]【i】【k】【j】 MP = new int[【prices.length】【3】【3】；

2，状态方程

 MP【i】【k】【j】= MP【i - 1】【k】【j】；

3，初始值

~~~java 
MP[0][0][0] = 0;
MP[0][0][1] = -prices[0];
MP[0][1][0] =0;//一次交易
MP[0][1][1] = -prices[0];
MP[0][2][0] = 0;//第二次交易，第一天的第二次交易
~~~



4，最优解





**解法一：**

~~~Java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 0){
            return 0;
        }
        int result = 0;
         //DP state
         //MP【i】【k】【j】 k表示交易次数， j表示当前的手上的股票状态(0无股票，1有股票，2完成一次交易)
         int[][][] MP = new int[prices.length][3][3]; 
         //初始值
         MP[0][0][0] = 0;
         MP[0][0][1] = -prices[0];
         MP[0][1][0] =0;//一次交易
         MP[0][1][1] = -prices[0];
         MP[0][2][0] = 0;//第二次交易，第一天的第二次交易

         for(int i = 1; i < prices.length; i++){
             MP[i][0][0] = MP[i - 1][0][0];//手上无股票，昨天手上也无股票
             MP[i][0][1] = Math.max(MP[i - 1][0][1], MP[i -1][0][0] - prices[i]);//之前有股票 或 昨天无股票，今天买入            
             MP[i][1][0] = Math.max(MP[i - 1][1][0], MP[i - 1][0][1] + prices[i]);//之前已经完成了一次交易，并且无股票  或 之前没有完成一次交易，手上有股票，这i天卖出
             MP[i][1][1] = Math.max(MP[i - 1][1][1], MP[i -1][1][0] - prices[i]);//之前已经完成了一次交易并且手上有股票 = 之前完成了一次交易并且手上有股票 或 之前完成了一份交易，当前手上无股票，现在买入
             MP[i][2][0] = Math.max(MP[i -1][2][0],MP[i -1][1][1] + prices[i]);//第二完成交易 = 之前一次手上有股票并卖出 + 之前一次已经完成了两次交易
             
             result = Math.max(Math.max(Math.max(Math.max(MP[i][0][0], MP[i][0][1]), Math.max(MP[i][1][0], MP[i][1][1])), MP[i][2][0]), result); 
            } 
         return result;  
    }
}
~~~





## 188 【买卖股票最佳时机 买卖k次】



~~~HTML
 * 给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。
 * 
 * 设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。
 * 
 * 注意: 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
 * 
 * 示例 1:
 * 
 * 输入: [2,4,1], k = 2
 * 输出: 2
 * 解释: 在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
 * 
 * 
 * 示例 2:
 * 
 * 输入: [3,2,6,5,0,3], k = 2
 * 输出: 7
 * 解释: 在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4
 * 。
 * 随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。
 * 
~~~





**解法：动态规划**

~~~Java
    /**
     * 一、定义状态
     * MP[i][j][k]:表示第 0 ---》i 天一共交易了 k  获得的最大利益
     * : i是当前数组的下标，j是表示当前状态手上是否持有股票 0 无股票，1 有股票  
     * 
     * 二、状态方程：
     *
     mp[i][0][kk] = mp[i - 1][0][kk];//kk为0 j为0，即交易0次 手上无股票 等于 之前也交易0次并且手上无股票
     
     mp[i][0][kk] = Math.max(mp[i - 1][0][kk], mp[ i - 1][1][kk - 1] + prices[i]);//kk不为0，即有交易，手上没股票 = 手上之前有股票，在这一天卖出 或 之前手上本来就没股票
     
      mp[i][1][kk] = Math.max(mp[i - 1][1][kk], mp[i - 1][0][kk] - prices[i]);//手上有股票 = 手上之前没有股票，并买入股票 或 之前手上有股票
     * 
     * 三、初始值
     //inint，设置初始值
        for(int kk = 0; kk <= k; kk++){
            mp[0][0][kk] = 0;//第一天 买入-卖出 循环K次之后不再买入，初始值为0
            mp[0][1][kk] = -prices[0];//第一天 买入-卖出 循环k次之后又买入，所以初始值为 -prices[0]
        }
     */

class Solution {
    public int maxProfit(int k, int[] prices) {
        if (prices == null || prices.length == 0)
            return 0;
        
        int result = 0;

        if(k > prices.length / 2){
            return greedy(prices);
        }
        //state
        int[][][] mp = new int[prices.length][3][k + 1];
        //inint
        for(int kk = 0; kk <= k; kk++){
            mp[0][0][kk] = 0;//第一天 买入-卖出 循环K次之后不再买入，初始值为0
            mp[0][1][kk] = -prices[0];//第一天 买入-卖出 循环k次之后又买入，所以初始值为 -prices[0]
        }
        for(int i = 1; i < prices.length; i++){
            for(int kk = 0; kk <= k; kk++){
                if(kk == 0){
                    mp[i][0][kk] = mp[i - 1][0][kk];//kk为0，即交易0次，所以等于 之前也交易0次
                }else{
                    mp[i][0][kk] = Math.max(mp[i - 1][0][kk], mp[ i - 1][1][kk - 1] + prices[i]);//kk不为0，即有交易，手上没股票 = 手上之前有股票，在这一天卖出 或 之前手上本来就没股票
                }

                mp[i][1][kk] = Math.max(mp[i - 1][1][kk], mp[i - 1][0][kk] - prices[i]);//手上有股票 = 手上之前没有股票，并买入股票 或 之前手上有股票

                result = Math.max(Math.max(mp[i][0][kk], mp[i][1][kk]), result);

            }
            
        }

        return result;
    }
    
    
    public int greedy(int[] prices){
        int reuslt = 0;
        for(int i = 0; i < prices.length - 1; i++){
            if(prices[i] < prices[i + 1]){
                reuslt += prices[i + 1] - prices[i];
            }
        }
        return reuslt;

    }
        
}
~~~



## 300 最长上升子序列

**题目**

~~~HTML
 * 给定一个无序的整数数组，找到其中最长上升子序列的长度。
 * 
 * 示例:
 * 
 * 输入: [10,9,2,5,3,7,101,18]
 * 输出: 4 
 * 解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
 * 
 * 说明:
 * 
 * 
 * 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
 * 你算法的时间复杂度应该为 O(n^2) 。
 * 
 * 
 * 进阶: 你能将算法的时间复杂度降低到 O(n log n) 吗?
~~~



**思路**

思路一：DFS + 剪枝

思路二：动态规划

dp[i]表示以**i**为结尾的最长上升序列的长度 ，要和**dp[j]+1**进行比较，**j在0-（i-1）的范围**

**解法**

**解法二**：

动态规划

1，定义状态

dp【i】表示当前数组下标为i的最长上升子序列

2，动态方程

dp[i] = dp[j] + 1;

j的范围：0 ----->i - 1, 并且dp[j] < dp[i]

3，最优解

最优解并不是最后一个n - 1，而是0 ----> n之间的最大值

![img](https://img-blog.csdnimg.cn/20190605210215350.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l5c2F2ZQ==,size_16,color_FFFFFF,t_70)

~~~java
class Solution {
    public int lengthOfLIS(int[] nums) {
       if(nums.length == 0){
           return 0;
       }
        
        //定义状态
        int[] dp = new int[nums.length];

        //初始值,第一位的长度为1
        dp[0] = 1;

        int result = dp[0];

        //状态方程
        for(int i = 1; i < nums.length; i++){
            //当前这个数的长度为1
            dp[i] = 1;
            //从0----> i - 1 判断在i之前的最大子序列
            for(int j = 0; j < i; j++){
                //dp[j] + 1 是因为要加上当前的自己本身这个数的长度，所以加1
                //如果当前的值大于当前之前的值，并且 dp【i】<dp[j] + 1
                if(nums[j] < nums[i] && dp[i] < dp[j] + 1){
                    dp[i] = dp[j] + 1;
                }
                //result存储每一次的结果值，如果当前的dp[i] > result ,则把当前值赋值给result
                result = Math.max(dp[i], result);
            }
            
        }
        return result;
        
    }

}
~~~



## 322 零钱兑换

题目

~~~HTML
 * 给定不同面额的硬币 coins 和一个总金额
 * amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。
 * 
 * 示例 1:
 * 
 * 输入: coins = [1, 2, 5], amount = 11
 * 输出: 3 
 * 解释: 11 = 5 + 5 + 1
 * 
 * 示例 2:
 * 
 * 输入: coins = [2], amount = 3
 * 输出: -1
 * 
 * 说明:
 * 你可以认为每种硬币的数量是无限的。
~~~



解题思路

思路一：暴力求解

思路二：**动态规划**

转变思路，转变成爬楼梯问题，**硬币的的面值**就变成能**爬楼梯的步数**，要**拼凑的金额**变成**一共多少级阶梯**

注意：**Integer.MAX_VALUE + 1会溢出，变成最小值**

**解法**



**解法二：**

1，定义状态

 //定义状态dp【i】上到i阶的最小值
        **int[] dp = new int[amount + 1];**

2，状态方程

if（i >= coins[i]）:

**dp[i] = Math.min(dp[i - coins[j]] + 1, dp[i]);**

3,最优解

 //如果最后一个和Integer.MAX_VALUE - 1相等，说明无解
        return **dp[amount] == Integer.MAX_VALUE - 1 ? -1 : dp[amount];**

~~~Java
/**
 * 装换成爬楼梯问题
 * amount就是需要爬的n阶楼梯
 * coins就是可以行走的步数
 */
class Solution {
    public int coinChange(int[] coins, int amount) {
        if(coins.length == 0){
            return -1;
        }
        //定义状态dp【i】上到i阶的最小值
        int[] dp = new int[amount + 1];
        //数组默认初始值为0
        //状态方程
        for(int i = 1; i <= amount; i++){
            //Integer.MAX_VALUE + 1会溢出，变成最小值
            dp[i] = Integer.MAX_VALUE - 1;
            for(int j = 0; j < coins.length; j++){
                //当i大于等于coins的面值时才可以进行运行
                if(i >= coins[j]){
                    //记录当前的最小值
                    dp[i] = Math.min(dp[i - coins[j]] + 1, dp[i]);
                }else{

                }
                
            }
        }
        //如果最后一个和Integer.MAX_VALUE - 1相等，说明无解
        return dp[amount] == Integer.MAX_VALUE - 1 ? -1 : dp[amount];

        
    }
}
~~~



## 72 编辑距离

 **\* 字符串匹配 动态规划解法的入门题**

 **\* 经典题，常考题**

题目

~~~html
 * 给定两个单词 word1 和 word2，计算出将 word1 转换成 word2 所使用的最少操作数 。
 * 
 * 你可以对一个单词进行如下三种操作：
 * 
 * 
 * 插入一个字符
 * 删除一个字符
 * 替换一个字符
 * 
 * 
 * 示例 1:
 * 
 * 输入: word1 = "horse", word2 = "ros"
 * 输出: 3
 * 解释: 
 * horse -> rorse (将 'h' 替换为 'r')
 * rorse -> rose (删除 'r')
 * rose -> ros (删除 'e')
 * 
 * 
 * 示例 2:
 * 
 * 输入: word1 = "intention", word2 = "execution"
 * 输出: 5
 * 解释: 
 * intention -> inention (删除 't')
 * inention -> enention (将 'i' 替换为 'e')
 * enention -> exention (将 'n' 替换为 'x')
 * exention -> exection (将 'n' 替换为 'c')
 * exection -> execution (插入 'u')
~~~



解题思路

思路一：DFS

思路二：BFS

思路三：DP动态规划



**解法**

**解法三**

我们定义函数 **f(i, j)**表示表示**第一个字符串的第i个位置**与**第二个字符串的第j个位置**转换的需要的**最少步骤**

 ~~~html
 1、定义状态

​    dp[i][j] 表示第一个字符串的第i个位置与第二个字符串的第j个位置操作几次到达

  2、状态方程

​    如果w1(i) == w2(j) 则不用处理，即等于上一次的操作数 dp[i][j] = dp[i - 1][j - 1]

​    

​    不相等就有三种情况

​    插入：w1中的i的位置不变，而w2中的j要向前一个位置，即f(i, j) = f(i, j - 1) + 1。

​    删除：删除的话，w1中会减少一个需要比较的元素，而w2中的j的位置不变f(i, j) = f(i - 1, j) + 1

​    

​    替换：replace会减少word1和word2中一个需要比较的元素（i和j会向前挪一个位置），也就是f(i,j)=f(i−1,j−1)+1 

  3、最优解

​    就是最后的dp[m][n]
 ~~~



~~~Java
 /**
  * 动态规划：
  * 
  1、定义状态
    dp[i][j] 表示第一个字符串的第i个位置与第二个字符串的第j个位置操作几次到达

  2、状态方程
    如果w1(i) == w2(j) 则不用处理，即等于上一次的操作数 dp[i][j] = dp[i - 1][j - 1]
    
    不相等就有三种情况

    插入：w1中的i的位置不变，而w2中的j要向前一个位置，即f(i, j) = f(i, j - 1) + 1。

    删除：删除的话，w1中会减少一个需要比较的元素，而w2中的j的位置不变f(i, j) = f(i - 1, j) + 1
    
    替换：replace会减少word1和word2中一个需要比较的元素（i和j会向前挪一个位置），也就是f(i,j)=f(i−1,j−1)+1 

  3、最优解
    就是最后的dp[m][n]
  */
class Solution {
    public int minDistance(String word1, String word2) {
        int w1Length = word1.length();
        int w2Length = word2.length();
        //当有一个字符串为空时
        if(w1Length * w2Length == 0){
            return w1Length > w2Length ? w1Length : w2Length;
        }
        //定义状态
        int[][] dp = new int[w1Length + 1][w2Length + 1];
        //初始值
        for(int i = 1; i <= w1Length; i++){
            //初始化,j为0，则要进行i次的操作
            dp[i][0] = i;
            for(int j = 1; j <= w2Length; j++){
                //初始化,i为0，则要进行j次的操作
                dp[0][j] = j;
                if(word1.charAt(i - 1) == word2.charAt(j - 1)){
                    dp[i][j] = dp[i - 1][j -1];
                }else{
                    dp[i][j] = Math.min(Math.min(dp[i][j - 1], dp[i -1][j]), dp[i - 1][j - 1]) + 1;
                }
            }
        }
        return dp[w1Length][w2Length];
        
    }
}
~~~


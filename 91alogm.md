asaa

#### 1、leetcode 144题：给你二叉树的根节点 root ，返回它节点值的 前序 遍历

输入：root = [1,null,2,3]
输出：[1,2,3]
code:

```
class Solution {
    List<Integer>  ans = new  ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        if(root == null){
            return ans;
        }
        ans.add(root.val);
        preorderTraversal(root.left);
        preorderTraversal(root.right);
        return ans
    }
}
```

#### 2、leetcode 94题 二叉树的中序遍历

输入：root = [1,null,2,3]
输出：[1,2,3]

```
递归版本
class Solution {
    List<Integer> ans = new ArrayList<>();
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root == null)
            return ans;
        inorderTraversal(root.left);
        ans.add(root.val);
        inorderTraversal(root.right);
        return ans;
    }
}
```

```
  class Solution {
    List<Integer>  ans = new  ArrayList<>();
    public List<Integer> inorderTraversal(TreeNode root) {
        if (root != null) {
            Stack<TreeNode> stack = new Stack<>();
            while (!stack.isEmpty() || root != null) {
                if (root != null) {
                    stack.push(root);
                    root = root.left;
                } else {
                    root = stack.pop();
                    ans.add(root.val);
                    root = root.right;
                }
            }
        }
        return ans;
    }
}
```



#### 3、leetcode 94题 二叉树的中序遍历

```
迭代
class Solution {
    List<Integer> ans = new ArrayList<>();
    public List<Integer> postorderTraversal(TreeNode root) {
        if(root == null)
            return ans;
        postorderTraversal(root.left);
        postorderTraversal(root.right);
        ans.add(root.val);
        return ans;
    }
}
```



```
非递归版本
class Solution {
    List<Integer> ans = new ArrayList<>();
    Stack<Integer> mystack = new Stack<>();
    public List<Integer> postorderTraversal(TreeNode root) {
        if(root == null)
            return ans;
        Stack<TreeNode> stack =  new Stack<>();
        stack.push(root);
        while(!stack.empty()){
            root =stack.pop();
            mystack.push(root.val);
            if(root.left != null) {
                stack.push(root.left);
            }
            if(root.right != null) {
                stack.push(root.right);
            }
        }
        while(!mystack.empty()){
            ans.add(mystack.pop());
        }
        return ans;
    }
}
```

#### 4、leetcode 102题 二叉树的层序遍历

```
递归版本
class Solution {
    public void addLevel(List<List<Integer>> list, int level, TreeNode root) {
        if(root == null)
           return;
        if(list.size()-1 < level)
           list.add(new ArrayList<>());
        list.get(level).add(root.val );
        addLevel(list, level+1 , root.left);
        addLevel(list, level+1 , root.right);
    }
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans  =new ArrayList<>();
        addLevel(ans, 0 ,root);
        return ans;
    }
}
```



```
非递归
public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans  =new ArrayList<>();
        if(root == null)   return ans;
        Queue<TreeNode> que = new LinkedList<>();
        que.add(root);
        int size = 0;
        int level =0;
        TreeNode node = null;
        while(!que.isEmpty()){
            size = que.size();
            ans.add(new ArrayList<>());
            for(int i=0;i<size;i++){
                node = que.poll();
                ans.get(level).add(node.val);
                if(node.left!= null) que.add(node.left);
                if(node.right!= null) que.add(node.right);
            }
            level++;
        }
        return ans;
}
```



#### 5、排序算法

##### 5.1冒泡排序：复杂度 O（N^2）稳定

```
 public static  void myBubbleSort(int []arr){
        for(int i= arr.length -1;i>0 ;i--){
            for (int j = 0; j < i; j++) {
                if(arr[j] > arr[j+1])
                    swap(arr,j,j+1);
            }
        }
    }
```



5.2选择排序：复杂度 O（N^2）不稳定；数据  5 8 5 2 9 



```
public static void selectionSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}
		for (int i = 0; i < arr.length - 1; i++) {
			int minIndex = i;
			for (int j = i + 1; j < arr.length; j++) {
				minIndex = arr[j] < arr[minIndex] ? j : minIndex;
			}
			swap(arr, i, minIndex);
		}
}


```



##### 5.3 堆排序：复杂度 O（NlogN）不稳定；



```
public static void heapSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}
		for (int i = 0; i < arr.length; i++) {
			heapInsert(arr, i);
		}
		int size = arr.length;
		swap(arr, 0, --size);
		while (size > 0) {
			heapify(arr, 0, size);
			swap(arr, 0, --size);
		}
	}

public static void heapInsert(int[] arr, int index) {
		while (arr[index] > arr[(index - 1) / 2]) {
			swap(arr, index, (index - 1) / 2);
			index = (index - 1) / 2;
		}
}
	
public static void heapify(int[] arr, int index, int size) {
		int left = index * 2 + 1;
		while (left < size) {
			int largest = left + 1 < size && arr[left + 1] > arr[left] ? left + 1 : left;
			largest = arr[largest] > arr[index] ? largest : index;
			if (largest == index) {
				break;
			}
			swap(arr, largest, index);
			index = largest;
			left = index * 2 + 1;
		}
}


```







##### 5.4快速排序：复杂度 O（NlogN）不稳定（可以稳定，01stable sort ）；

```
public static void quickSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}
		quickSort(arr, 0, arr.length - 1);
	}

	public static void quickSort(int[] arr, int l, int r) {
		if (l < r) {
			swap(arr, l + (int) (Math.random() * (r - l + 1)), r);
			int[] p = partition(arr, l, r);
			quickSort(arr, l, p[0] - 1);
			quickSort(arr, p[1] + 1, r);
		}
	}
	
	public static int[] partition(int[] arr, int l, int r) {
		int less = l - 1;
		int more = r;
		while (l < more) {
			if (arr[l] < arr[r]) {
				swap(arr, ++less, l++);
			} else if (arr[l] > arr[r]) {
				swap(arr, --more, l);
			} else {
				l++;
			}
		}
		swap(arr, more, r);
		return new int[] { less + 1, more };
	}


```



##### 5.4.1 LeetCode 75 颜色分类问题。类似于荷兰国旗问题

```
class Solution {
  public void swap(int[] nums,int i,int j){
    int temp = nums[i];
    nums[i]= nums[j];
    nums[j] = temp;
  }
  public void sortColors(int[] nums) {
     int less = -1, more = nums.length;
     int i= 0;
     while(i < more){
       if(nums[i]==0) {
         swap(nums, ++less, i++);
       }else if(nums[i] == 2){
         swap(nums, --more,i);
       }else if(nums[i] == 1){
         i++;
       }
     }
  }
}
```



##### 5.5归并排序：复杂度 O（NlogN）稳定

```
public static void mergeSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}
		mergeSort(arr, 0, arr.length - 1);
	}

	public static void mergeSort(int[] arr, int l, int r) {
		if (l == r) {
			return;
		}
		int mid = l + ((r - l) >> 1);
		mergeSort(arr, l, mid);
		mergeSort(arr, mid + 1, r);
		merge(arr, l, mid, r);
	}
	
	public static void merge(int[] arr, int l, int m, int r) {
		int[] help = new int[r - l + 1];
		int i = 0;
		int p1 = l;
		int p2 = m + 1;
		while (p1 <= m && p2 <= r) {
			help[i++] = arr[p1] < arr[p2] ? arr[p1++] : arr[p2++];
		}
		while (p1 <= m) {
			help[i++] = arr[p1++];
		}
		while (p2 <= r) {
			help[i++] = arr[p2++];
		}
		for (i = 0; i < help.length; i++) {
			arr[l + i] = help[i];
		}
	}


```



```
非递归
class Solution {
    List<Integer>  ans = new  ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        if(root == null){
            return ans;
        }
        Stack<TreeNode> stack =  new Stack<>();
        stack.push(root);
        //TreeNode node;
        while(!stack.empty()){
            root =stack.pop();
            ans.add(root.val);
            if(root.right != null) {
                stack.push(root.right);
            }
            if(root.left != null) {
                stack.push(root.left);
            }
        }
        return ans;
    }
}
```



#### 6、leetcode 剑指Offer 41数据流的中位数

```

class MedianFinder {

    /** initialize your data structure here. */
    PriorityQueue<Integer> maxHeap;
    PriorityQueue<Integer> minHeap;
    public MedianFinder() {
         maxHeap = new PriorityQueue<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2.compareTo(o1);
            }
        });
    
        minHeap = new PriorityQueue<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1.compareTo(o2);
            }
        });
    }
    
    public void addNum(int num) {
        if(maxHeap.isEmpty())   {
            maxHeap.add(num);
            return;
        }
        if(num > maxHeap.peek())
            minHeap.add(num);
        else
            maxHeap.add(num);
        while(   maxHeap.size() - minHeap.size()  >1){
            minHeap.add(maxHeap.poll());
        }
        while(  minHeap.size() - maxHeap.size()  >= 1){
            maxHeap.add(minHeap.poll());
        }
    }
    
    public double findMedian() {
        double ans =0;
        int size = maxHeap.size() + minHeap.size();
        ans = (size%2 ==0)? (maxHeap.peek() + minHeap.peek())/2.0f: maxHeap.peek();
        return ans;
    }
}

```

#### 12月10号

#### 7、桶排序

假设数据只在（0-200之间，数据波动比较小）比如年龄等。开辟一个大小为200的数组，去进行计数。最后将数组的每个元素的值恢复成排序。





	public static void bucketSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}
		int max = Integer.MIN_VALUE;
		//找到最大的值
		for (int i = 0; i < arr.length; i++) {
			max = Math.max(max, arr[i]);
		}
		int[] bucket = new int[max + 1];
		for (int i = 0; i < arr.length; i++) {
			bucket[arr[i]]++;
		}
		int i = 0;
		//将计数后的数组恢复回去
		for (int j = 0; j < bucket.length; j++) {
			while (bucket[j]-- > 0) {
				arr[i++] = j;
			}
		}
	}
##### 7.1 桶排序相关的题  leetcode 164 最大间距 有bug

思路：有n个数，遍历获得最大最小值，申请n+1个桶，将每个数放进桶中。维护每个桶的最大最小值遍历一次。如果是访问第一个

获得答案

```
有点Bug需要调试一下
public static int maximumGap(int[] nums) {
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        for(int i=0;i<nums.length;i++){
            max = nums[i]>max ? nums[i]:max;
            min = nums[i]<min ? nums[i]:min;
        }
        int []bucket = new int[nums.length+1];
        int []buMin = new int[nums.length+1];
        int []buMax = new int[nums.length+1];
        boolean []buFlag = new boolean[nums.length+1];
        double dis = (max - min)*1.0f / nums.length;
        int id;
        for(int i =0;i<nums.length;i++){
            id = (int)Math.round( (nums[i] - min) / dis);
            if(buFlag[id] == false){
                buMin[id] = buMax[id] = nums[i];
                buFlag[id] = true ;
            }else{
                buMin[id] = nums[i]<buMin[id] ? nums[i]:buMin[id];
                buMax[id] = nums[i]>buMin[id] ? nums[i]:buMax[id];
            }
        }
        int ans =Integer.MIN_VALUE;
        for(int i=0;i<bucket.length;){
            int j=i;
            while(++j < bucket.length &&  buFlag[j] == false);
            if(j >= bucket.length)
                return ans;
            if(j!= i+1 ){
                ans = buMin[j]- buMax[i]> ans ?buMin[j]- buMax[i] : ans;
                i =j;
            }else{
                ans = buMin[j]- buMax[i]> ans ?buMin[j]- buMax[i] : ans;
                i++;
            }
        }
        return ans;
    }
```

 

#### 8 链表相交的一系列问题

##### 8.1 leetcode 剑指Offer 52 两个链表相交的第一个公共节点

```
思路挺简单，没啥说的
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int len1 = 0, len2 = 0;
        ListNode node = headA;
        while(node!=null){
            len1++;
            node=node.next;
        }
        node = headB;
        while(node!=null){
            len2++;
            node=node.next;
        }
        

        ListNode node1 = len1 > len2 ?  headA : headB;
        ListNode node2 = len1 > len2 ?  headB : headA;
        int len3 = Math.abs(len1-len2);
        while(len3-- >0){
            node1=node1.next;
        }
        while(node1 != node2){
            node1= node1.next;
            node2= node2.next;
        }
        return node1;
    }

}
```



##### 8.2 leetcode 141  如何判断一个链表是否有环 



```

function detectCycle(head) {
    let fast = head;
    let slow = head; 

    while (fast && fast.next) {
        fast = fast.next.next;
        slow = slow.next;

        if (fast == slow) {
            // 其中一个指针指向不动，另一个指针指向头
            slow = head;
            while (fast !== slow) {
                // 同时只移动一步
                slow = slow.next;
                fast = fast.next;
            }
            // 此时再次相遇，指向的那个节点就是入环节点
            return slow;
        }
    }

    return null;
}
```



#####  8.3如何判断两个有环链表是否相交, 相交则返回第一个相交节点, 不相交则返回null. 

有时间回过头来写，这道题有点麻烦

#### 9、栈队列问题

##### 9.1两个栈实现一个队列

```
Stack<Integer> stack1;
    Stack<Integer> stack2;
    public CQueue() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }
    public void appendTail(int value) {
        stack1.push(value);
    }
    public int deleteHead() {
        if(stack1.isEmpty())
            return -1;
        while(!stack1.isEmpty()){
            stack2.push(stack1.pop());
        }
        int ans = stack2.pop();
        while(!stack2.isEmpty()){
            stack1.push(stack2.pop());
        }
        return ans;
    }
```



##### 9.2两个队列实现一个栈



```
/** Initialize your data structure here. */
    Queue<Integer> queue1;
    Queue<Integer> queue2;
    int size =0;
    public MyStack() {
        queue1 =  new LinkedList<Integer>() ;
        queue2 =  new LinkedList<Integer>() ;
        

    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        queue1.add(x);
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        if(queue1.size() ==0)
            return -1;
        while(queue1.size() > 1){
            queue2.add(queue1.poll());
        }
        int ans = queue1.poll();
        while(!queue2.isEmpty()){
            queue1.add(queue2.poll());
        }
        return ans;
    }
    
    /** Get the top element. */
    public int top() {
        if(queue1.size() ==0)
            return -1;
        while(queue1.size() > 1){
            queue2.add(queue1.poll());
        }
        int ans = queue1.peek();
        queue2.add(queue1.poll());
        while(!queue2.isEmpty()){
            queue1.add(queue2.poll());
        }
        return ans;
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return queue1.isEmpty();
    }


```

##### 9.3 用数组实现栈 leetcode622



```
数组实现队列
    int []arr = null;
    int curId =0 ,endId =0, curSize =0,size = 0;

    public MyCircularQueue(int k) {
        arr = new int[k];
        size =k;
    }
    
    public boolean enQueue(int value) {
        if(curSize < size){
            arr[curId% size] = value;
            curSize++;
            curId++;
            return true;
        }
        return false;
    }
    
    public boolean deQueue() {
        if(curSize<=0) return  false;
        endId ++;
        endId = endId % size;
        curSize--;
        return true;
    }
    
    public int Front() {
        if(curSize<=0) return -1;
        return arr[(endId) % size];
    }
    
    public int Rear() {
        if(curSize<=0) return -1;
        return arr[(curId-1) % size];
    
    }
    
    public boolean isEmpty() {
        return curSize==0;
    }
    
    public boolean isFull() {
        return curSize==size;
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return queue1.isEmpty();
    }




```



#### 10 设计一个getMin()函数的栈



    /** initialize your data structure here. */
        Stack<Integer> stack1 = new Stack<>();
        Stack<Integer> minStack = new Stack<>();
    
    public MinStack() {
    }
    
    public void push(int x) {
        if (stack1.isEmpty()) {
            minStack.push(x);
            stack1.push(x);
            return;
        }
    
        stack1.push(x);
        if (x < minStack.peek())
            minStack.push(x);
        else
            minStack.push(minStack.peek());
    }
    
    public void pop() {
        stack1.pop();
        minStack.pop();
    }
    
    public int top() {
        return stack1.peek();
    }
    
    public int min() {
        return minStack.peek();
    }
#### 12-11号

#### 11、N*N旋转矩阵问题  面试题 01 07

#### 

```
class Solution {
    public void myswap(int [][]arr ,int tR ,int tC,int bR ,int bC ){
        int tmp = 0;
        for(int i=0;i < bC -tC ;i++ ){
            tmp = arr[bC-i][tC];
            //System.out.print ( "tmp = " + tmp  );
            arr[bC-i][tC] = arr[bR][bC-i];
            //System.out.print ( "tmp = " + arr[bC-i][tC]  );
            arr[bR][bC-i] = arr[tR+i][bC];
            //System.out.print ( "tmp = " + arr[bR][bC-i]   );
            arr[tR+i][bC] =arr[tR][ tC+i];
            //System.out.print ( "tmp = " + arr[tR+i][bC]   );
            arr[tR][ tC+i] = tmp;
           // System.out.println ( "tmp = " + arr[tR][ tC+i]   );
        }
    }
    public void rotate(int[][] matrix) {
        int N = matrix.length;
        for(int i=0;i< N ;i++){
            if(i<= N-i-1)
                myswap(matrix , i ,  i, N-i-1 ,N-i-1 );
        }
    }
}
```

#### 11、螺旋打印矩阵问题    有bug



```
public void myPrint(List<Integer> ans ,int [][] arr, int tR, int tC , int bR, int bC){
        for(int i = tC ; i< bC ;i++){
            ans.add(arr[tR][i]);
            System.out.print(arr[tR][i]);
        }
        System.out.println();
        for(int i = tR ; i< bR ;i++){
            ans.add(arr[i][bC]);
            System.out.print(arr[i][bC]);
        }System.out.println();
        for(int i = bC ; i>tC ;i--){
            ans.add(arr[bR][i]);
            System.out.print(arr[bR][i]);
        }System.out.println();
        for(int i = bR ; i>tR ;i--){
            ans.add(arr[i][tC]);
            System.out.print(arr[i][tC]);
        }System.out.println();

    }
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> ans = new ArrayList<>();
        int Row = matrix.length , Col = matrix[0].length;
        int min = Math.min(Row, Col);
        for(int i =0 ; i < min ;i++){
            myPrint(ans , matrix,i,i, Row -i -1,Col-1-i);
        }
        return ans;
    }


```



#### 12 leetcode206 反转链表

##### （1）普通情形，对所有链表反转操作

```
public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null )
            return head;
        ListNode pNext = head.next;
        ListNode pPre = null , pCur = head;
        while(pCur!=null){
            pNext = pCur.next;
            pCur.next=pPre;
            pPre= pCur;
            pCur = pNext;
        }
        return pPre;
    }
```



##### （2）进阶，leetcode 92题 对m,n之间的链表进行反转操作

增加哑结点，代码看起来很爽 c++ 版本的，代码写的真牛逼



```
public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode dummy = new ListNode(-1);
        dummy.next= head;
        ListNode node = dummy;
        for(int i=1;i<m;i++)
            node= node.next;
        ListNode tail=node, first = node.next; //正常链表的最后一个点，反转链表的第一个点
        ListNode pre= node.next;
        ListNode cur= node.next.next;

        for(int i=m;i<n;i++){
            ListNode next= cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        first.next =cur;
        tail.next = pre;
        return dummy.next;
    }
```

##### （3）再进阶，leetcode 25题 每K个节点之间的链表进行反转操作

	public ListNode reverseKGroup(ListNode head, int k) {
	    if(head == null) return head;
	    int len = 0;
	    ListNode dummy = new ListNode(-1);
	    dummy.next=head;
	    while(head!=null){
	        len++;
	        head=head.next;
	    }
	    ListNode tail = dummy;
	    ListNode first = dummy.next;
	    ListNode cur = first.next;
	    ListNode pre = first;
	
	    for(int i=0;i< len ;i+=k){
	        for(int j=0;j<k;j++){
	            ListNode next = cur.next;
	            cur.next = pre;
	            pre = next ;
	            cur = next;
	        }
	        first.next = cur;
	        pre.next= tail;
	        first = cur;
	        tail = pre;
	
	    }
	    return dummy.next;
	}


#### 13、矩阵打印 zigag问题   



#### 14、 leetcode234 回文链表

##### 1、空间问题为O（N） 

思路直接快慢指针到终点，压栈比对



```
public ListNode getMidNode(ListNode head){
         ListNode fast = head,slow = fast.next;
         while(fast.next!=null && fast.next.next!= null){
             fast=fast.next.next;
             slow = slow.next;
         }
         return slow;
     }
    public boolean isPalindrome(ListNode head) {
        if(head == null ||head .next == null )
            return true;
        ListNode node = getMidNode(head);
        Stack<Integer > stack1 = new Stack<>();
        while (node!=null){
            stack1.push(node.val);
            node=   node.next;
        }
        ListNode tmp = head;
        while (!stack1.isEmpty()){
            int num = stack1.pop();
            if(tmp.val != num){
                return false;
            }
            tmp=tmp.next;
        }
        return true;
    }
```





##### 2、空间O(1)

找到链表终点，反转链表，比对；再恢复链表结构





#### 15 leetcode 138 随机链表的复制 

##### 1、哈希表





##### 2、链表复制



#### 16 程序员代码面试指南 用栈实现另外一个栈的排序

```
思路： 顺序进辅助栈，如果发现不是顺序，就吐出数据。

public static void sortStackByStack(Stack<Integer> stack){
        Stack<Integer> stack1 = new Stack<>();
        while(! stack.isEmpty()){
            int cur = stack.pop();
            while(!stack1.isEmpty() && stack1.peek() < cur){
                stack.push(stack1.pop());
            }
            stack1.push(cur);
        }
        while (!stack1.isEmpty()){
            stack.push(stack1.pop());
        }
}
```


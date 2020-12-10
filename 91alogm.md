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
注意：此方法是正确的，但是过不了测试案例。需要优化
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
        maxHeap.add(num);
    }
    
    public double findMedian() {
        double ans =0;
        int size = maxHeap.size();
        boolean bflag =false;//true 代表是奇数
        bflag = ( maxHeap.size()%2 ==0)? false:true;
        size=  size/2 ;
    
        while(size-->0){
            minHeap.add( maxHeap.poll());
        }
    
        if(bflag == false) {
            ans =  (maxHeap.peek() + minHeap.peek())/2.0f;
        }
        else{
            ans = maxHeap.peek();
        }
        while(!minHeap.isEmpty()){
            maxHeap.add( minHeap.poll());
        }
        return ans;
    }

}

```
9.2两个队列实现一个栈
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

9.1两个栈实现一个队列

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
    
    
    10 设计一个getMin()函数的栈
    
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

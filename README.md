# 目录
* [3 无重复字符的最长子串](#3-无重复字符的最长子串)
* [21 合并两个有序链表](#21-合并两个有序链表)   
* [23 合并k个有序链表](#23-合并k个有序链表)   
* [53 最大子序和](#53-最大子序和)   
* [70 爬楼梯](#70-爬楼梯)     
* [98 验证二叉搜索树](#98-验证二叉搜索树)      
* [101 对称二叉树](#101-对称二叉树)    
  * [解法1 递归](#解法1-递归)    
  * [解法2 迭代](#解法2-迭代)   
* [102 二叉树的层序遍历](#102-二叉树的层序遍历)      
* [104 二叉树的最大深度](#104-二叉树的最大深度)      
  * [解法1 递归](#解法1-递归)      
  * [解法2 层序遍历 队列](#解法2-层序遍历-队列)      
* [111 二叉树的最小深度](#111-二叉树的最小深度)      
  * [解法1 递归](#解法1-递归)      
  * [解法2 层序遍历 队列](#解法2-层序遍历-队列)      
* [148 排序链表](#148-排序链表)    
* [199 二叉树的右视图](#199-二叉树的右视图)    
  * [解法1 广度优先搜索](#解法1-广度优先搜索)    
  * [解法2 深度优先搜索](#解法2-深度优先搜索)    
* [225 用队列实现栈](#225-用队列实现栈)    
* [226 翻转二叉树](#226-翻转二叉树)    
  * [解法1 递归](#解法1-递归)    
  * [解法2 迭代](#解法2-迭代)   
* [232 用栈实现队列](#232-用栈实现队列)  
* [234 回文链表](#234-回文链表)    
  * [解法1 放入list里](#解法1-放入list里)    
  * [解法2 只翻转后半部分链表](#解法2-只翻转后半部分链表)    
* [236 二叉树的最近公共祖先](#236-二叉树的最近公共祖先)    
* [387 字符串中的第一个唯一字符](#387-字符串中的第一个唯一字符)    
* [704 二分查找](#704-二分查找)     
* [Other 1 判断是否为完全二叉树](#Other-1-判断是否为完全二叉树)    
* [Other 2 找出数组中每一个数右面距离它最近的比它大的数](#Other-2-找出数组中每一个数右面距离它最近的比它大的数)    
* [Other 3 给一个奇数位升序 偶数位降序的链表排成有序链表](#Other-3-给一个奇数位升序-偶数位降序的链表排成有序链表)    
* [Other 4 反转一个英文句子中的单词顺序 单词内部字母顺序不变](#Other-4-反转一个英文句子中的单词顺序-单词内部字母顺序不变)    
* [Other 5 两个超大数相加](#Other-5-两个超大数相加)    



# 3 无重复字符的最长子串
* 滑动窗口机制；
* 如果size == 0直接输出0；
* 准备一个滑动窗口的存储map<character，integer>，integer存的是character在s字符串中的下标；
* 两个指针，i和j，i表示后面的end，j表示前面的begin；
* 一开始i和j都指向第一个元素；
* 将第一个元素放入map中，然后往后遍历；
* 如果map中有了这个元素，就把j更新，因为要不重复，肯定是从后一个重复的字符开始重新算；
* j是j和map中这个char的index+1的最大值；
* 如果没有这个元素，就加入这个char，map的key是char的下标（以0开始，所以在更新j的时候要index+1）；
* 同时要更新max，是max和i-j+1的最大值；

```java
    public int lengthOfLongestSubstring(String s) {
        if(s.length() == 0){
            return 0;
        }
        HashMap<Character, Integer> map = new HashMap<>();
        int i=0;
        int j=0;
        int max = 0;
        for(i = 0; i<s.length(); i++){
            if(map.containsKey(s.charAt(i))){
                j = Math.max(j, map.get(s.charAt(i)) + 1);
            }
            map.put(s.charAt(i), i);
            max = Math.max(max, i-j+1);
        }
        return max;
    }
```

# 21 合并两个有序链表
* 建立新链表头；
* 判断两个链表头大小，选择放入；
* 最后将剩下的链表头放入；

```java
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode i = l1;
        ListNode j = l2;
        ListNode curNode = new ListNode(-1);
        ListNode resultNode = curNode;
        while(i != null && j != null) {
        	if(i.value < j.value) {
        		curNode.next = i;
        		i = i.next;
        	}else {
        		curNode.next = j;
        		j = j.next;
        	}
        	curNode = curNode.next;
        }
        curNode.next = i != null ? i : j;
        return resultNode.next;
    }
	
```

# 23 合并k个有序链表
* 常规归并；

```java
    public ListNode mergeKLists(ListNode[] lists) {
		if(lists.length == 0) {
			return null;
		}
		if(lists.length == 1) {
			return lists[0];
		}
		if(lists.length == 2) {
			return mergeTwoLists(lists[0], lists[1]);
		}
		int mid = lists.length / 2;
		ListNode[] l1 = new ListNode[mid];
		for(int i=0; i<mid; i++) {
			l1[i] = lists[i];
		}
		ListNode[] l2 = new ListNode[lists.length - mid];
		for(int i=0, j=mid; j<lists.length; i++, j++) {
			l2[i] = lists[j];
		}
		
		return mergeLists(mergeKLists(l1), mergeKLists(l2));
    }
	
	public ListNode mergeLists(ListNode n1, ListNode n2) {
		ListNode curNode = new ListNode(-1);
		ListNode result = curNode;
		ListNode i = n1;
		ListNode j = n2;
		while(i != null && j != null) {
			if(i.value < j.value) {
				curNode.next = i;
				i = i.next;
			}
			if(i.value > j.value) {
				curNode.next = j;
				j = j.next;
			}
			curNode = curNode.next;
		}
		curNode.next = i == null ? j : i;
		return result.next;
	}
```


# 53 最大子序和
* 设置全局最大值，开始，结束index；
* 设置前一个最大值，sub开始，sub结束index；
* 比较从index=1开始的所有元素；
* 如果subMax>0表示对当前元素有增加作用，subMax就加上这个元素，subEnd++；
* 否则sub就从当前元素开始，sub的开始结束index都变成当前index，sub的max也等于当前元素的值；
* 然后比较subMax和max，看是否需要更新前后index；

```java
    public int maxSubArray(int[] nums) {
		int start = 0;
		int end = 0;
		int subStart = 0;
		int subEnd = 0;
		int max = nums[0];
		int subMax = nums[0];
		for(int i=1; i<nums.length; i++) {
			if(subMax > 0) {
				subMax += nums[i];
				subEnd++;
			}else {
				subMax = nums[i];
				subStart = i;
				subEnd = i;
			}
			if(subMax > max) {
				max = subMax;
				start = subStart;
				end = subEnd;
			}
		}
		System.out.println(start + "..." + end);
		return max;
	}
```



# 70 爬楼梯
* 递归的话会超过时间限制；
* 动态规划定义数组大小为n+3；
* 因为必须要大于n+2才能定义result[2]；

```java
    public int climbStairs(int n) {
		int[] result = new int[n+3];
		result[0] = 0;
		result[1] = 1;
		result[2] = 2;
		for(int i=3; i<=n; i++) {
			result[i] = result[i-1] + result[i-2];
		}
		return result[n];
    }
```
【拓展】
一个台阶总共有n级，如果一次可以跳1级，也可以跳2级......它也可以跳上n级。此时该青蛙跳上一个n级的台阶总共有多少种跳法？ -> `2 * f（n-1）`




# 98 验证二叉搜索树
* 借助一个helper方法给定node和lower，upper界限；
* 如果node的值在这个范围内（闭区间），就符合条件；

```java
    public boolean isValidBST(TreeNode root) {
		return helper(root, null, null);
    }
	
	public boolean helper(TreeNode node, Integer lower, Integer upper) {
		if(node == null) {
			return true;
		}
		
		int value = node.value;
		if(lower != null && value <= lower) {
			return false;
		}
		if(upper != null && value >= upper) {
			return false;
		}
		
		if(!helper(node.LNode, lower, value)) {
			return false;
		}
		if(!helper(node.RNode, value, upper)) {
			return false;
		}
		
		return true;
	}
```
 


# 101 对称二叉树
## 解法1 递归
* 判断root和root；
* 返回p.val == q.val && check(p.left, q.right) && check(p.right, q.left);

```java
    public boolean isSymmetric(TreeNode root) {
		return check1(root, root);
    }
	
	public boolean check1(TreeNode p, TreeNode q) {
		if(p == null && q == null) {
			return true;
		}
		if(p == null || q == null) {
			return false;
		}
		return p.value == q.value && check1(p.LNode, q.RNode) && check1(p.RNode, q.LNode);
	}
```

## 解法2 迭代
* 提到迭代就是队列；
* 把两个root放进队列；
* 判断出队的两个元素；
* 记得先判断它俩是否为空，再判断值是否相等，因为会出现它是null取不到值的情况；
* 再按照左的左，右的右，左的右，右的左的顺序，放入队列；

```java
    public boolean check2(TreeNode p, TreeNode q) {
		Queue<TreeNode> queue = new LinkedList<>();
		queue.add(p);
		queue.add(q);
		TreeNode nodeP = null;
		TreeNode nodeQ = null;
		while(!queue.isEmpty()) {
			nodeP = queue.poll();
			nodeQ = queue.poll();
			if(nodeP == null && nodeQ == null) {
				continue;
			}
			if(nodeP == null || nodeQ == null || nodeP.value != nodeQ.value) {
				return false;
			}
			queue.add(nodeP.LNode);
			queue.add(nodeQ.RNode);
			
			queue.add(nodeP.RNode);
			queue.add(nodeQ.LNode);
		}
		return true;
	}
```


# 102 二叉树的层序遍历
* 常规队列；
* 定位当前层floor；

```java
    public List<List<Integer>> levelOrder(TreeNode root) {
		List<List<Integer>> result = new LinkedList<>();
		if(root == null) {
			return result;
		}
		Queue<TreeNode> queue = new LinkedList<>();
		queue.add(root);
		TreeNode curNode = null;
		while(!queue.isEmpty()) {
			int size = queue.size();
			List<Integer> curFloor = new LinkedList<>();
			for(int i=0; i<size; i++) {
				curNode = queue.peek();
				if(curNode.LNode != null) {
					queue.add(curNode.LNode);
				}
				if(curNode.RNode != null) {
					queue.add(curNode.RNode);
				}
				curFloor.add(curNode.value);
				queue.poll();
			}
			result.add(curFloor);
			
		}
		return result;
    }
```	


# 104 二叉树的最大深度

## 解法1 递归
* 1 + 子树的最大深度；
* 子树最大深度 = 左右中的最大值；

```java
    public int maxDepth1(TreeNode root) {
		if(root == null) {
			return 0;
		}
		if(root.LNode == null && root.RNode == null) {
			return 1;
		}
		int LMax = maxDepth1(root.LNode);
		int RMax = maxDepth1(root.RNode);
		return Math.max(LMax, RMax) + 1;
    }
```

## 解法2 层序遍历 队列
* 常规队列，定位层数；

```java
    public int maxDepth2(TreeNode root) {
		if(root == null) {
			return 0;
		}
		Queue<TreeNode> queue = new LinkedList<>();
		queue.add(root);
		TreeNode curNode = null;
		int depth = 0;
		while(!queue.isEmpty()) {
			int size = queue.size();
			
			for(int i=0; i<size; i++) {
				curNode = queue.peek();
				if(curNode.LNode != null) {
					queue.add(curNode.LNode);
				}
				if(curNode.RNode != null) {
					queue.add(curNode.RNode);
				}
				queue.poll();
			}
			depth++;	
		}
		return depth;
	}
```

# 111 二叉树的最小深度
## 解法1 递归
* 1 + 子树的最小深度；
* 注意最小深度是按照叶子节点的层数来算的；
* 所以在过程中要去除不是叶子节点的层数；
* 即用if left ！= null来判断，决定是否要继续遍历这一层left；

```java
    public int minDepth1(TreeNode root) {
		if(root == null) {
			return 0;
		}
		if(root.LNode == null && root.RNode == null) {
			return 1;
		}
		int min = Integer.MAX_VALUE;
		if(root.LNode != null) {
			min = Math.min(min, minDepth1(root.LNode));
		}
		if(root.RNode != null) {
			min = Math.min(min, minDepth1(root.RNode));
		}
		
		return 1 + min;
    }
```


## 解法2 层序遍历 队列
* 常规队列；
* 其中如果碰到叶子节点，即左右节点均是null，就可以直接return level；

```java
    public int minDepth2(TreeNode root) {
		if(root == null) {
			return 0;
		}
		Queue<TreeNode> queue = new LinkedList<>();
		queue.add(root);
		TreeNode curNode = null;
		int level = 0;
		while(!queue.isEmpty()) {
			level++;
			int size = queue.size();
			for(int i=0; i<size; i++) {
				curNode = queue.peek();
				if(curNode.LNode == null && curNode.RNode == null) {
					return level;
				}
				if(curNode.LNode != null) {
					queue.add(curNode.LNode);
				}
				if(curNode.RNode != null) {
					queue.add(curNode.RNode);
				}
				queue.poll();
			}
		}
	
		return -1;
    }
```




# 148 排序链表
* 归并排序；
* 先递归排序好左右两部分；
* 然后把左右两部分合并；
* 返回链表头即可；

```java
    public ListNode sortList(ListNode head) {
		if(head == null || head.next == null) {
			return head;
		}
		ListNode fast = head;
		ListNode slow = head;
		while(fast.next != null && fast.next.next != null) {
			fast = fast.next.next;
			slow = slow.next;
		}
		ListNode tempNode = slow.next;
		slow.next = null;
		ListNode leftList = sortList(head);
		ListNode rightList = sortList(tempNode);
		ListNode result = mergeList(leftList, rightList);
		return result;
    }
	
	public ListNode mergeList(ListNode left, ListNode right) {
		ListNode i = left;
		ListNode j = right;
		ListNode curIndexNode = new ListNode(-1);
		ListNode result = curIndexNode;
		while(i != null && j != null) {
			if(i.value < j.value) {
				curIndexNode.next = i;
				i = i.next;
			}else {
				curIndexNode.next = j;
				j = j.next;
			}
			curIndexNode = curIndexNode.next;
		}
		curIndexNode.next = i == null ? j : i;
		return result.next;
	}
```


# 199 二叉树的右视图
## 解法1 广度优先搜索
* 层序遍历；
* 每一层最后一个元素就是右视图见到的第一个元素；
* 队列；
* 设置每一层的元素个数size即queue.size()来判断这一层是否遍历结束；

```java
    public List<Integer> rightSideView1(TreeNode root) {
		Queue<TreeNode> queue = new LinkedList<>();
		List<Integer> result = new ArrayList<>();
		if(root == null) {
			return result;
		}
		queue.add(root);
		TreeNode curNode = null;
		while(!queue.isEmpty()) {
			int curSize = queue.size();
			for(int i=0; i<curSize; i++) {
				curNode = queue.peek();
				if(curNode.LNode != null) {
					queue.add(curNode.LNode);
				}
				if(curNode.RNode != null) {
					queue.add(curNode.RNode);
				}
				if(i == curSize - 1) {
					result.add(curNode.value);
				}
				queue.poll();
			}
			
		}
		
		
		return result;
    }
```

## 解法2 深度优先搜索
* 按深度进行搜索；
* 先看右子树；
**【左视图就先看左子树】**
* 每一层最右面第一个访问的元素即右视图；
* 设置当前depth，如果==result.size()表示当前节点是新一层的第一个节点，加入result list中；
* 根节点的depth为0；

```java
    List<Integer> result = new ArrayList<>();
	public List<Integer> rightSideView2(TreeNode root) {
		dfs(root, 0);
		return result;
    }
	
	private void dfs(TreeNode root, int depth) {
    	if(root == null) {
    		return;
    	}
    	if(depth == result.size()) {
    		result.add(root.value);
    	}
    	depth++;
    	dfs(root.RNode, depth);
    	dfs(root.LNode, depth);
    }
```


# 225 用队列实现栈
* 入栈的队列；
* 出栈的队列；
* 入栈入到input里，如果output有东西，依次poll出来到input里；
* input和output交换；
* 这样之后output的顺序就是栈的顺序；

```java
class MyStack {
    private Queue<Integer> input;
    private Queue<Integer> output;
    /** Initialize your data structure here. */
    public MyStack() {
        input = new LinkedList<>();
        output = new LinkedList<>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        input.add(x);
        while(!output.isEmpty()){
            input.add(output.poll());
        }
        Queue temp = input;
        input = output;
        output = temp;
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        
        return output.poll();
    }
    
    /** Get the top element. */
    public int top() {
        
        return output.peek();

    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return output.isEmpty();
    }
}
```



# 226 翻转二叉树
## 解法1 递归
* root的left = 翻转right；
* root的right = 翻转left；

```java
    public TreeNode invertTree1(TreeNode root) {
		 if(root == null) {
			 return null;
		 }
		 TreeNode left = invertTree1(root.LNode);
		 TreeNode right = invertTree1(root.RNode);
		 root.LNode = right;
		 root.RNode = left;
		 return root;
	}
```

## 解法2 迭代
* 队列；
* 定义temp表示左；
* 左右交换；
* 如果有左子右子继续往队列里加；

```java
    public TreeNode invertTree2(TreeNode root) {
		 if(root == null) {
			 return null;
		 }
		 Queue<TreeNode> queue = new LinkedList<>();
		 queue.add(root);
		 while(!queue.isEmpty()) {
			 TreeNode curNode = queue.poll();
			 TreeNode tempNode = curNode.LNode;
			 curNode.LNode = curNode.RNode;
			 curNode.RNode = tempNode;
			 if(curNode.LNode != null) {
				 queue.add(curNode.LNode);
			 }
			 if(curNode.RNode != null) {
				 queue.add(curNode.RNode);
			 }
			 
		 }
		 return root;
	 }
```


# 232 用栈实现队列
* 入队列的栈；
* 出队列的栈；
* 入队列就正常的入栈到表示入队列的栈；
* 出队列先判断表示出队列的栈是否为空，如果为空，把表示入队列的栈内元素全部pop再push进表示出队列的栈，最后出栈即可；
* peek和出队列同理；

```java
class MyQueue {
    private Stack<Integer> input;
    private Stack<Integer> output;
    /** Initialize your data structure here. */
    public MyQueue() {
        input = new Stack<>();
        output = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        input.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if(output.isEmpty()){
            while(!input.isEmpty()){
                output.push(input.pop());
            }
        }
        return output.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        if(output.isEmpty()){
            while(!input.isEmpty()){
                output.push(input.pop());
            }
        }
        return output.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return input.isEmpty() && output.isEmpty();
    }
}
```


# 234 回文链表
## 解法1 放入list里
* 将单向链表的数据循环存在list当中；
* 两个指针分别指向开头和结尾；
* 判断两个指针指向元素的值，如果相等则继续；
* 如果中途有值不等的直接return false；
* 循环判断结束条件是i > j，即如果i <= j都继续判断；
* 如果可以经历最后一步，则return true，表示全都比对完了；

```java
    public Boolean isPalidrome1(ListNode head) {
		if(head == null || head.next == null) {
			return true;
		}
		ArrayList<Integer> list = new ArrayList<>();
		ListNode cur = head;
		while(cur != null) {
			list.add(cur.value);
			cur = cur.next;
		}
		int i = 0;
		int j = list.size() - 1;
		while(i <= j) {
			if(list.get(i).equals(list.get(j))) {
				i++;
				j--;
			}else {
				return false;
			}
		}
		return true;
	}
```

## 解法2 只翻转后半部分链表
* 得到前半部分的最后一个节点；
* 得到后半部分翻转后的第一个节点；
* 比较两个链表；
* 前半部分的最后一个节点通过快慢指针得到；
* 后半部分翻转通过正常的移动得到；

```java
    public Boolean isPalidrome2(ListNode head) {
		if(head == null || head.next == null) {
			return true;
		}
		
		
		ListNode half1End = getHalf1End(head);
		ListNode half2Start = reverseList(half1End.next);
		
		ListNode p = head;
		ListNode q = half2Start;
		
		while(q != null) {
			if(p.value == q.value) {
				p = p.next;
				q = q.next;
			}else {
				return false;
			}
		}
		ListNode newHalf2Start = reverseList(half2Start);
		half1End.next = newHalf2Start;
		
		return true;
	}
	
	public ListNode getHalf1End(ListNode head) {
		ListNode slow = head;
		ListNode fast = head;
		while(fast.next != null && fast.next.next != null) {
			slow = slow.next;
			fast = fast.next.next;
		}
		return slow;
	}
	
	public ListNode reverseList(ListNode head) {
		ListNode preNode = null;
		ListNode curNode = head;
		while(curNode != null) {
			ListNode nextNode = curNode.next;
			curNode.next = preNode;
			preNode = curNode;
			curNode = nextNode;
		}
		return preNode;
	}
	
```




# 236 二叉树的最近公共祖先
* 如果根节点是null，或者根节点恰好是p或者q，直接返回根节点；
* 让left=以根节点的左子节点为根节点的树找pq的lca；
* 让right=以根节点的右子节点为根节点的树找pq的lca;
* 如果left==null，说明pq都不在left上，那么就在right上；
* 如果right==null，说明pq都不在right上，那么就在left上；
* 如果left和right都不为null，说明一面有一个，那么lca就是当前的root；

```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q){
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.LNode, p, q);
        TreeNode right = lowestCommonAncestor(root.RNode, p, q);
        if(left == null){
            return right;
        }
        if(right == null){
            return left;
        }
        return root;
    }
```

# 387 字符串中的第一个唯一字符
* 将字符串s放入char数组中；
* 遍历整个数组，将每个字母出现的次数++到count[26]数组中；
* 以下标i开始顺序遍历整个char数组；
* 如果count中对应的次数为1则输出下标；

```java
    public int firstUniqChar(String s) {
		int[] count = new int[26];
		char[] arr = s.toCharArray();
		for(char ch:arr) {
			count[ch - 'a']++;
		}
		for(int i=0; i<s.length(); i++) {
			if(count[arr[i] - 'a'] == 1) {
				return i;
			}
		}
		return -1;
    }
```


# 704 二分查找
* 左0右length-1；
* 只要左<右就while循环；
* 让mid=中间index；
* 如果中间==target直接返回；
* 如果<target，left = mid+1；
* 如果>target，right = mid-1；

```java
    public int search(int[] nums, int target) {
		 if(nums == null || nums.length == 0){
			 return -1;
		 }
		 int left = 0;
		 int right = nums.length - 1;
		 while(left <= right) {
			 int mid = left + (right - left) / 2;
			 if(nums[mid] == target) {
				 return mid;
			 }else if(nums[mid] > target) {
				 right = mid - 1;
			 }else if(nums[mid] < target) {
				 left = mid + 1;
			 }
		 }
		 return -1;
	 }
	
```




# Other 1 判断是否为完全二叉树
* 通过层序遍历，将第一个null之前的所有节点入队列；
* 先将根节点入队，再将其左右子节点入队，如果没有则break，然后根节点出队；
* 再把此时队列的第一位的左右子节点入队，它自己出队poll；
* 直到有一个是null，跳出while循环；
* 如果是完全二叉树，那么第一个null之后的所有节点都应该是null；
* 先判断剩下的节点们，用peek判断队首，是否有右节点，如果没有，它也poll；
* 否则的话，依次遍历剩下的节点们，如果左右有一个不是空，那么就return false；

```java
    public Boolean isCompleteTree(TreeNode root) {
		if(root == null || (root.LNode == null && root.RNode == null)) {
			return true;
		}
		Queue<TreeNode> queue = new LinkedList<>();
		queue.add(root);
		TreeNode curNode = null;
		while(!queue.isEmpty()) {
			curNode = queue.peek();
			if(curNode.LNode != null) {
				queue.add(curNode.LNode);
			}else {
				break;
			}
			if(curNode.RNode != null) {
				queue.add(curNode.RNode);
			}else {
				break;
			}
			queue.poll();
		}
		if(queue.peek().RNode == null) {
			queue.poll();
		}
		while(!queue.isEmpty()) {
			curNode = queue.poll();
			if(curNode.LNode != null || curNode.RNode != null) {
				return false;
			}
		}
		
		
		return true;
	}
```

# Other 2 找出数组中每一个数右面距离它最近的比它大的数
* 建立一个结果数组；
* 先将第一个数的index-0入栈；
* 如果第二个数大于栈顶元素，result[栈顶元素] = 第二个数；
* 否则，第二个数的index-1入栈；
* for循环直到最后一个数，将栈里剩余元素对应的result为-1；
* （循环数组的话，逆向，找到最后一个比它大的数即可；）

```java
	public int[] findNearestMax(int[] arr) {
		if(arr == null) {
			return arr;
		}
		int[] result = new int[arr.length];
		Stack<Integer> indexStack = new Stack<>();
		indexStack.push(0);
		int index = 1;
		while(index < arr.length) {
			if(!indexStack.isEmpty() && arr[index] > arr[indexStack.peek()]) {
				result[indexStack.pop()] = arr[index];
			}else {
				indexStack.push(index);
				index++;
			}
		}
		if(!indexStack.isEmpty()) {
			result[indexStack.pop()] = -1;
		}
		
		return result;
	}
```

# Other 3 给一个奇数位升序 偶数位降序的链表排成有序链表
* 先根据奇偶位进行拆分成两个链表；
* 注意赋值拆分后的链表最后一位的next是null；
* 对偶数位的链表进行翻转；
* 两个链表进行合并；

```java
    public ListNode orderCustomList(ListNode head) {
		ListNode tempNode1 = new ListNode(-1);
		ListNode tempNode2 = new ListNode(-1);
		ListNode listNode1 = tempNode1;
		ListNode listNode2 = tempNode2;
		ListNode curNode = head;
		while(curNode != null && curNode.next != null) {
			tempNode1.next = curNode;
			tempNode2.next = curNode.next;
			curNode = curNode.next.next;
			tempNode1 = tempNode1.next;
			tempNode2 = tempNode2.next;
		}
		tempNode1.next = curNode;
		tempNode1 = tempNode1.next;
		tempNode1.next = null;
		tempNode2.next = null;
		
		listNode1 = listNode1.next;
		listNode2 = reverseList(listNode2.next);
		
		ListNode result = mergeList(listNode1, listNode2);
		return result;
	}
	
	public ListNode reverseList(ListNode head) {
		ListNode preNode = null;
		ListNode curNode = head;
		while(curNode != null) {
			ListNode nextNode = curNode.next;
			curNode.next = preNode;
			preNode = curNode;
			curNode = nextNode;
		}
		return preNode;
	}
	
	public ListNode mergeList(ListNode left, ListNode right) {
		ListNode i = left;
		ListNode j = right;
		ListNode curIndexNode = new ListNode(-1);
		ListNode result = curIndexNode;
		while(i != null && j != null) {
			if(i.value < j.value) {
				curIndexNode.next = i;
				i = i.next;
			}else {
				curIndexNode.next = j;
				j = j.next;
			}
			curIndexNode = curIndexNode.next;
		}
		curIndexNode.next = i == null ? j : i;
		return result.next;
	}
```



# Other 4 反转一个英文句子中的单词顺序 单词内部字母顺序不变
* 将string用split分开得到一个string数组；
* 将string数组内元素反转；
* 后赋值到一个result string当中；

```java
    public String reverseSentence(String str) {
		if(str == null) {
			return null;
		}
		String[] sentence = str.split(" ");
		for(int i=0, j=sentence.length-1; i<=j; i++, j--) {
			String temp = sentence[i];
			sentence[i] = sentence[j];
			sentence[j] = temp;
		}
		String result = "";
		for(String word:sentence) {
			result += word + " ";
		}
		
		return result;
	}
```

# Other 5 两个超大数相加
* 用stringbuilder将它俩反转，放入一个char数组中；
* 建立一个新的int数组，长度是最长的char数组的长度+1；
* 两个char数组相加过程中建立temp表示当前两个数相加和；
* 如果有进位就处理进位；
* 最后判断最后一位是不是零，如果是零就不要放入结果string中；
* 然后stringbuilder继续把结果string反转；

```java
    public static String bigAdd(String a, String b) {
		String result = "";
		char[] arrA = new StringBuilder(a).reverse().toString().toCharArray();
		char[] arrB = new StringBuilder(b).reverse().toString().toCharArray();
		
		int  maxLength = 1 + Math.max(arrA.length, arrB.length);
		int[] addAB = new int[maxLength];
		int temp = 0;
		for(int i=0; i<maxLength; i++) {
			if(i < arrA.length) {
				temp += arrA[i] - '0';
			}
			if(i < arrB.length) {
				temp += arrB[i] - '0';
			}
			addAB[i] = temp % 10;
			temp = temp < 10 ? 0 : 1;
		}
		int i;
		for(i=0; i<addAB.length-1; i++) {
			result += Integer.toString(addAB[i]);
		}
		if(addAB[i] != 0) {
			result += Integer.toString(addAB[i]);
		}
		
		String bigAddResult = new StringBuilder(result).reverse().toString();
		
		return bigAddResult;
	}
```

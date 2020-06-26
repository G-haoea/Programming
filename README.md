# 目录
* [53 最大子序和](#53-最大子序和)    
* [101 对称二叉树](#101-对称二叉树)    
  * [解法1 递归](#解法1-递归)    
  * [解法2 迭代](#解法2-迭代)    
* [148 排序链表](#148-排序链表)    
* [199 二叉树的右视图](#199-二叉树的右视图)    
  * [解法1 广度优先搜索](#解法1-广度优先搜索)    
  * [解法2 深度优先搜索](#解法2-深度优先搜索)    
* [234 回文链表](#234-回文链表)    
  * [解法1 放入list里](#解法1-放入list里)    
  * [解法2 只翻转后半部分链表](#解法2-只翻转后半部分链表)    
* [236 二叉树的最近公共祖先](#236-二叉树的最近公共祖先)    
* [387 字符串中的第一个唯一字符](#387-字符串中的第一个唯一字符)    
* [Other 1 判断是否为完全二叉树](#Other-1-判断是否为完全二叉树)    
* [Other 2 找出数组中每一个数右面距离它最近的比它大的数](#Other-2-找出数组中每一个数右面距离它最近的比它大的数)    
* [Other 3 给一个奇数位升序 偶数位降序的链表排成有序链表](#Other-3-给一个奇数位升序-偶数位降序的链表排成有序链表)    


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
* 提到跌倒就是队列；
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

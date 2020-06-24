# 目录
* [234 回文链表](#234-回文链表)    
  * [解法1 放入list里](#解法1-放入list里)    
  * [解法2 只翻转后半部分链表](#解法2-只翻转后半部分链表)    
* [236 二叉树的最近公共祖先](#236-二叉树的最近公共祖先)    
* [387 字符串中的第一个唯一字符](#387-字符串中的第一个唯一字符)    
* [Other 1 判断是否为完全二叉树](#Other-1-判断是否为完全二叉树)    
* [Other 2 找出数组中每一个数右面，距离它最近的比它大的数](#Other-2-找出数组中每一个数右面，距离它最近的比它大的数)    

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

# Other 2 找出数组中每一个数右面，距离它最近的比它大的数
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

**2-21-206-23-237-148-138-141-24-234-445-147-143** 链表
**42-20-85-155-739-173-316-394-150-224-94** 栈
**104-226-96-617-173-108-297-100-105-95-124-654** 树
### 4. 树
- 节点的度：节点有几个孩子
	- 度为 0 的节点比度为 2 的节点多一个 `a+b+c-1 = 2c+b+0c   ==> a = c - 1`
- 平衡二叉树：所有节点的两子树高度差不超过 1
- 二叉搜索树：中序遍历结果是有序数组
- 满二叉树：只有度为0和2的树
- 完全二叉树：完美二叉树少最下面的几个
	- 若根编号为1，孩子编号都为 2i 和 2i+1
	- **所以可以用连续空间存储（数组）**
- 记录式  <==> 计算式   时间空间互换
- 树结构的深入理解
	- 节点：集合 => 子节点：性质不同的子集；所有子集相加等于全集
	- 边：关系
- 多叉树 ----> 二叉树：左孩子右兄弟 ---> 当根节点有右节点，表示森林

- 二叉树
	- 完全二叉树（维护集合最值）
		- 堆
		- 优先队列
- 多叉树 / 森林
	- 字典树--->字符串及其相关转化问题
	- AC 自动机--同上
	- 并查集--->连通性问题
- 二叉排序树
	- AVL 树---> 语言标准库中重要的数据检索容器底层实现
	- 2-3树 ---> 同上
	- 红黑树---> 同上
	- B-树 / B+树 ---> 文件系统、数据库 底层重要数据结构


### 5. 堆
**是优先队列的一种实现方式**
**集合最值**
- 数据结构：定义一种性质，并且维护这种性质
- 堆的基础应用
  - [剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)
  - [1046. 最后一块石头的重量](https://leetcode-cn.com/problems/last-stone-weight/)
  - [703. 数据流中的第 K 大元素](https://leetcode-cn.com/problems/kth-largest-element-in-a-stream/)
  - [373. 查找和最小的K对数字](https://leetcode-cn.com/problems/find-k-pairs-with-smallest-sums/)
  - [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)
- 堆的进阶应用
  - [355. 设计推特](https://leetcode-cn.com/problems/design-twitter/)
  - [692. 前K个高频单词](https://leetcode-cn.com/problems/top-k-frequent-words/)
  - [面试题 17.20. 连续中值](https://leetcode-cn.com/problems/continuous-median-lcci/)
  - [295. 数据流的中位数](https://leetcode-cn.com/problems/find-median-from-data-stream/)
  - [1801. 积压订单中的订单总数](https://leetcode-cn.com/problems/number-of-orders-in-the-backlog/)
- 智力发散
  - [264. 丑数 II](https://leetcode-cn.com/problems/ugly-number-ii/)
  - [313. 超级丑数](https://leetcode-cn.com/problems/super-ugly-number/)
  - [1753. 移除石子的最大得分](https://leetcode-cn.com/problems/maximum-score-from-removing-stones/)


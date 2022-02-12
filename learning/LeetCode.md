### 1. 链表

**2-21-206-23-237-148-138-141-24-234-445-147-143**

- 链表的访问
  - [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)
  - [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
  - [202. 快乐数](https://leetcode-cn.com/problems/happy-number/)
- 链表的反转
  - [138. 复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)
  - [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/)
  - [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)
  - [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)
  - [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)
  - [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)
  - [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)
- 链表节点删除
  - [19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)
  - [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)
  - [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

### 2. 队列

- [622. 设计循环队列](https://leetcode-cn.com/problems/design-circular-queue/)
- [641. 设计循环双端队列](https://leetcode-cn.com/problems/design-circular-deque/)
- [1670. 设计前中后队列](https://leetcode-cn.com/problems/design-front-middle-back-queue/)
- [933. 最近的请求次数](https://leetcode-cn.com/problems/number-of-recent-calls/)
- [面试题 17.09. 第 k 个数](https://leetcode-cn.com/problems/get-kth-magic-number-lcci/)
- [859. 亲密字符串](https://leetcode-cn.com/problems/buddy-strings/)
- [860. 柠檬水找零](https://leetcode-cn.com/problems/lemonade-change/)
- [969. 煎饼排序](https://leetcode-cn.com/problems/pancake-sorting/)
- [621. 任务调度器](https://leetcode-cn.com/problems/task-scheduler/)

### 3. 递归与栈

**处理具有完全包含关系的问题**

**42-20-85-155-739-173-316-394-150-224-94**

- 栈的基本操作
  - [面试题 03.04. 化栈为队](https://leetcode-cn.com/problems/implement-queue-using-stacks-lcci/)
  - [682. 棒球比赛](https://leetcode-cn.com/problems/baseball-game/)
  - [844. 比较含退格的字符串](https://leetcode-cn.com/problems/backspace-string-compare/)
  - [946. 验证栈序列](https://leetcode-cn.com/problems/validate-stack-sequences/)
- 栈结构扩展应用
  - [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)
  - [1021. 删除最外层的括号](https://leetcode-cn.com/problems/remove-outermost-parentheses/)
  - [1249. 移除无效的括号](https://leetcode-cn.com/problems/minimum-remove-to-make-valid-parentheses/)
  - **[145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)**    **递归转迭代**
  - [331. 验证二叉树的前序序列化](https://leetcode-cn.com/problems/verify-preorder-serialization-of-a-binary-tree/)
  - [227. 基本计算器 II](https://leetcode-cn.com/problems/basic-calculator-ii/)
- 智力发散题
  - **[636. 函数的独占时间](https://leetcode-cn.com/problems/exclusive-time-of-functions/)**
  - **[1124. 表现良好的最长时间段](https://leetcode-cn.com/problems/longest-well-performing-interval/)**

### 4. 树

**104-226-96-617-173-108-297-100-105-95-124-654**

- 节点的度：节点有几个孩子
  - 度为 0 的节点比度为 2 的节点多一个

    `a+b+c-1 = 2c+b+0c   ==> a = c-1`

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
- 递归关键点：赋予递归函数明确的意义
- 多叉树 ----> 二叉树：左孩子右兄弟 ---> 当根节点有右节点，表示森林

- 二叉树
  - 完全二叉树
    - 堆
    - 优先队列
    - 维护集合最值
  - 多叉树 / 森林
    - 字典树--->字符串及其相关转化问题
    - AC 自动机--同上
    - 并查集--->连通性问题
  - 二叉排序树
    - AVL 树---> 语言标准库中重要的数据检索容器底层实现
    - 2-3树 ---> 同上
    - 红黑树---> 同上
    - B-树 / B+树 ---> 文件系统、数据库 底层重要数据结构

- 二叉树的基本操作
  - [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)
  - **[589. N 叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)**    **递归转迭代**
  - [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)
  - [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)
  - [107. 二叉树的层序遍历 II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)
  - [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)
- 二叉树的进阶操作
  - [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)
  - [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)
  - [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
  - [222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)
  - [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)
  - [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)
  - [968. 监控二叉树](https://leetcode-cn.com/problems/binary-tree-cameras/)（难度有点大）
  - [662. 二叉树最大宽度](https://leetcode-cn.com/problems/maximum-width-of-binary-tree/) （恶心人的东西）

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








- 22-1-17  [650. 只有两个键的键盘](https://leetcode-cn.com/problems/2-keys-keyboard/)
- 22-1-18  [539. 最小时间差](https://leetcode-cn.com/problems/minimum-time-difference/)
- 22-1-18  [593. 有效的正方形](https://leetcode-cn.com/problems/valid-square/)
- 22-1-18  [1450. 在既定时间做作业的学生人数](https://leetcode-cn.com/problems/number-of-students-doing-homework-at-a-given-time/)
- 22-1-19  [219. 存在重复元素 II](https://leetcode-cn.com/problems/contains-duplicate-ii/)
- 22-1-20
	- [2029. 石子游戏 IX](https://leetcode-cn.com/problems/stone-game-ix/) 分析计算
	- [887. 鸡蛋掉落](https://leetcode-cn.com/problems/super-egg-drop/) 动态规划
- 22-1-21 [821. 字符的最短距离](https://leetcode-cn.com/problems/shortest-distance-to-a-character/)
- 22-1-24
	- [1456. 定长子串中元音的最大数目](https://leetcode-cn.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)
	- [319. 灯泡开关](https://leetcode-cn.com/problems/bulb-switcher/)
	- [2000. 反转单词前缀](https://leetcode-cn.com/problems/reverse-prefix-of-word/)
- 22-1-25  [1688. 比赛中的配对次数](https://leetcode-cn.com/problems/count-of-matches-in-tournament/)
- 22-1-26  [2013. 检测正方形](https://leetcode-cn.com/problems/detect-squares/)
- 22-1-27  [2047. 句子中的有效单词数](https://leetcode-cn.com/problems/number-of-valid-words-in-a-sentence/)
- 22-1-28  [1996. 游戏中弱角色的数量](https://leetcode-cn.com/problems/the-number-of-weak-characters-in-the-game/)
- 22-1-29  [1669. 合并两个链表](https://leetcode-cn.com/problems/merge-in-between-linked-lists/)
- 22-2-30  [278. 第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)
- 22-1-31  [1342. 将数字变成 0 的操作次数](https://leetcode-cn.com/problems/number-of-steps-to-reduce-a-number-to-zero/)
- 22-2-1    [1763. 最长的美好子字符串](https://leetcode-cn.com/problems/longest-nice-substring/)
- 22-2-2   [2000. 反转单词前缀](https://leetcode-cn.com/problems/reverse-prefix-of-word/)
- 22-2-3   [1414. 和为 K 的最少斐波那契数字数目](https://leetcode-cn.com/problems/find-the-minimum-number-of-fibonacci-numbers-whose-sum-is-k/)
- 22-2-4   [1725. 可以形成最大正方形的矩形数目](https://leetcode-cn.com/problems/number-of-rectangles-that-can-form-the-largest-square/)
- 22-2-5   [1652. 拆炸弹](https://leetcode-cn.com/problems/defuse-the-bomb/)
- 22-2-6   [1748. 唯一元素的和](https://leetcode-cn.com/problems/sum-of-unique-elements/)
- 22-2-7   [1405. 最长快乐字符串](https://leetcode-cn.com/problems/longest-happy-string/)
- 22-2-8   [剑指 Offer II 008. 和大于等于 target 的最短子数组](https://leetcode-cn.com/problems/2VG8Kg/)
- 22-2-9   [2006. 差的绝对值为 K 的数对数目](https://leetcode-cn.com/problems/count-number-of-pairs-with-absolute-difference-k/)
- 22-2-10 [1447. 最简分数](https://leetcode-cn.com/problems/simplified-fractions/)
- 22-2-11 [1984. 学生分数的最小差值](https://leetcode-cn.com/problems/minimum-difference-between-highest-and-lowest-of-k-scores/)
- 22-2-12 [1020. 飞地的数量](https://leetcode-cn.com/problems/number-of-enclaves/)
- 22-2-13 
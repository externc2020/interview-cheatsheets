1.传统的diff算法通过循环递归对节点进行依次对比，算法复杂度达到 O(n^3)，n就是指代的节点数量

2.但是在react当中，将 O(n^3) 复杂度的问题转换成 O(n) 复杂度。主要的出发点就是webdom的跨层级移动的场景其实非常少

3.react中的diff策略一般有以下几点：
- DOM 节点跨层级的移动操作特别少，可以忽略不计。
- 拥有相同类的两个组件将会生成相似的树形结构，拥有不同类的两个组件将会生成不同的树形结构。
- 对于同一层级的一组子节点，它们可以通过唯一 id 或者 key 进行区分。

4.react中diff一般分为以下三种
- tree diff
- component diff
- element diff

## react diff 采用通过分层求异的策略，具体的步骤拆分为下面3步,

### 1.Tree Diff
React 通过 updateDepth 对 Virtual DOM 树进行层级控制，在updateChildren方法中只会对同级的dom进行比较，如果发现节点不存在，就直接推出递归。

react只会简单的考虑同级节点的变化，对于不同层级只有创建和删除操作。所以出现跨层级移动的时候，并不会移动，而是会直接删除老的然后在新的地方重新新建

这也是react官方建议不要进行 DOM 节点跨层级的操作的原因。

### 2.Component Diff

### 3.element diff
设置唯一的key，对更新进行优化
当节点处于同级的时候，会有具体的三个操作：INSERT_MARKUP（插入）、MOVE_EXISTING（移动）和 REMOVE_NODE（删除）


_updateChildren中，通过对prev和next children进行比较，里头遍历了nextChildren，
如果prevchildren == nextchildren，就进行move操作
如果不相等，存在prevchildren的时候，进行删除操作，然后在最后进行新建操作


### V16中的变化
之前v15的时候react的重新diff的过程是不能中断的

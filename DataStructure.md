# 数据结构  
 
数据结构是数据元素相互之间存在一种或多种特定关系的集合。“结构”就是指数据元素之间存在的关系，分为逻辑结构和存储结构。  

工程师将实际问题转化为计算机指令的方法就是设计出数据结构，再施加以算法就行。  

## 一、逻辑结构  

简单的来说逻辑结构就是数据之间的关系，逻辑结构大概统一的可以分成两种：线性结构、非线性结构。  

线性结构：是一个有序数据元素的集合。 其中数据元素之间的关系是一对一的关系，即除了第一个和最后一个数据元素之外，其它数据元素都是首尾相接的。常用的线性结构有: 栈，队列，链表，线性表。  

非线性结构：各个数据元素不再保持在一个线性序列中，每个数据元素可能与零个或者多个其他数据元素发生联系。常见的非线性结构有 二维数组，树等。  

## 二、存储结构  

存储结构是逻辑结构用计算机语言的实现，常见的存储结构有顺序存储、链式存储、索引存储以及散列存储。  
+ 数组在内存中的位置是连续的，它就属于顺序存储；  
+ 链表是主动建立数据间的关联关系的，在内存中却不一定是连续的，它属于链式存储；  
+ 顺序和逻辑上都不存在顺序关系，但是可以通过一定的方式去访问它的哈希表，数据散列存储。  

## 三、树和二叉树  

### 1. 树  

树是一种非线性的数据结构，是n（n>=0)个结点的有限集。n=0时称为空树。在任意一颗非空树中：  
+ 有且仅有一个特定的称为根（Root）的结点；  
+ 当n>1时，其余结点可分为m(m>0)个互不相交的有限集T1、T2、…、Tn，其中每一个集合本身又是一棵树，并且称为根的子树。因此，树是递归定义的。  

  ![Alt](images/DataStructure/树.png)  

**节点的度** ： 一个节点含有的子树的个数称为该节点的度； 如上图： A的为6，B的为0，D的为1。  
**叶节点或终端节点** ： 度为0的节点称为叶节点； 如上图： B、 C、 H、 I...等节点为叶节点 非终端。  
**节点或分支节点** ： 度不为0的节点。  
**双亲节点或父节点** ： 若一个节点含有子节点，则这个节点称为其子节点的父节点； 如上图： A是B的父节点。  
**孩子节点或子节点** ： 一个节点含有的子树的根节点称为该节点的子节点； 如上图： B是A的孩子节点。  
**兄弟节点** ： 具有相同父节点的节点互称为兄弟节点； 如上图： B、 C是兄弟节点。  
**树的度** ： 一棵树中，最大的节点的度称为树的度； 如上图：树的度为6。  
**节点的层次** ： 从根开始定义起，根为第1层，根的子节点为第2层，以此类推。  
**树的高度或深度** ： 树中节点的最大层次； 如上图：树的高度为4。  
**堂兄弟节点** ： 双亲在同一层的节点互为堂兄弟；如上图： H、 I互为兄弟节点。  
**节点的祖先** ： 从根到该节点所经分支上的所有节点；如上图： A是所有节点的祖先。  
**子孙** ： 以某节点为根的子树中任一节点都称为该节点的子孙。如上图：所有节点都是A的子孙。  
**森林** ： 由m（m>0）棵互不相交的树的集合称为森林。  

**树的表示** ： 树结构相对线性表比较复杂，存储表示起来比较麻烦，既然保存值域，也要保存结点和结点之间的关系。树有很多种表示方式如：双亲表示法，孩子表示法、孩子双亲表示法以及孩子兄弟表示法等。  

+ **1) 双亲表示法 (顺序存储结构)**  

  优点：parent(tree, x)操作可以在常量时间内实现 

  缺点：求结点的孩子时需要遍历整个结构  

  用一组连续的存储空间来存储树的结点，同时在每个结点中附加一个指示器(整数域) ，用以指示双亲结点的位置(下标值) 。  

  ![Alt](images/DataStructure/树的双亲存储结构.png)  

  图所示是一棵树及其双亲表示的存储结构。这种存储结构利用了任一结点的父结点唯一的性质。可以方便地直接找到任一结点的父结点，但求结点的子结点时需要扫描整个数组。  

  ```javascript
  function ParentTree() {
      this.nodes = [];
  }
  ParentTree.prototype = {
      constructor: ParentTree,
      getDepth: function () {
          var maxDepth = 0;

          for (var i = 0; i < this.nodes.length; i++) {
              var dep = 0;
              for (var j = i; j >= 0; j = this.nodes[i].parent) dep++;
              if (dep > maxDepth) maxDepth = dep;
          }

          return maxDepth;
      }
  };
  function ParentTreeNode(data, parent) {
      // type: ParentTree
      this.data = data || null;
      // 双亲位置域 {Number}
      this.parent = parent || 0;
  }
  var pt = new ParentTree();
  pt.nodes.push(new ParentTreeNode('R', -1));
  pt.nodes.push(new ParentTreeNode('A', 0));
  pt.nodes.push(new ParentTreeNode('B', 0));
  pt.nodes.push(new ParentTreeNode('C', 0));
  pt.nodes.push(new ParentTreeNode('D', 1));
  pt.nodes.push(new ParentTreeNode('E', 1));
  pt.nodes.push(new ParentTreeNode('F', 3));
  pt.nodes.push(new ParentTreeNode('G', 6));
  pt.nodes.push(new ParentTreeNode('H', 6));
  pt.nodes.push(new ParentTreeNode('I', 6));
  ```  

+ **2) 孩子链表表示法**  

    树中每个结点有多个指针域，每个指针指向其一棵子树的根结点。有两种结点结构。  

  + 1) 定长结点结构  
  
        指针域的数目就是树的度。  

        其特点是：链表结构简单，但指针域的浪费明显。结点结构如图所示。在一棵有n个结点，度为k的树中必有n(k-1)+1空指针域。  
        
        ![Alt](images/DataStructure/孩子表示法的定长结点结构.png)  

  + 2) 不定长结点结构  
  
        树中每个结点的指针域数量不同，是该结点的度，如图所示。没有多余的指针域，但操作不便。  
        
        ![Alt](images/DataStructure/孩子表示法的不定长结点结构.png)  

  + 3) 复合链表结构  
        对于树中的每个结点，其孩子结点用带头结点的单链表表示，表结点和头结点的结构如图所示。  
        n个结点的树有n个(孩子)单链表(叶子结点的孩子链表为空)，而n个头结点又组成一个线性表且以顺序存储结构表示。  

        ![Alt](images/DataStructure/孩子链表结点结构.png)  

        ![Alt](images/DataStructure/树T的孩子链表存储结构.png)  

        ```javascript
        function ChildTree() {
            this.nodes = [];
        }
        ChildTree.prototype = {
            constructor: ChildTree,
            getDepth: function () {
                var self = this;
                return function subDepth(rootIndex) {
                    if (!self.nodes[rootIndex]) return 1;

                    for (var sd = 1, p = self.nodes[rootIndex]; p; p = p.next) {
                        var d = subDepth(p.child);
                        if (d > sd) sd = d;
                    }

                    return sd + 1;
                }(this.data[0]);
            }
        };
        /**
        *
        * @param {*} data
        * @param {ChildTreeNode} firstChild 孩子链表头指针
        * @constructor
        */
        function ChildTreeBox(data, firstChild) {
            this.data = data;
            this.firstChild = firstChild;
        }
        /**
        * 孩子结点
        *
        * @param {Number} child
        * @param {ChildTreeNode} next
        * @constructor
        */
        function ChildTreeNode(child, next) {
            this.child = child;
            this.next = next;
        }
        ```  
        孩子表示法便于涉及孩子的操作的实现，但不适用于parent操作。  

+ **2) 孩子兄弟表示法(二叉树表示法)**  

    以二叉链表作为树的存储结构。  

    ![Alt](images/DataStructure/树及孩子兄弟存储结构.png)  

    两个指针域：分别指向结点的第一个子结点和下一个兄弟结点。结点类型定义如下：  

    ```javascript
    // 孩子兄弟表示法(二叉树表示法)
    // 可增设一个parent域实现parent操作
    function ChildSiblingTree(data) {
        this.data = data || null;
        this.firstChild = null;
        this.nextSibling = null;
    }
    ChildSiblingTree.prototype = {
        // 输出孩子兄弟链表表示的树的各边
        print: function print() {
            for (var child = this.firstChild; child; child = child.nextSibling) {
                console.log('%c %c', this.data, child.data);
                print.call(child);
            }
        },
        // 求孩子兄弟链表表示的树的叶子数目
        leafCount: function leafCount() {
            if (!this.firstChild) return 1;
            else {
                var count = 0;
                for (var child = this.firstChild; child; child = child.nextSibling) {
                    count += leafCount.call(child);
                }
                return count;
            }
        },
        // 求树的度
        getDegree: function getDegree() {
            if (!this.firstChild) return 0;
            else {
                var degree = 0;
                for (var p = this.firstChild; p; p = p.nextSibling) degree++;

                for (p = this.firstChild; p; p = p.nextSibling) {
                    var d = getDegree.call(p);
                    if (d > degree) degree = d;
                }

                return degree;
            }
        },
        getDepth: function getDepth() {
            if (this === global) return 0;
            else {
                for (var maxd = 0, p = this.firstChild; p; p = p.nextSibling) {
                    var d = getDepth.call(p);
                    if (d > maxd) maxd = d;
                }

                return maxd + 1;
            }
        }
    };
    ```  

```javascript

```  
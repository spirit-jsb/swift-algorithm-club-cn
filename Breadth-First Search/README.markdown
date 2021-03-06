# Breadth-First Search
# 广度优先搜索(BFS)

> This topic has been tutorialized [here](https://www.raywenderlich.com/155801/swift-algorithm-club-swift-breadth-first-search)
> 这个话题已经有个辅导[文章](https://www.raywenderlich.com/155801/swift-algorithm-club-swift-breadth-first-search)


Breadth-first search (BFS) is an algorithm for traversing or searching [tree](../Tree/) or [graph](../Graph/) data structures. It starts at a source node and explores the immediate neighbor nodes first, before moving to the next level neighbors.
广度优先搜索（BFS）是用于遍历或搜索[树](../Tree/)或[图](../Graph/)数据结构的算法。 它从源节点开始，在移动到下一级邻居之前首先探索直接邻居节点。

Breadth-first search can be used on both directed and undirected graphs.
广度优先搜索可以用于有向图和无向图。

## Animated example
## 动画示例

Here's how breadth-first search works on a graph:
以下是广度优先搜索在图上的工作原理：

![Animated example of a breadth-first search](Images/AnimatedExample.gif)

When we visit a node, we color it black. We also put its neighbor nodes into a [queue](../Queue/). In the animation the nodes that are enqueued but not visited yet are shown in gray.
当我们访问节点时，我们将其着色为黑色。 我们还将其邻居节点放入[queue](../Queue/)。 在动画中，排队但尚未访问的节点以灰色显示。

Let's follow the animated example. We start with the source node `A` and add it to a queue. In the animation this is shown as node `A` becoming gray.
让我们按照动画示例进行操作。 我们从源节点`A`开始，并将其添加到队列中。 在动画中，这显示为节点`A`变为灰色。

```swift
queue.enqueue(A)
```

The queue is now `[ A ]`. The idea is that, as long as there are nodes in the queue, we visit the node that's at the front of the queue, and enqueue its immediate neighbor nodes if they have not been visited yet.
队列现在是`[A]`。 我们的想法是，只要队列中有节点，我们就会访问位于队列前端的节点，如果尚未访问它们，则将其直接邻居节点入队。

To start traversing the graph, we pull the first node off the queue, `A`, and color it black. Then we enqueue its two neighbor nodes `B` and `C`. This colors them gray.
要开始遍历图形，我们将第一个节点从队列中拉出`A`，并将其着色为黑色。 然后我们将它的两个邻居节点`B`和`C`排队。 这会将它们变成灰色。

```swift
queue.dequeue()   // A
queue.enqueue(B)
queue.enqueue(C)
```

The queue is now `[ B, C ]`. We dequeue `B`, and enqueue `B`'s neighbor nodes `D` and `E`.
队列现在是`[B，C]`。 我们将`B`出列，并将`B`的邻居节点`D`和`E`排入队列。

```swift
queue.dequeue()   // B
queue.enqueue(D)
queue.enqueue(E)
```

The queue is now `[ C, D, E ]`. Dequeue `C`, and enqueue `C`'s neighbor nodes `F` and `G`.
队列现在是`[C，D，E]`。 将`C`出列，并将`C`的邻居节点`F`和`G`排队。

```swift
queue.dequeue()   // C
queue.enqueue(F)
queue.enqueue(G)
```

The queue is now `[ D, E, F, G ]`. Dequeue `D`, which has no neighbor nodes.
队列现在是`[D，E，F，G]`。 出列`D`，没有邻居节点。

```swift
queue.dequeue()   // D
```

The queue is now `[ E, F, G ]`. Dequeue `E` and enqueue its single neighbor node `H`. Note that `B` is also a neighbor for `E` but we've already visited `B`, so we're not adding it to the queue again.
队列现在是`[E，F，G]`。 将`E`出列并将其单个邻居节点`H`排队。 注意`B`也是`E`的邻居，但我们已经访问了`B`，所以我们不再将它添加到队列中。

```swift
queue.dequeue()   // E
queue.enqueue(H)
```

The queue is now `[ F, G, H ]`. Dequeue `F`, which has no unvisited neighbor nodes.
队列现在是`[F，G，H]`。 出队`F`，没有未访问的邻居节点。

```swift
queue.dequeue()   // F
```

The queue is now `[ G, H ]`. Dequeue `G`, which has no unvisited neighbor nodes.
队列现在是`[G，H]`。 出列`G`，它没有未访问的邻居节点。

```swift
queue.dequeue()   // G
```

The queue is now `[ H ]`. Dequeue `H`, which has no unvisited neighbor nodes.
队列现在是`[H]`。 出列`H`，它没有未访问的邻居节点。

```swift
queue.dequeue()   // H
```

The queue is now empty, meaning that all nodes have been explored. The order in which the nodes were explored is `A`, `B`, `C`, `D`, `E`, `F`, `G`, `H`.
队列现在为空，这意味着已经探索了所有节点。 探索节点的顺序是`A`，`B`，`C`，`D`，`E`，`F`，`G`，`H`。

We can show this as a tree:
我们可以将其显示为树：

![The BFS tree](Images/TraversalTree.png)

The parent of a node is the one that "discovered" that node. The root of the tree is the node you started the breadth-first search from.
节点的父节点是"发现"该节点的节点。 树的根是您从广度优先搜索开始的节点。

For an unweighted graph, this tree defines a shortest path from the starting node to every other node in the tree. So breadth-first search is one way to find the shortest path between two nodes in a graph.
对于未加权的图，此树定义从起始节点到树中每个其他节点的最短路径。 广度优先搜索是在图中找到两个节点之间的最短路径的一种方法。

## The code
## 代码

Simple implementation of breadth-first search using a queue:
使用队列简单实现广度优先搜索：

```swift
func breadthFirstSearch(_ graph: Graph, source: Node) -> [String] {
  var queue = Queue<Node>()
  queue.enqueue(source)

  var nodesExplored = [source.label]
  source.visited = true

  while let node = queue.dequeue() {
    for edge in node.neighbors {
      let neighborNode = edge.neighbor
      if !neighborNode.visited {
        queue.enqueue(neighborNode)
        neighborNode.visited = true
        nodesExplored.append(neighborNode.label)
      }
    }
  }

  return nodesExplored
}
```

While there are nodes in the queue, we visit the first one and then enqueue its immediate neighbors if they haven't been visited yet.
虽然队列中有节点，但我们访问第一个节点，然后如果它们尚未被访问则将其直接邻居排队。

Put this code in a playground and test it like so:
将此代码放在 playground 并像这样测试：

```swift
let graph = Graph()

let nodeA = graph.addNode("a")
let nodeB = graph.addNode("b")
let nodeC = graph.addNode("c")
let nodeD = graph.addNode("d")
let nodeE = graph.addNode("e")
let nodeF = graph.addNode("f")
let nodeG = graph.addNode("g")
let nodeH = graph.addNode("h")

graph.addEdge(nodeA, neighbor: nodeB)
graph.addEdge(nodeA, neighbor: nodeC)
graph.addEdge(nodeB, neighbor: nodeD)
graph.addEdge(nodeB, neighbor: nodeE)
graph.addEdge(nodeC, neighbor: nodeF)
graph.addEdge(nodeC, neighbor: nodeG)
graph.addEdge(nodeE, neighbor: nodeH)
graph.addEdge(nodeE, neighbor: nodeF)
graph.addEdge(nodeF, neighbor: nodeG)

let nodesExplored = breadthFirstSearch(graph, source: nodeA)
print(nodesExplored)
```

This will output: `["a", "b", "c", "d", "e", "f", "g", "h"]`
   
## What is BFS good for?
## BFS有什么用？

Breadth-first search can be used to solve many problems. A small selection:
广度优先搜索可用于解决许多问题。 一小部分选择：

* Computing the [shortest path](../Shortest%20Path%20(Unweighted)/) between a source node and each of the other nodes (only for unweighted graphs).
* 计算源节点和其他每个节点之间的[最短路径](../Shortest%20Path%20(Unweighted)/)（仅适用于未加权的图形）。
* Calculating the [minimum spanning tree](../Minimum%20Spanning%20Tree%20(Unweighted)/) on an unweighted graph.
* 在未加权的图表上计算[最小生成树](../Minimum%20Spanning%20Tree%20(Unweighted)/)。

*Written by [Chris Pilcher](https://github.com/chris-pilcher) and Matthijs Hollemans*

*作者：[Chris Pilcher](https://github.com/chris-pilcher)， Matthijs Hollemans*   
*翻译：[Andy Ron](https://github.com/andyRon)*  

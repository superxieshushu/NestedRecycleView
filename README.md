# NestedRecycleView
RecycleView 嵌套之后导致作为 item 的 RecycleView 所有的 ChildView 都会一次性初始化，item RecycleView 没有办法触发回收机制。嵌套的情况下响应滑动事件永远是最外层的 RecycleView （如果不会特殊处理）。

解决方案：

- 思路1, 从 LayoutManager 开始手动触发回收机制。从LayoutManager 里面onLayoutChildren 方法入手、手动干预RecycleView的布局过程; 根据RecycleView 的高度来进行对应的布局。从而避免不可见的View进行初始化操作。（**由于RecycleView的高度获取问题无法准备计算出可见区域的高度，暂时没有办法控制不可见区域的ChildView不进行View创建**）

- 思路2, 基于思路1, 可以把 Item RecycleView 的高度定死（例如300dp）; 然后解决滑动嵌套的问题。RecycleView 已经实现了NestedScrollingChild 这个接口，所以只要最外层的 RecycleView 再实现 NestedScrollingParent 接口从而实现嵌套 RecycleView 的滑动 Item RecycleView 高度固定可以上下滑动，滑动到尽头的时候，外层的RecycleView滑动。（**高度固定的前提下能实现、上下滑动、触发回收机制**）

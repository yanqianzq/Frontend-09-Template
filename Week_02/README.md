学习笔记
###1、启发式寻路
用一个函数去判断map上的点扩展的优先级，让我们有目的的去寻找，数学家证明，只要启发式的函数它所使用的估值只要能够一定小于点到终点的路径，它就是一个一定能找到最优路径的这样一种启发式寻路。这种能找到最优路径的启发式寻路，在计算机领域，我们管它叫 A* ；而不一定能找到最优路径的启发式寻路叫 A 。A*即A寻路的一个特例。

我们只需要把这个先进先出的queue变成一个以一定的优先级来提供点的这样的一个数据结构就行。

Sorted数据结构，只需保证take取出的是最小值即可。如：winner tree、数组、二叉树、heap堆数据结构


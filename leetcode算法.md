leetcode算法：

单调栈问题：[leetcode 84](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/), [leetcode 85](https://leetcode-cn.com/problems/maximal-rectangle/)

使用两个数组记录每个点左侧和右侧第一个不满足条件的点的位置，初始化时左侧为-1，右侧为size。遍历数组，如果大于栈顶就压栈，不满足条件，那么这个元素就是栈顶元素的第一个不满足条件的值，因此记录栈顶元素的左侧/右侧值，并将其弹出，直到满足条件，当前遍历的元素可以压栈为止。

背包问题：[leetcode 416](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

背包问题是指，一共N件物品，每件物品的重量为w[i]，价值为v[i]，在总重量不超过背包上限w的情况下，装入背包的最大价值。

背包问题分为0-1背包问题与完全背包问题，0-1背包是指物品只能选择一次，完全背包是指物品数量无上限。

前缀和： 

计算区间和问题时使用前缀和，利用一个数组f记录当前值，另一个数组s计算前缀和，即``s[i] = f[1] +... + f[i]``，通常从1开始，计算区间[i, j]的和``f[i] + f[i + 1] +...+f[j]``，可以使用``s[j] - s[i - 1]``，计算得到，有效降低时间复杂度。


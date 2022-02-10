#### [547. 省份数量](https://leetcode-cn.com/problems/number-of-provinces/)

有 `n` 个城市，其中一些彼此相连，另一些没有相连。如果城市 `a` 与城市 `b` 直接相连，且城市 `b` 与城市 `c` 直接相连，那么城市 `a` 与城市 `c` 间接相连。

**省份** 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 `n x n` 的矩阵 `isConnected` ，其中 `isConnected[i][j] = 1` 表示第 `i` 个城市和第 `j` 个城市直接相连，而 `isConnected[i][j] = 0` 表示二者不直接相连。

返回矩阵中 **省份** 的数量。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

```
输入：isConnected = [[1,1,0],[1,1,0],[0,0,1]]
输出：2
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

```
输入：isConnected = [[1,0,0],[0,1,0],[0,0,1]]
输出：3
```

 

**提示：**

- `1 <= n <= 200`
- `n == isConnected.length`
- `n == isConnected[i].length`
- `isConnected[i][j]` 为 `1` 或 `0`
- `isConnected[i][i] == 1`
- `isConnected[i][j] == isConnected[j][i]`





## 解析

* 问题原型 ：解决连通分支数

* 解决策略：

   1. DFS/BFS：

      ​		每一个节点为起点，进行深度/广度搜索标记，只要统计能作为起点个数即可，因为互通的节点都会被之前的节点标记。

      部分代码如下：

      ### DFS

      ```c++
      for(int i=0;i<n;i++)
      {
          if(判断i是否未被访问过)
          {
              ans++;
          }
          dfs(i);// 	
      }     
      ```

      ### BFS

      ```c++	
      for(int i=0;i<isConnected.size();i++)
      {
          if(判断i是否未被访问过)
          {
              ans++;
              q.push(i);
              v[i]=1;
              while(!q.empty())
              {
                  int temp=q.front();
                  for(int j=0;j<isConnected.size();j++)
                  {
                      if(!v[j] && isConnected[temp][j])
                      {
                          q.push(j);
                          v[j]=1;//放进来就进行标记
                      }
                  }
                  q.pop();
              }
          }
      }
      ```

      

   2. **并查集**：

      ​		经典并查集的运用。

      
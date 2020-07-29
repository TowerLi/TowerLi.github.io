---
title: DijkstraAlgorithm
date: 2020-07-09 09:30:27
tags:
---

# 1 . 题目

成研访客

描述：访客来到成研办公场地，需要找到具体的到访区域。现各个区域间可到达关系已知、耗时已知，请为访客设计一条最省时的路径。例如，访客需访问普惠金融组，可选择多种路径：大厅->二楼电梯厅->B区->A区，大厅->二楼电梯厅->E区->D区->中部电梯->A区，大厅->二楼电梯厅->E区->D区->C区->A区等，其中，选择大厅->二楼电梯厅->B区->A区最快。

输入1：节点个数，节点按照字母顺序编号。

输入2：字符串表示，可达节点间耗时。

输入3：输入为访客当前位置。

输入4：访客目的地。

输出：输入最短耗时。如无路径输出字符串NONE。

示例：

输入1:8

输入2:a->b:4,b->h:1,b->e:2, e->d:1, h->g:1, h->c:2, g->f:2, g->c:5, f->c:2, c->d:1

输入3：a

输入4：d

输出：abed

解释：从a到d共有4种走法：a->b->e->d，耗时7；a->b->h->g->c->d，耗时9；a->b->h->g->f->c->d，耗时11；a->b->h->c->d，耗时8。可知第一种走法耗时最短，输出abed。

# 2. 代码

```
package com.lihaitao.algorithm;

import Java.util.Arrays;

/**
* Created by Administrator on 2020/7/8.
*/
public class ChengYanVisitor {
   public static int MAXV = 1000000;
   public static String getShortestPath(int iNodes, String strInitPath, String strStart, String strEnd){
       boolean[] visted = new boolean[iNodes];
       //构建邻接矩阵
       int[][] matrix = new int[iNodes][iNodes];
       for (int i = 0; i < iNodes; i++ ) {
           Arrays.fill(matrix[i],MAXV);
       }
       //最短距离表
       int[] dis = new int[iNodes];
       Arrays.fill(dis,MAXV);
       //前驱节点
       int[] pre  = new int[iNodes];
       //邻接矩阵初始化
       String[] path = strInitPath.split(",");
       for (int i = 0 ; i < path.length; i++) {
           matrix[path[i].charAt(0)-'a'][path[i].charAt(3)-'a'] = Integer.valueOf(path[i].charAt(5)-'0');
       }
       for (int i = 0 ; i < iNodes; i++) {
           System.out.println("matrix is " + Arrays.toString(matrix[i]));
       }
       dis[Integer.valueOf(strStart.toCharArray()[0]-'a')] = 0;
       for (int i = 0; i < iNodes; i++) {
           //初始状态每个点前驱为自身
           pre[i] = i;
       }
       System.out.println("pre first is " + Arrays.toString(pre));
       for (int i = 0; i < iNodes; i++) {
           int u = -1, minn = MAXV;
           for (int j = 0; j < iNodes; j++) {
               if (visted[j] == false && dis[j] < minn) {
                   u = j;
                   minn = dis[j];
               }
           }
           if (u == -1) return "NONE";
           visted[u] = true;
           for (int v = 0; v < iNodes; v++) {
               if (visted[v] == false && dis[u] + matrix[u][v] < dis[v]) {
                   dis[v] = dis[u] + matrix[u][v];
                   pre[v] = u;
               }
           }
       }
       System.out.println("pre is " + Arrays.toString(pre));
       System.out.println("dis is " + Arrays.toString(dis));
       if (dis[Integer.valueOf(strEnd.toCharArray()[0]-'a')] != MAXV) {
           int i = Integer.valueOf(strEnd.toCharArray()[0]-'a');
           StringBuilder ans = new StringBuilder();
           ans.append(String.valueOf((char)(strEnd.toCharArray()[0])));
           while (pre[i] != Integer.valueOf(strStart.toCharArray()[0]-'a')) {
               ans.append(String.valueOf((char)(pre[i]+'a')));
               i = pre[i];
           }
           ans.append(String.valueOf((char)(pre[i]+'a')));
           return ans.reverse().toString();
       }
       return "NONE";
   }


   public static void main(String[] args) {
       ChengYanVisitor cy = new ChengYanVisitor();
       int iNodes;
       String strInitPath;
       String strStart;
       String strEnd;
       iNodes = 8;
       strInitPath = "a->b:4,b->h:1,b->e:2,e->d:1,h->g:1,h->c:2,g->f:2,g->c:5,f->c:2,c->d:1";
       strStart = "a";
       strEnd = "d";
       System.out.println("ans is " + cy.getShortestPath(iNodes,strInitPath,strStart,strEnd));
   }
}
```



# 3.  可参考此图理解 

![avatar](./DijkstraAlgorithm/clipboard.png)



| vertex | shortest distance from a | previous |
| ------ | ------------------------ | -------- |
| A      | 0                        |          |
| B      | ∞                        |          |
| C      | ∞                        |          |
| D      | ∞                        |          |
| E      | ∞                        |          |

 请填充此表，就可以得出答案~
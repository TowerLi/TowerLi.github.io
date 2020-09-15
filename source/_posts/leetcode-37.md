---
title: leetcode_37
tags: [算法]
qrcode: false
date: 2020-09-15 12:47:40
---

#  Leetcode 37 解数独

##  不多BB，上题目

[37.解数独](https://leetcode-cn.com/problems/sudoku-solver/) 难度 **困难**

编写一个程序，通过已填充的空格来解决数独问题。

一个数独的解法需遵循如下规则：

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。
空白格用 '.' 表示。

![img](http://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

​																					一个数独。

![img](http://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)

​																				答案被标成红色。

**Note:**

- 给定的数独序列只包含数字 `1-9` 和字符 `'.'` 。
- 你可以假设给定的数独只有唯一解。
- 给定数独永远是 `9x9` 形式的。

## 想法与思路

​        这题拿到手，只能说日了狗了，平时自己动手做数独都要死了，还要写程序去填充整个数独，这怎么玩？ 好在计算机就是牛逼，可以帮我思考，帮我计算，我们只需要告诉他们（计算机），麻烦你们从第一行开始，一行一行地，从左到右，从上到下，直到搜索到最后一行，每次遇到空白格，你就试着往里面填数字1到9，给我搜到底就完事了，实际上时间复杂度也到了*O*(9^(9X9))，即最多有 9X9 个空白格，每个格子可以填 [1,9] 中的任意整数，就是暴力，给我炸。

​       什么数字可以填？或者说这个数字有没有什么约束？当然有，题目要求同一行，同一列，同一个格子都不能出现相同数字，所以我们搜索之前就要确定好哪一行，哪一列，哪一个格子已经出现了对应数字，在搜索的时候，这些已经填过数字的行、列、格子就要避免，在一次次尝试填入数字的时候，也要把新填入的数字对应的行、列、格子上的约束更新，告诉下一次搜索，这一行、列、格子被老子占了，你给我往后稍稍，别动老子的位置。

​	  你以为你这样子一行填完，下面的行按照你填好的行来填数独就一定不冲突？做梦吧，小老弟，如果你这一行填的，让下一行填不下去了，你就老老实实地给我恢复成原样，所有行、列、格子对应的约束都恢复，代表你不再占用了，然后格子里的数字也给老子恢复成空白，这一步就叫做回溯。

​	 搜索到什么时候我们认为结束了，没错，当你搜索完最后一行的时候，也就是行下标等于9，已经是搜索到第10行了，你说是不是搜完了？算法结束，我们下期见，( ^_^ )/~~拜拜。

## 代码

```
//记录行是否被使用
    int[][] rows = new int[9][10];
    //记录 列 是否被使用
    int[][] cols = new int[9][10];
    //记录每个格子 是否被使用
    int[][] boxes = new int[9][10];
    public void solveSudoku(char[][] board) {
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                if (board[i][j] != '.') {
                    int n = board[i][j] - '0';
                    int nr = i / 3;
                    int nc = j / 3;
                    rows[i][n] = 1;
                    cols[j][n] = 1;
                    boxes[nr * 3 + nc][n] = 1;
                }
            }
        }
        fill(board,0,0);
    }

    private boolean fill(char[][] board, int i, int j) {
        //找到了结果
        if (i == 9) {
            return true;
        }
        //下一个搜索列的坐标
        int nc = (j + 1) % 9;
        //下一个搜索行的坐标
        int nr = (nc == 0) ? i + 1 : i;
        //如果是数字，进行递归求解
        if (board[i][j] != '.') return fill(board,nr,nc);

        //穷举空白格，从1到9
        for(int digit = 1; digit <=9; digit++) {
            int br = i / 3;
            int bc = j / 3;
            int boxes_key = br * 3 + bc;
            //当且仅当该数字可用时再填入
            if (rows[i][digit] != 1 && cols[j][digit] != 1 && 
            boxes[boxes_key][digit] != 1) {
                rows[i][digit] = 1;
                cols[j][digit] = 1;
                boxes[boxes_key][digit] = 1;
                board[i][j] = (char) (digit + '0');
                //递归
                if (fill(board,nr,nc)) return true;
                //回溯
                board[i][j] = '.';
                rows[i][digit] = 0;
                cols[j][digit] = 0;
                boxes[boxes_key][digit] = 0;
            }
        }

        return false;
    }

    private void printBoard(char[][] board) {
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length;j++) {
                System.out.print(board[i][j] + " ");
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        Leetcode_37 leetcode_37 = new Leetcode_37();
        char[][] board = new char[][]{
                {'5', '3', '.', '.', '7', '.', '.', '.', '.'},
                {'6', '.', '.', '1', '9', '5', '.', '.', '.'},
                {'.', '9', '8', '.', '.', '.', '.', '6', '.'},
                {'8', '.', '.', '.', '6', '.', '.', '.', '3'},
                {'4', '.', '.', '8', '.', '3', '.', '.', '1'},
                {'7', '.', '.', '.', '2', '.', '.', '.', '6'},
                {'.', '6', '.', '.', '.', '.', '2', '8', '.'},
                {'.', '.', '.', '4', '1', '9', '.', '.', '5'},
                {'.', '.', '.', '.', '8', '.', '.', '7', '9'}
        };
        System.out.println("调用函数前的board: " );
        leetcode_37.printBoard(board);
        leetcode_37.solveSudoku(board);
        System.out.println("调用函数后的board: " );
        leetcode_37.printBoard(board);
    }
```


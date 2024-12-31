# python扫雷游戏！
这是一个好玩的python扫雷游戏，欢迎优化升级代码！
# 基本方法
运行python Minesweeper.py即可
# 代码分享
```python
import tkinter as tk
import random

class Minesweeper:
    def __init__(self, root, rows=10, cols=10, mines=10):
        self.root = root
        self.root.title("Minesweeper")
        
        self.rows = rows
        self.cols = cols
        self.mines = mines
        self.board = []
        self.buttons = []
        self.mine_positions = set()
        self.flags = set()
        self.game_over = False

        self.create_board()
        self.place_mines()
        self.calculate_neighbors()

    def create_board(self):
        for r in range(self.rows):
            row = []
            button_row = []
            for c in range(self.cols):
                cell = {
                    "mine": False,
                    "neighbors": 0,
                    "revealed": False
                }
                row.append(cell)

                button = tk.Button(self.root, width=3, height=1, command=lambda r=r, c=c: self.reveal_cell(r, c))
                button.bind("<Button-3>", lambda e, r=r, c=c: self.toggle_flag(r, c))
                button.grid(row=r, column=c)
                button_row.append(button)
            self.board.append(row)
            self.buttons.append(button_row)

    def place_mines(self):
        while len(self.mine_positions) < self.mines:
            r = random.randint(0, self.rows - 1)
            c = random.randint(0, self.cols - 1)
            if (r, c) not in self.mine_positions:
                self.board[r][c]["mine"] = True
                self.mine_positions.add((r, c))

    def calculate_neighbors(self):
        for r in range(self.rows):
            for c in range(self.cols):
                if not self.board[r][c]["mine"]:
                    self.board[r][c]["neighbors"] = self.count_mines(r, c)

    def count_mines(self, row, col):
        count = 0
        for r in range(max(0, row - 1), min(self.rows, row + 2)):
            for c in range(max(0, col - 1), min(self.cols, col + 2)):
                if self.board[r][c]["mine"]:
                    count += 1
        return count

    def reveal_cell(self, row, col):
        if self.game_over or self.board[row][col]["revealed"] or (row, col) in self.flags:
            return

        self.board[row][col]["revealed"] = True
        self.buttons[row][col].config(text=str(self.board[row][col]["neighbors"]), state=tk.DISABLED, relief=tk.SUNKEN)

        if self.board[row][col]["mine"]:
            self.buttons[row][col].config(text="*", bg="red")
            self.game_over = True
            self.reveal_all_mines()
            tk.messagebox.showinfo("Game Over", "You clicked on a mine!")
        elif self.board[row][col]["neighbors"] == 0:
            self.expand_reveal(row, col)

    def expand_reveal(self, row, col):
        for r in range(max(0, row - 1), min(self.rows, row + 2)):
            for c in range(max(0, col - 1), min(self.cols, col + 2)):
                if not self.board[r][c]["revealed"]:
                    self.reveal_cell(r, c)

    def toggle_flag(self, row, col):
        if self.game_over or self.board[row][col]["revealed"]:
            return

        if (row, col) in self.flags:
            self.flags.remove((row, col))
            self.buttons[row][col].config(text="")
        else:
            self.flags.add((row, col))
            self.buttons[row][col].config(text="F")

    def reveal_all_mines(self):
        for (r, c) in self.mine_positions:
            if not self.board[r][c]["revealed"]:
                self.buttons[r][c].config(text="*", bg="red")

if __name__ == "__main__":
    root = tk.Tk()
    game = Minesweeper(root)
    root.mainloop()
```
# 屏幕截图
![github Logo](https://github.com/sun12yyds/python-Minesweeper/blob/main/ph/1.png)
# 打包思路
1.安装 PyInstaller：
确保你已经安装了。你可以使用以下命令通过来安装：
     
     pip install pyinstaller

2.pip install pyinstaller
编写 Python 程序：
确保你的Python程序（如上面的扫雷游戏代码）保存在一个文件中，例如。minesweeper.py

3.打包程序：
打开命令行终端，导航到保存的目录，然后运行以下命令：minesweeper.py

      pyinstaller --onefile --windowed minesweeper.py
      --onefile选项将所有文件打包到一个可执行文件中。
      --windowed选项确保在运行时不显示命令行窗口（适用于GUI应用程序）。
找到可执行文件：
PyInstaller 将创建一个目录，里面包含你的可执行文件。你可以在目录中找到它。distminesweeper.exedist

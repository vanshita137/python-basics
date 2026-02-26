import tkinter as tk
from tkinter import messagebox

CELL = 60
ANIMATION_DELAY = 40  # ms


class NQueensGame:
    def __init__(self, root):
        self.root = root
        self.root.title("N-Queens Game & Visualizer")

        self.n = 6
        self.board = [-1] * self.n
        self.animating = False

        self.top = tk.Frame(root)
        self.top.pack(pady=10)

        tk.Label(self.top, text="N:").pack(side=tk.LEFT)
        self.n_entry = tk.Entry(self.top, width=4)
        self.n_entry.insert(0, "6")
        self.n_entry.pack(side=tk.LEFT, padx=5)

        tk.Button(self.top, text="Replay", command=self.new_game).pack(side=tk.LEFT)
        tk.Button(self.top, text="Auto Solve", command=self.start_animation).pack(side=tk.LEFT)

        self.canvas = tk.Canvas(root)
        self.canvas.pack()

        self.canvas.bind("<Button-1>", self.handle_click)
        self.new_game()

    # ---------------- GAME SETUP ---------------- #

    def new_game(self):
        try:
            self.n = int(self.n_entry.get())
            if self.n < 4:
                raise ValueError
        except ValueError:
            messagebox.showerror("Error", "N must be ≥ 4")
            return

        self.board = [-1] * self.n
        self.animating = False

        size = self.n * CELL
        self.canvas.config(width=size, height=size)
        self.draw()

    # ---------------- PLAYER MODE ---------------- #

    def handle_click(self, event):
        if self.animating:
            return

        row = event.y // CELL
        col = event.x // CELL

        if row >= self.n or col >= self.n:
            return

        if self.board[row] == col:
            self.board[row] = -1
        else:
            if self.is_safe(row, col):
                self.board[row] = col
            else:
                return

        self.draw()

        if self.is_complete():
            messagebox.showinfo("Victory", "You solved N-Queens!")

    def is_complete(self):
        return -1 not in self.board and self.validate_full()

    # ---------------- SOLVER ANIMATION ---------------- #

    def start_animation(self):
        self.board = [-1] * self.n
        self.animating = True
        self.solve_step(0)

    def solve_step(self, row):
        if row == self.n:
            self.animating = False
            return

        for col in range(self.n):
            if self.is_safe(row, col):
                self.board[row] = col
                self.draw()

                self.root.after(
                    ANIMATION_DELAY,
                    lambda r=row + 1: self.solve_step(r)
                )
                return

        # backtrack
        self.board[row] = -1
        self.draw()

        self.root.after(
            ANIMATION_DELAY,
            lambda r=row - 1: self.solve_step(r) if r >= 0 else None
        )

    # ---------------- LOGIC ---------------- #

    def is_safe(self, row, col):
        for r in range(row):
            c = self.board[r]
            if c == col or abs(c - col) == abs(r - row):
                return False
        return True

    def validate_full(self):
        for r in range(self.n):
            for r2 in range(r + 1, self.n):
                c1, c2 = self.board[r], self.board[r2]
                if c1 == c2 or abs(c1 - c2) == abs(r - r2):
                    return False
        return True

    # ---------------- DRAWING ---------------- #

    def draw(self):
        self.canvas.delete("all")

        for i in range(self.n):
            for j in range(self.n):
                x1, y1 = j * CELL, i * CELL
                x2, y2 = x1 + CELL, y1 + CELL
                color = "#f1f5f9" if (i + j) % 2 == 0 else "#334155"

                self.canvas.create_rectangle(
                    x1, y1, x2, y2,
                    fill=color,
                    outline="black"
                )

                if self.board[i] == j:
                    self.canvas.create_text(
                        x1 + CELL / 2,
                        y1 + CELL / 2,
                        text="♛",
                        font=("Arial", 30),
                        fill="red"
                    )


if __name__ == "__main__":
    root = tk.Tk()
    NQueensGame(root)
    root.mainloop()

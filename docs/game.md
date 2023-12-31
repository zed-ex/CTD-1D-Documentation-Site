### def __init__(self, master = None)
Initialise Game Tkinter frame.
```py
def __init__(self, master = None):
    # Initialize the frame
    tk.Frame.__init__(self, master)
    self.grid()
    self.master.title('CTD 1D Project Team 11C')

    # Create the main grid for the game
    self.main_grid = tk.Frame(
        self, 
        bg=c.GRID_COLOR, 
        bd=3, 
        width=400, 
        height=400
        )
    self.main_grid.grid(pady=(100, 0))
    self.make_GUI()
    self.start_game()

    # Bind arrow keys to game actions
    self.master.bind("<Left>", self.left)
    self.master.bind("<Right>", self.right)
    self.master.bind("<Up>", self.up)
    self.master.bind("<Down>", self.down)

```


### def make_GUI(self):
Initialise Game Tkinter frame and create Score Header.
```py
def make_GUI(self):   
    ### Make Grid Layout ###
    # Create grid cells
    self.cells = []
    for i in range(4):
        row = []
        for j in range(4):
            cell_frame = tk.Frame(
                self.main_grid,
                bg=c.EMPTY_CELL_COLOR,
                width=100,
                height=100
                )
                
            cell_frame.grid(
                row=i, 
                column=j, 
                padx=5, 
                pady=5
                )
                
            cell_number = tk.Label(
                self.main_grid, 
                bg=c.EMPTY_CELL_COLOR
                )
                
            cell_number.grid(
                row=i, 
                column=j
                )
            cell_data = {"frame": cell_frame, "number": cell_number}
            row.append(cell_data)
        self.cells.append(row)

    ### Make Score Header ###
    # Create score header
    score_frame = tk.Frame(self)
    score_frame.place(
        relx=0.5, 
        y=40, 
        anchor="center"
        )
    tk.Label(
        score_frame,
        text="SCORE",
        font=c.SCORE_LABEL_FONT).grid(
        row=0
        )
    self.score_label = tk.Label(
        score_frame, 
        text="0", 
        font=c.SCORE_FONT
        )
    self.score_label.grid(row=1)
```

### def start_game(self):
Start game logic.
```py
def start_game(self):
    ### Create Matrix of Zeroes ###
    self.matrix = [[0] * 4 for _ in range(4)]
    
    ### Fill 2 random cells with 2s
    row = random.randint(0, 3)
    col = random.randint(0, 3)
    self.matrix[row][col] = 2
    self.cells[row][col]["frame"].configure(bg=c.CELL_COLORS[2])
    self.cells[row][col]["number"].configure(
        bg=c.CELL_COLORS[2],
        fg=c.CELL_NUMBER_COLORS[2],
        font=c.CELL_NUMBER_FONTS[2],
        text="2"
        )
    while(self.matrix[row][col] != 0):
        row = random.randint(0, 3)
        col = random.randint(0, 3)
    self.matrix[row][col] = 2
    self.cells[row][col]["frame"].configure(bg=c.CELL_COLORS[2])
    self.cells[row][col]["number"].configure(
        bg=c.CELL_COLORS[2],
        fg=c.CELL_NUMBER_COLORS[2],
        font=c.CELL_NUMBER_FONTS[2],
        text="2"
        )

    # Initialize the score
    self.score = 0

```

### def stack(self)
Matrix Manipulation Function
``` py
def stack(self):
    # Stack non-zero values to the left
    new_matrix = [[0] * 4 for _ in range(4)]
    for i in range(4):
        fill_position = 0
        for j in range(4):
            if self.matrix[i][j] != 0:
                new_matrix[i][fill_position] = self.matrix[i][j]
                fill_position += 1
    self.matrix = new_matrix
```


### def combine(self)
Combine two tiles together and sum the values of the tiles.
``` py
def combine(self):
    # Combine adjacent equal values
    for i in range(4):
        for j in range(3):
            if self.matrix[i][j] != 0 and self.matrix[i][j] == self.matrix[i][j + 1]:
                self.matrix[i][j] *= 2
                self.matrix[i][j + 1] = 0
                self.score += self.matrix[i][j]
```

### def reverse(self)
Reverse function.
``` py
 def reverse(self):
    # Reverse the order of values in each row
    new_matrix = []
    for i in range(4):
        new_matrix.append([])
        for j in range(4):
            new_matrix[i].append(self.matrix[i][3 - j])
    self.matrix = new_matrix
```

### def transpose(self)
Transpose function.
``` py
def transpose(self):
    # Transpose the matrix
    new_matrix = [[0] * 4 for _ in range(4)]
    for i in range(4):
        for j in range(4):
            new_matrix[i][j] = self.matrix[j][i]
    self.matrix = new_matrix
```


### def add_new_title(self)
Add a new 2 or 4 tile randomly to an empty cell.
``` py
def add_new_tile(self):
    if any(0 in row for row in self.matrix):
        row = random.randint(0, 3)
        col = random.randint(0, 3)
        while(self.matrix[row][col] != 0):
            row = random.randint(0, 3)
            col = random.randint(0, 3)
        self.matrix[row][col] = random.choice([2, 4])
```


### def update_GUI(self)
Update the GUI to match the matrix.
``` py
def update_GUI(self):
    for i in range(4):
        for j in range(4):
            cell_value = self.matrix[i][j]
            if cell_value == 0:
                self.cells[i][j]["frame"].configure(bg=c.EMPTY_CELL_COLOR)
                self.cells[i][j]["number"].configure(
                    bg=c.EMPTY_CELL_COLOR, 
                    text=""
                    )
            else:
                self.cells[i][j]["frame"].configure(bg=c.CELL_COLORS[cell_value])
                self.cells[i][j]["number"].configure(
                    bg=c.CELL_COLORS[cell_value],
                    fg=c.CELL_NUMBER_COLORS[cell_value],
                    font=c.CELL_NUMBER_FONTS[cell_value],
                    text=str(cell_value)
                    )
    self.score_label.configure(text=self.score)        
    global scoreValue
    scoreValue = self.score_label.cget("text")
    self.update_idletasks()
```

### def left(self, event)
Left-arrow key function.
``` py
def left(self, event):
    # Move left
    self.stack()
    self.combine()
    self.stack()
    self.add_new_tile()
    self.update_GUI()
    self.game_over()
```

### def right(self, event)
Right-arrow key function.
``` py
def right(self, event):
    # Move right
    self.reverse()
    self.stack()
    self.combine()
    self.stack()
    self.reverse()
    self.add_new_tile()
    self.update_GUI()
    self.game_over()
```

### def up(self, event)
Up-arrow key function.
``` py
def up(self, event):
    # Move up
    self.transpose()
    self.stack()
    self.combine()
    self.stack()
    self.transpose()
    self.add_new_tile()
    self.update_GUI()
    self.game_over()
```

### def down(self, event)
Down-arrow key function.
``` py
def down(self, event):
    # Move down
    self.transpose()
    self.reverse()        
    self.stack()
    self.combine()
    self.stack()
    self.reverse()
    self.transpose()
    self.add_new_tile()
    self.update_GUI()
    self.game_over()
```


### def horizontal_move_exists(self)
Check if any valid horizontal moves exist.
``` py
def horizontal_move_exists(self):
    for i in range(4):
        for j in range(3):
            if self.matrix[i][j] == self.matrix[i][j + 1]:
                return True
    return False
```

### def vertical_move_exists(self)
Check if any valid vertical moves exist.
``` py
def vertical_move_exists(self):
    for i in range(3):
        for j in range(4):
            if self.matrix[i][j] == self.matrix[i + 1][j]:
                return True
    return False
```

### def game_over(self)
Check if game is over (win/lose).
``` py
def game_over(self):
    if any(2048 in row for row in self.matrix):
        game_over_frame = tk.Frame(self.main_grid, borderwidth=2)
        game_over_frame.place(relx=0.5, rely=0.5, anchor="center")
        tk.Button(
            game_over_frame,
            text="You Win!",
            bg=c.WINNER_BG,
            fg=c.GAME_OVER_FONT_COLOR,
            font=c.GAME_OVER_FONT,
            command=self.show_signup
            ).pack()
    elif not any(0 in row for row in self.matrix) and not self.horizontal_move_exists() and not self.vertical_move_exists():
        game_over_frame = tk.Frame(self.main_grid, borderwidth=2)
        game_over_frame.place(relx=0.5, rely=0.5, anchor="center")
        tk.Button(
            game_over_frame,
            text="Game Over!",
            bg=c.LOSER_BG,
            fg=c.GAME_OVER_FONT_COLOR,
            font=c.GAME_OVER_FONT,
            command=self.show_signup
            ).pack()
```

### def show_signup(self)
The method destroys the current window and opens up the signUp() Class as the new window.
``` py
def show_signup(self):
    # Display the sign-up window
    self.destroy()
    signup_window = signUp()
    signup_window.mainloop()
```


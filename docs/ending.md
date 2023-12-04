### def __init__(self, master = None)
Initialise Ending Tkinter frame.
``` py
def __init__(self, master = None):
    # Initialize the frame with background color and grid settings
    tk.Frame.__init__(self, master, bg='#CA6680')
    self.grid(sticky="nsew")
    self.master.columnconfigure(0, weight=1)
    self.master.rowconfigure(0, weight=1)
    self.master.geometry("500x500")
    self.master.title('CTD 1D Project Team 11C')
```
### tk.Label(param_&_values).place(positioning_params)
The Labels made are displayed to the user as follows: "Thank you for playing", "Credits", "Team member names + (student ID)"
``` py
    ### Labels for the ending screen
    self.label = tk.Label(
        self, 
        text="THANK YOU FOR PLAYING!", 
        font=c.SCORE_LABEL_FONT
        bg='#CA6680'
        ).place(relx=0.5, rely=0.05, anchor=tk.CENTER)
        
    self.label1 = tk.Label(
        self, 
        text="CREDITS:", 
        font=c.SCORE_LABEL_FONT
        bg='#CA6680'
        ).place(relx=0.5, rely=0.15, anchor=tk.CENTER)
    
    self.label2 = tk.Label(
        self, 
        text="1. Afrith Ahamed A. (1008109)\n\n 2. Darrell Lu Jun Qiang (1007857)\n\n 3. Khoo Li Cheng Dylan (1005088)\n\n 4. Tan Tian Kovan (1007519)\n\n 5. Varsha Ramesh (1008477)\n\n 6. Chong Zhi Xun (1008140)", 
        font=c.USERNAME_LABEL
        bg='#93B7BE'
        ).place(relx=0.5, rely=0.4, anchor=tk.CENTER)
```

### tk.Button(param_&_values).place(positioning_params)
self.play_agn_btn: This interface calls the method confirm_prompt() upon being pressed to open a new window to display the messagebox.askyesorno.

self.back_btn: This interface calls the method goBack() upon being pressed to transition to the previous UI window.
``` py
    ### Return to Game Btn with confirmation prompt
    self.play_agn_btn = tk.Button(
        self, 
        text="PLAY AGAIN?",
        font=c.NEXT_BTN,
        border=5,
        command=self.confirm_prompt
        ).place(relx=0.85, rely=0.71, anchor=tk.CENTER)
    
    ### Button to go back to the scores screen
    self.back_btn = tk.Button(
        self,
        text="< BACK",
        font=c.NEXT_BTN,
        border=5,
        command=self.goBack
        ).place(relx=0.15, rely=0.71, anchor=tk.CENTER)
```

### def confirm_prompt(self)
Messagebox to confirm if the user wishes to play again. If yes, the method show_game() will be called. If no, the system will close and return the user to the terminal.
``` py
# Method to display a confirmation prompt before playing again
def confirm_prompt(self):
    response = messagebox.askyesno("Play Again :))", "Are You Sure You Want To Play Again?")
    if (response == 1):
        self.show_game()
    elif (response == 0):
        sys.exit()
```

### def goBack(self)
The method destroys the current window and opens up the Scores() Class as the new window.
``` py
# Method to go back to the scores screen
def goBack(self):
    self.master.destroy()
    scores_window = Scores()
    scores_window.mainloop()
```

### def show_game(self)
The method destroys the current window and opens up the Game() Class as the new window.
``` py
# Method to start a new game
def show_game(self):
    self.master.destroy()
    ending_window = Game()
    ending_window.mainloop()
```

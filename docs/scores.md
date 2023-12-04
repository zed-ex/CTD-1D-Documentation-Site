### __init__(self, master = None)
Initialise Scores Tkinter frame.
``` py
def __init__(self, master = None):
    tk.Frame.__init__(self, master, bg='#8447FF')
    self.grid(sticky="nsew")
    self.master.columnconfigure(0, weight=1)
    self.master.rowconfigure(0, weight=1)
    self.master.geometry("500x500")
    self.master.title('CTD 1D Project Team 11C')
```

### self.selectionDropdown = tk.OptionMenu()
`options`: a list containing options available in the dropdown menu.

`self.selected_option`: `StringVar()` is used to track currently selected options in the dropdown menu.

`self.selectionDropdown = tk.OptionMenu()`: When an option tracked by `StringVar()` is selected, the method `show()` is called to make a query request to the database to retrieve the respective filtered entries of highest score to lowest or the most recent scores.  
``` py
    ### Dropdown Menu
    options = ["Leaderboard", "Recent Scores"]     

    self.selected_option = tk.StringVar(self)
    self.selected_option.set(options[0])

    self.selectionDropdown = tk.OptionMenu(
        self,
        self.selected_option,
        *options,
        command=self.show
        )
    self.selectionDropdown.config(font=c.SCORE_LABEL_FONT)
    self.selectionDropdown.place(relx=0.5, rely=0.05, anchor=tk.CENTER)
```

### self.display_scores = scrolledtext.ScrolledText()
The retrieved filtered entries are displayed inside the interface frame and its state is set to 'disabled' to prevent the user from altering the score entries on the interface.
``` py
    self.display_scores = scrolledtext.ScrolledText(
        self,
        wrap=tk.WORD,
        width=50, 
        height=15, 
        state='disabled',
        )
    self.display_scores.place(relx=0.5, rely=0.4, anchor=tk.CENTER)
```

### self.next_btn:
This interface calls the method `show_ending()` upon being pressed to transition to the next UI window.
``` py
    ### Next Btn 
    self.next_btn = tk.Button(
        self, 
        text="NEXT >",
        font=c.NEXT_BTN,
        border=5,
        command=self.show_ending
        ).place(relx=0.85, rely=0.75, anchor=tk.CENTER)
        
    self.show()
```

### def show(self, *args)
`leaderboard_content`: `db.fetch_high_scores()` is a method from the `database_interface.py` file that is used to query for the top 10 highest scores in the database. 

`recent_content`: `db.fetch_latest_scores()` is a method from the `database_interface.py` file that is used to query for the top 10 most recent scores in the database.

`self.display_scores.configure(state='normal')`: enables `display_scores` to be modified.
`self.display_scores.configure(state='disabled')`: disables `display_scores` to be modified.

`self.display_scores.delete(1.0, tk.END)`: clears the current content in `display_scores`

`self.display_scores.insert(tk.END, row_str + "\n\n")`: inserts a formatted string of each row in `display_scores`
``` py
def show(self, *args):
    selected_option = self.selected_option.get()
    leaderboard_content = db.fetch_high_scores()
    recent_content = db.fetch_latest_scores()
    content = leaderboard_content

    if (selected_option == "Leaderboard"):
        content = leaderboard_content
        self.selectionDropdown.configure(bg='#D7DAE5')
        self.display_scores.configure(bg='#D7DAE5')
    elif (selected_option == "Recent Scores"):
        content = recent_content
        self.selectionDropdown.configure(bg='#CEB992')
        self.display_scores.configure(bg='#CEB992')

    self.display_scores.configure(state='normal')
    self.display_scores.delete(1.0, tk.END)
    for row in content:
        row_str = str(row).strip('{}')
        self.display_scores.insert(tk.END, row_str + "\n\n")
    self.display_scores.configure(state='disabled')
```

### def show_ending(self)
The method destroys the current window and opens up the `Ending()` Class as the new window.
``` py
def show_ending(self):
    self.destroy()
    ending_window = Ending()
    ending_window.mainloop()
```

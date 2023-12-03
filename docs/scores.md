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
        
    self.display_scores = scrolledtext.ScrolledText(
        self,
        wrap=tk.WORD,
        width=50, 
        height=15, 
        state='disabled',
        )
    self.display_scores.place(relx=0.5, rely=0.4, anchor=tk.CENTER)
        
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
Show high scores.
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
Show Ending page.
``` py
def show_ending(self):
    self.destroy()
    ending_window = Ending()
    ending_window.mainloop()
```
### def __init__(self, master = None)
Initialise SignUp Tkinter frame.
``` py
 def __init__(self, master = None):
    tk.Frame.__init__(self, master, bg='#F2D0A4')
    self.grid(sticky="nsew")
    self.master.columnconfigure(0, weight=1)
    self.master.rowconfigure(0, weight=1)
    self.master.geometry("500x500")
    self.master.title('CTD 1D Project Team 11C')


    ### Label
    self.label = tk.Label(
        self, 
        text="Fill in to Record Your Score!", 
        font=c.SCORE_LABEL_FONT
        bg='#F7F7FF'
        ).place(relx=0.5, rely=0.05, anchor=tk.CENTER)


    ### Username Label
    self.entry_label = tk.Label(
        self, 
        text="Username:",
        font=c.USERNAME_LABEL
        bg='#F7F7FF'
        ).place(relx=0.21, rely=0.28)
    
        
    ### Username Entry
    validate_cmd = self.register(self.validate_username)

    self.user_entry = tk.Entry(self, validate="key", validatecommand=(validate_cmd, "%P"))
    self.user_entry.place(relx=0.5, rely=0.3, anchor=tk.CENTER)
        

    ### Submit Btn
    self.submit_btn = tk.Button(
        self,
        text="SUBMIT",
        font=c.SUBMIT_BTN,
        border=3,
        bg='#C03221', #92374D
        command=self.dbUpload
        )
    self.submit_btn.place(relx=0.72, rely=0.3, anchor=tk.CENTER)


    ### Next Btn 
    self.next_btn = tk.Button(
        self, 
        text="NEXT >",
        font=c.NEXT_BTN,
        border=5,
        command=self.show_scores
        ).place(relx=0.85, rely=0.70, anchor=tk.CENTER)
        
```

### def validate_username(self, new_value)
Check if username length is less than or equal 20.
``` py
def validate_username(self, new_value):
        return len(new_value) <= 20
```

### def dbUpload(self)
Upload score to database.
``` py
    def dbUpload(self):
        self.submit_btn.configure(state='disabled')
        username = self.user_entry.get().strip()
        db.update_score(username, scoreValue)
```

### def show_scores(self)
Show Scores page.
``` py
def show_scores(self):
        self.destroy()
        scores_window = Scores()
        scores_window.mainloop()
```
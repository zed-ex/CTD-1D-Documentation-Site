### def __init__(self, master = None)
Initialise SignUp Tkinter frame. 
``` py
 def __init__(self, master = None):
    # Initialize the frame
    tk.Frame.__init__(self, master, bg='#F2D0A4')
    self.grid(sticky="nsew")
    self.master.columnconfigure(0, weight=1)
    self.master.rowconfigure(0, weight=1)
    self.master.geometry("500x500")
    self.master.title('CTD 1D Project Team 11C')

```

### tk.Label(param_&_values).place(positioning_params)
Creation of customization texts to be displayed on the screen with access 
to the place method to achieve the desired position of the label element. 
``` py
    ### Label
    # Create sign-up page layout
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
```

### tk.Entry(param_&_values).place(positioning_params) and def validate_username(self, new_value)
Creation of user input texts to be displayed on the screen with access 
to the place method to achieve the desired position of the label element. 

The overall purpose of this code is to ensure that the user's input in the 
Entry widget `self.user_entry` is validated according to the rules defined in the 'validate_username' function.

`self.register(self.validate_username)`: Registers a validation function called `validate_username`. The register method is used to create a Tcl wrapper for a Python function, making it callable from Tcl (Tool Command Language).

`validate="key"`: Specifies that the validation should occur whenever a key is pressed. In other words, the validation function `validate_username` will be called each time a key is pressed in the Entry widget.

`validatecommand=(validate_cmd, "%P")`: Specifies the validation command to be executed. %P is a special substitution code that represents the value of the Entry widget if the edit is allowed. Here, it passes the current content of the Entry widget to the validation function.
``` py
    ### Username Entry
    validate_cmd = self.register(self.validate_username)

    self.user_entry = tk.Entry(self, validate="key", validatecommand=(validate_cmd, "%P"))
    self.user_entry.place(relx=0.5, rely=0.3, anchor=tk.CENTER)
``` 

### tk.Button(param_&_values).place(positioning_params)
`self.submit_btn`: This interface calls the method `dbUpload()` upon being pressed to upload the user's score and username to the database. It is immediately disabled upon submission of the entry to prevent users from submitting multiple scores with the same/different username.

`self.next_btn`: This interface calls the method `show_scores()` upon being pressed to transition to the next UI window.
``` py
    ### Create submit button
    self.submit_btn = tk.Button(
        self,
        text="SUBMIT",
        font=c.SUBMIT_BTN,
        border=3,
        bg='#C03221', #92374D
        command=self.dbUpload
        )
    self.submit_btn.place(relx=0.72, rely=0.3, anchor=tk.CENTER)


    ### Create next button 
    self.next_btn = tk.Button(
        self, 
        text="NEXT >",
        font=c.NEXT_BTN,
        border=5,
        command=self.show_scores
        ).place(relx=0.85, rely=0.70, anchor=tk.CENTER)
```

### def validate_username(self, new_value)
The function is expected to return a boolean value indicating whether the input is valid or not. If the input is valid, the edit is allowed; otherwise, the edit is rejected. Its purpose is to check if the username length is less than or equal to 20.
``` py
# Validate the length of the username
def validate_username(self, new_value):
        return len(new_value) <= 20
```

### def dbUpload(self)
The method changes the state of the button to disabled preventing further user entries and uploads the user's username and score to the database. 
The `.strip()` method is used to remove any excess spaces before and after the username input. 
``` py
    # Upload the score to the database
    def dbUpload(self):
        self.submit_btn.configure(state='disabled')
        username = self.user_entry.get().strip()
        db.update_score(username, scoreValue)
```

### def show_scores(self)
The method destroys the current window and opens up the `Scores()` Class as the new window.
``` py
# Show the scores window
def show_scores(self):
        self.destroy()
        scores_window = Scores()
        scores_window.mainloop()
```

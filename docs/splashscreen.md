### def __init__(self, master = None)
Initialise SplashScreen Tkinter frame.
``` py 
def __init__(self, master = None):
    # Initialize the frame
    tk.Frame.__init__(self, master)
    self.grid(sticky="nsew")
    self.master.geometry("600x600")
    self.master.configure(background="Black")
    self.master.title('CTD 1D Project Team 11C')    

    # Set up splash screen layout    
    width=400
    height=400  

    image = Image.open("1.jpg")
    resize_image = image.resize((width, height))
    img = ImageTk.PhotoImage(resize_image)
    label1 = tk.Label(self.master,image=img)
    label1.image = img
    label1.place(relx=0.5, rely=0.4, anchor=tk.CENTER)

   # Add "Play" button 
   self.bt1=tk.Button(
       self.master,
       text="Play",
       width=10,
        height=2,
        fg="black",
        font=("OCR A Extended", 14, "bold"),
        command=self.start
        )
    self.bt1.place(relx=0.15, rely=0.8, anchor=tk.CENTER)

    # Add "Exit" button        
    self.exit_button=tk.Button(
        self.master,
        text='Exit',
        font=('OCR A Extended', 14, "bold"), 
        bg='gold', 
        bd=5, 
        width=10,
        height=2,
        command=self.quit
        )
    self.exit_button.place(relx=0.85, rely=0.8, anchor=tk.CENTER)
```
### def start(self)
Start the game.
``` py
# Start the game when "Play" button is pressed
def start(self):
    self.master.destroy()
    start_window = Game()
    start_window.mainloop()
```

### def quit(self)
Quit the game.
```py
# Quit the application when "Exit" button is pressed
def quit(self):
    sys.exit()
```

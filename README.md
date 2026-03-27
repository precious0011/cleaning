Exception in Tkinter callback
Traceback (most recent call last):
  File "C:\Program Files\Python312\Lib\tkinter\__init__.py", line 1967, in __call__
    return self.func(*args)
           ^^^^^^^^^^^^^^^^
  File "\\dom.sandwell.ac.uk\Staff\Home\Cadbury & Sandwell Shared Folders\T-Level Digital OS Exam Files\tlevel-digital-os-11\Task 2\MY CODE\Task 2 version 1.py", line 129, in register
    open_dashboard()
  File "\\dom.sandwell.ac.uk\Staff\Home\Cadbury & Sandwell Shared Folders\T-Level Digital OS Exam Files\tlevel-digital-os-11\Task 2\MY CODE\Task 2 version 1.py", line 167, in open_dashboard
    milk = tk.PhotoImage(file="images/milk.png")
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Program Files\Python312\Lib\tkinter\__init__.py", line 4151, in __init__
    Image.__init__(self, 'photo', name, cnf, master, **kw)
  File "C:\Program Files\Python312\Lib\tkinter\__init__.py", line 4098, in __init__
    self.tk.call(('image', 'create', imgtype, name,) + options)
_tkinter.TclError: couldn't open "images/milk.png": no such file or directory

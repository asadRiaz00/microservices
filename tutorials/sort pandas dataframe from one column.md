+++
title = "How to make Tkinter Text widget read only"
date = 2019-11-24T13:32:59+01:00
draft = false
weight = 120
keywords = ["Tkinter Text widget", "Tkinter Text widget read only", "Tkinter make Text widget read-only", "how to make Tkinter Text widget read only"]
description = "This tutorial demonstrates how to make Tkinter Text widget read-only."
tags = ["Tkinter Text", "Tkinter"]
author = "Jinku Hu"
postlink = 12364981
inarticle = true

+++

We will introduce methods to make the Tkinter Text widget read-only,

1. Set the `Text` state to be `disable`
2. Bind any key press to the `break` function

## Set the `Text` State to `disable` to make Tkinter `Text` read only

The `Text` widget becomes read-only after its state is set to be `disable`.

```python
import tkinter as tk
root = tk.Tk()
readOnlyText = tk.Text(root)
readOnlyText.insert(1.0,"ABCDEF")
readOnlyText.configure(state='disabled')
readOnlyText.pack()

root.mainloop()
```

The default state of a `Text` widget is `NORMAL`, that means the user could edit, append, insert or edit text content in it.

```python
readOnlyText.configure(state='disabled')
```

<!--adsense-->

You need to change the `Text` widget state to `DISABLED` to make it read-only. Any attempt to change the text inside that widget will be silently ignored.

{{% warning title="Warning" %}}

You need to change the state from `disabled` to `normal` if you intend to update the `Text` widget content, otherwise, it keeps read-only.

{{% /warning %}}

## Bind any key press to the `break` function to make Tkinter `Text` read only

If we bind any key stroke to the function that only return `break` to the `Text` widget, we could get the same result that the `Text` becomes read-only.

```python
import tkinter as tk
root = tk.Tk()
readOnlyText = tk.Text(root)
readOnlyText.insert(1.0,"ABCDEF")
readOnlyText.bind("<Key>", lambda a: "break")
readOnlyText.pack()

root.mainloop()
```

<!--adsense-->

{{% warning title="Attention" %}}

The difference between this solution and the above solution is that the `CTRL+C` doesn't work here. It means you could neither edit the content nor copy it.

We need to make the exception of `CTRL+C` to the function binding to the `Text` if `CTRL+C` is desired.

{{% /warning %}}

```python
import tkinter as tk

def ctrlEvent(event):
    if(12==event.state and event.keysym=='c' ):
        return
    else:
        return "break"

root = tk.Tk()
readOnlyText = tk.Text(root)
readOnlyText.insert(1.0,"ABCDEF")
readOnlyText.bind("<Key>", lambda e: ctrlEvent(e))
readOnlyText.pack()

root.mainloop()
```

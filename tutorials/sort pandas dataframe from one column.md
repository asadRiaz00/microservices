+++
title = how to sort pandas dataframe by column"
date = 2019-11-24T13:32:59+01:00
draft = false
weight = 120
keywords = [""]
description = "This tutorial demonstrates  how to sort pandas dataframe by column."
tags = ["pandas", "Pandas Dataframe Sorting"]
author = "Jinku Hu"
postlink = 12364981
inarticle = true

+++

We will introduce methods to sort the dataframes records by column,

Consider the following dataframe

```python
import pandas as pd
df = pd.DataFrame({
    'col1': ['A', 'A', 'B', 'C', 'D', 'C'],
    'col2': [2, 1, 9, 8, 7, 4],
    'col3': [0, 1, 9, 4, 2, 3],
})
print(df)
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

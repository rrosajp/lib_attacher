# summary
This is a very simple (small) software library.  It is used only for one thing.

Suppose you are working in a project with a directory structure like this:

```
project_folder
|-scripts_folder
| |-python_libs
| | |-my_python_lib
| | | * __init__.py
|-sub_folder_1
| | * my_python_script.py
```

Suppose you want your python script `my_python_script.py` to consume the `my_python_lib` library.

Then, at the top of your script, you would put the following code.

```python
import os
import lib_attacher

# Get the folder containing your script file.
this_folder=os.path.dirname(os.path.realpath(__file__))

# Attach to the parent folder of your library
attached = lib_attacher.attach_folder_in_hierarchy(
    start_folder=this_folder,
    target_sub_path="scripts_folder/python_libs")

# Check to make sure it worked
if attached is None:
    raise Exception("Failed to find python_libs folder.")

# Now you can import your library.
import my_python_lib
```

If you subsequently move your `my_python_script.py` file around, it will still work.  For example,
it will also work from any of these locations:

```
project_folder
|-sub_folder_1
| |-sub_folder_2
| | | * my_python_script.py
```


```
project_folder
|-sub_folder_1
| |-sub_folder_2
| | |-sub_folder_3
| | | | * my_python_script.py
```

```
project_folder
| * my_python_script.py
```

If, by some chance, there are multiple `scripts_folder/python_libs` folders, you can specify the
`extra_sub_path` parameter to ensure that you match the target folder exactly (without ambiguity).

```python
# Attach to the parent folder of your library
attached = lib_attacher.attach_folder_in_hierarchy(
    start_folder=this_folder,
    target_sub_path="scripts_folder/python_libs",
    extra_sub_path="my_python_lib/__init__.py")
```
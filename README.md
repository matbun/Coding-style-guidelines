# Summary of the most useful tricks to keep in mind
 
## Tools 

Also markdown linters are really useful though!
To have a good documentation.
 
For Sublime Text:
 
* SublimeLinter (framework)
* SublimeLinter-pycodestyle (PEP8 linter)
* SublimeLinter-mdl (Markdown linter)

For VS code:

* Markdown linter: https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint
* `.md` to `.pdf`: https://marketplace.visualstudio.com/items?itemName=yzane.markdown-pdf
 
## Alignment
 
* Indent with 4 spaces (can be set in Sublime to substitute Tab automatically)
* Functions args: align with opening bracket
 
  ```
  my_fun(arg1,
         arg2)
  ```
 
## Coding
 
* `==` compares *by value*, whereas `is` *by reference* (`id(obj)`).
  Never do `var == None`. Use `var is None`.
* `dict[k]` ==> `dict.get(k)`: never throws exception (default None).
* `if(type("foo") is str)` ==> `if(isinstance("foo", str))`. `type()` is for printing.
* Encapsulate main code in `main()` function. Otherwise, when the code is imported
  the code not encapsulated in a function is executed. May cause unwanted behavior.
 
  ```
  if __name__ == "__main__":
    # This module was run from the CLI
 
  if __name__ == "module_name":
    # This module was imported from another module
  ```
 
* Avoid `"number of actions: " + str(12)`
 
## Performances
 
* Use `tuple` instead of `list` when possible.
* Use `Decimal` for precise `float`.
* If the size of a list is known *a priori*, allocate it instead of using the
  expensive `append()` method:
 
  ```
  l = [None] * 1000
  for i in range(1000):
    l[i] = "foo"
  ```
 
* Avoid `+= "my_str"` in loops. Use:
 
  ```
  tense = []
  for i in range(1000);
    tense.append("foo")
  full_str = ''.join(tense)
  ```
 
## Comments
 
* Docstrings
  * module (.py file)
  * methods & functions: description, Args, Returns, Raises
  * classes
* Align with code block (e.g. after the `if`)
 
## Classes
 
* Internal (private) methods and attributes with leading `_`.
* Methods order: magic (e.g. `__init__`, `__str__`), public, private.
* Immutable class attributes initialized before the `__init__`, if needed.
  Otherwise just type hinted with no initialization
* Use `@classmethod` to enable a method to access class attributes in a shared way
  for all instances of a class. When just one instance changes the value for a
  class attribute with a `@classmethod`, the value for that attribute whill be
  changed for all instances as well.
* Attributes with a leading `__` are not influenced by parent or child classes
  using `@classmethod` methods.
 
## Modules
 
* Explicitly define, if needed, the members that are imported from a module when
  doing `from mod import *`:
 
  ```
  __all__ = ["fun_1", "Class2"]
  ```
 
* Do not modify the `sys.path` list in the code to add a custom package to
  the PYTHONPATH. Look [here](https://stackoverflow.com/questions/19048732/python-setup-py-develop-vs-install) to know how to solve this.
 
  ```
  pip install -e . --user
  ```
 
  `--user` does not require `sudo` and `-e` is the installation that supports development
  in the meantime.
 
## Multi-line break
 
* Multi-line `if`: use parentheses (allow comments) and not backslash.
 
  ```
  if (a is True           # allow comment
        and b is False):  # allow comment
    print("ciao")
  ```
 
* Multi-line `import`: use parentheses again (allow comments)
 
  ```
  from mod import (fun_a, fun_b,  # comment 1
                   fun_c, fun_d)  # comment 2
  ```
 
* Multi-line strings (very long strings). Use parentheses as well
 
  ```
  s = ("this is my really, really, really, really, really, "  # comments ok
     "really long string that I'd like to shorten.")  # comment 2
  ```
 
* Multi-line ternary (very long strings). Use parentheses as well
 
  ```
  answer = (
    'Ten for that? You must be mad!' if does_not_haggle(brian)
    else "It's worth ten if it's worth a shekel.")
  ```
 
## Multiprocessing v. multithreading
 
* Due to Python GIL only one thread can run at a time. As a consequence threads
  are running **concurrently** (run a bit of thread1, than a bit of thread2... =>
  Reduces the *latency*) but not **in parallel** (run all threads at the same time
  on different cores of the processor => Increases the *speed*).
* Multithreading is good when the slow operation is slow due to I/O operations.
* Multiprocessing is good when the slow operation is slow due to CPU intensive tasks.

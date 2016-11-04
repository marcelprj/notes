# Intro

[Markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

# VIM && VIMDIFF

- set + [filetypes, hlsearch, ]

- verbose + cmd: to debug the command

- ./vim/ftdetect: is the directory that can contain the commands to recoginze file types

- drop lines that don't contain an expression: `v/<reg_expr>/d` like `v/Iteration.*loss/d`

- The replace directive `%s` operates using the sed-syntax. This can be used to extract fields using reg_expr by the following:
```bash
%s/<reg_expr>/\1, \2, <any_expr> \9/g
```
Note that the **g** at the end only indicates that the command (sed) should continue searching the rest of the line and not just skip to next line when a match is found. An example of this command is given below:
```bash
:%s/^.*Iteration \([.0-9]*\).*loss = \([.0-9e-]*\).*$/\1,\2 
```
Note that the characters **^** and **$** indicates the beginning and the end of the line respectively. These can be used to match characters at the beginnings and the end of each sentence. The **\\(**, **\\)** indicate field selection, while **\\#** refers to the field with the respective number.

- use `vim --version ?| grep <expr>` to see what options has vim been compiled with and where it looks for file

- use `set all` to see all possible setting parameters; ex. (`set number[nu]`/ `set nonumber[nonu]`)

- use `<CTRL+N>` for autocompletion within the file (great for variables), and `<CTRL+X>, <CTRL+F>` for path completetion filename omni completion.

- `se list` and `se nolist` show special characters and hide them respectively.

- yanking: `:Xy` yanks line number *X*, `:X,Ky` yanks lines from *X-K*, `<CTRL+R><">` pastes yanked lines to command line [link](http://stackoverflow.com/questions/3997078/how-to-paste-yanked-text-into-vim-command-line)

- searching across multiple lines: lot of special characters to help with that can be found here
[link](http://vim.wikia.com/wiki/Search_across_multiple_lines)




# Terminal

## bash scripting

-

## grep

- filter all files in dir_path  based on reg_expr: `grep reg_expr dir_path` like `grep Iteration.*loss`

- use `-i` to ignore case

- use `-R` or `-r` to do a recursive search into the dir_path

- use `-I` to ignore binary files

- use `--exclude=<expr>` or `--include=<expr>` to include or exclude certain file types


## sed

- uses the same syntax the VIM

- print result to screen: `-n`

- directly modify file: `sed -i <expr> file`

- drop files that don't match match1 OR match2: `sed '/match1\|match2/!d' file`. The **!** indicates to execute on the inverse of the condition.

- implement grep: `sed '/pattern/!d' file`. It has one advantage over grep that it allows for the `-i` condition that will modify the file in place instead of having to pipe it with grep.


## ps

- use the `ps -p $PID -o pid,vsz=MEMORY, -o user,group=GROUP -o comm,args=ARGS` command (or part of it) to get information about a processing with a given `$PID`.

# Python

- `locals()` can sometimes be useful to avoid using `eval` or `exec` for assigment. It returns a dict.

- Use argument unpacking to convert a `list` or `tuple` into a `namedtuple`:
```python
namedtuple = NamedTuple(*list(...))
```

- Named tuples are very useful to force an input array into a specific format. They also provide a `_asdict` method to cast them as dicts of named fields.

- String literal concatenation: multiple adjacent strings are allows and their meaning is the same as their concatenation. Especially useful in function calls or error/warning messages:
```python
multiline_str = ("batata"
		 "harra {}".format(i lov)")

eat("batata"
    "7arra")
``` 

## IPython

- IPython expands variables with `$name`, bash-style. This is true for **all magics**, like `%run`, `!`, ect...


## Pandas

- Split-Apply-Combine: split the DataFrame based on i) its index if using a key function, or ii) column if using a string. Then, we apply a function to each grou and combine using the *aggregate* commnad. Example: 

```python
bbox_loss_df.groupby(lambda x: x // 100, axis=0).aggregate(mean)
```
Here, the function applies the lambda on the index, takes the mean of each group and returns the results as a DataFrame.

# command line tricks
## chun li

---
# intro
- Tailored to `OS X` and command line beginners
- Intro to command line basics
- Some examples of common commands used
- Some of my other favourite tools
- Best advice in general: take the additional 15 minutes to learn how to do something via command line, in the long term it's worth it
- You will not be good at command line from this presentation, the only real way is by doing it
- A lot of this is just experience

---
# the command line
One reason the command line is such an efficient tool is because of the effectiveness of the `UNIX` philosophy:
1. Write programs that do one thing and do it well.
2. Write programs to work together.
3. Write programs to handle text streams, because that is a universal interface.

---
# the command line
<!-- some of these statements are false in zsh -->
All programs have `stdin`, `stdout`, and `stderr`, which can be redirected in the shell.
- You can make a command redirect its `stdout` to the next command's `stdin` via a pipe
	- This is done with the pipe character `|`
- You can also redirect `stdout` and `stderr` to a file using `>`/`2>` or `>>`/`2>>`, and redirect a file into `stdin` via `<`
- You can redirect into file descriptors, including `stdin` (0), `stdout` (1) and `stderr` (2) via `1>&2` or `>&2` <!-- no real example here... it's kind of not useful every day -->
- Lastly, `$(command)` allows you to substitute the output of `command` into the shell for execution.

---
# the command line
- These few simple tools make the shell very versatile and powerful
- For example:
`cat <file> | grep <search> # piping two commands`
`echo "hello world" > new.txt # outputing to a file`
`grep "log4j" < MyClass.java # file input`
`echo "Today is $(date +%Y-%m-%d)" # substitute the output of a command`

---
# common commands
Some other common operations done:
- `tail -f <log_file> | grep <search_term>` look for a search string in a log
- `find . -name <filename>` to look for a file
	- `find . -type f` lists all files, `-type d` lists all directories
- `xargs` is a very useful tool that takes in a command and executes it with `stdin` as the arguments.
	- Often piped in
	- ex. `find . -type f -name '*.java' | xargs grep 'searchstring'` find all files with extension `.java` and searches for the term `searchstring`.
		- Of course, you could always just use `rgrep`
<!-- Can invite to try and write a program to delete merged branches -->

---
# common commands
- `sed s/<search>/<replace>/g` lets you substitute and replace
	- `-i .bak` lets you edit files in place
	- ex. how would you replace `log4j` with `slf4j` in every Java file?
	<!-- find . -type f -name '*.java' | xargs sed -i '.bak' 's/log4j/slf4j/g' -->

---
# `vim`
- `vim` is a modal text editor
- Has 2 main modes: Normal mode to edit text (`esc`), and Insert mode to write (`i` or `a`)
- `:help <search_term>` is the most important vim command
	- If you don't know how to use help, `:help help`
	- If you *really* don't know how to use help, there's always Google
	- If you still need help, then may God have mercy on your soul
	- `<esc>:q!` is also an important vim command
	- It's right up there with `C-x C-c` for `emacs`

---
# `vim` navigation
- `^D` and `^U` to go up and down half a page
- `hjkl` to move around
- `w` and `b` for words forward and backwards
- `$` for end-of-line, `^` for the beginning
-  `gt<char>` to go to next char
-  `/<search_term>` for find
-  `:%s/<find>/<replace>/gc` for find and replace across whole file (see `:help substitute`)
-  These can also be used in the `less` pager

---
# `vim` navigation
- `y` to copy/yank, `d` to delete, `p` to paste, `x` to cut, `s` to substitute character
- `v/V` lets you get into visual mode to select blocks of text for editing
	- `C-v` allows you to go into visual block mode
-  How can I add `#` to the beginning of every line? <!-- :%s/^/#/g -->
- `q<letter>` allows you to bind a macro to `<letter>`
	- Type in the macro commands and hit q again to stop recording. Then you can hit `@<letter>` to replay the macro.
-  A lot of these tools have similar bindings; ex. `^` and `$` are used similarly in vim, regex, sed, less, e.t.c.

---
# `tmux` navigation
- `tmux` is the GUI of the command line
- Mainly useful for maintaining sessions  in `ssh` sessions and working with multiple terminals at once
- Am going to use my shortcuts for `tmux` for this demonstration
- `tmux` uses a prefix (`PR`), which you press before issuing a command so it knows you are talking to it and not your own shell.
	- By default it is `C-b`, but I like `C-a`
	- You can usually send the prefix signal to the shell by entering the prefix twice.

---
# `tmux`
- `tmux new` or `tmux new -s <session_name>` to start a new session
- `tmux attach -t <session_name>` to attach
- `tmux ls` to list all sessions
- `tmux kill-session -t <session_name>` to kill a session
- `<PR>-d` to detach from a session

---
# `tmux`
`<PR>-c` to create a new window
`<PR>-<space>` or `<PR>-n` for the next window
`<PR>-<backspace>` or `<PR>-p` for the previous window
`<PR>-<num>` to go to the `num`th window
`<PR>-,` to rename the window

---
# `tmux`
`<PR>-s` or `<PR>-"` to split panes horizontally
`<PR>-v` or `<PR>-%` to split panes vertically
`<PR>-hjkl` to move around panes
`tmux` has a concept of predefined layouts, which you can cycle through via `<PR>-<enter>`, or select via `<PR>-M-<num>`, where `num` is between 1-5.
`<PR>[` to copy, `<PR>]` to paste

---
# other useful tools
- `ag` is a faster, better `rgrep`
- `zsh` has a better interactive shell
	- lets you do things such as `ls ../**`, more plugins, smarter history
- There are tons of classic and new `UNIX` style tools such as `awk`, regexes. Also, more specialized tools such as:
	- `netstat` and `ifconfig` for networking
	- `top` and `ps` for process managment
	- `systemctl`, `service`, `init`, or whatever startup system you like
	- `units`, `wc`, `sort`, `head`, `cut`, `less`, `gzip`, `tar`, `diff`, `patch`, `apropos`, `man`, `who` and of course, `toilet` and `say`

---
# shell scripting 101
- A shell script is just a file with commands you would have typed in the terminal
- It always starts with a shebang `#!` followed by the program to execute the shell (usually `/bin/bash`)
- Variables are accessed via `${var_name}`. `$1` is the first argument, `$2` the second, e.t.c.
	- `$0` is the command name
	- `$@` is all the arguments
	- `$#` is the number of arguments
	- `$?` is the exit code of the last command run
- Variables are set via `<var_name>=<value>`
	- ex. `foo="bar" ; echo $foo`
	- `;` allows multiple bash statements in one line

---
# shell scripting
<!-- show ~/bin scripts as examples -->
- Shell scripting is useful for quickly automating tasks you would have done in the shell anyways
	- If you would not normally do the task via the shell, a scripting language such as `python` may be more appropriate
- `bash` can be a dangerous and very confusing language
	- ex. what will `$10` output in a script? Is there a difference from `${10}`?
	<!-- it outputs the first argument with 0 concatenated to the end -->
    - From my personal experience, the error messages are not the greatest
- There are a few tips to help keep yourself from blasting your foot off

---
# shell scripting
- Always start your script with `#!/bin/bash`
	- There *are* small differences, and although I consider `zsh` better, I still use `bash` for scripting as it is the most common and most available shell
- `set -e` will fail the script if any line fails
	- Note failed pipe commands will not trigger this, use `set -o pipefail`
- In general, you should try to quote values and strings
- Use `$(command)` instead of `command' as it is cleaner and requires less quoting
- Use `[[ ]]` over `[ ]`

```bash
if [[ -f /bin/bash ]] ; then
  # do something
fi
```

---
# that's all folks!

Thank you

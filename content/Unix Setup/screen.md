+++

Title = "GNU screen"

weight = 6

tags = ["screen","scripts"]

+++

GNU Screen is a terminal multiplexer, a software that can be used to multiplex several virtual consoles, allowing a user to access multiple separate login session inside of a single terminal window, or detach and reattach session from a terminal.

Following contains some useful config and `screenrc` for GNU screen.

##### Screen Config

edit or create `/etc/screenrc` or `~/.screenrc` and add below code. This code adds a nice status bar to the default screen.

````c++
autodetach on
startup_message off
hardstatus alwayslastline
shelltitle 'bash'

hardstatus string '%{wk}%?%-Lw%?%{=b kR}(%{W}%n*%f %t%?(%u)%?%{=b kR})%{= w}%?%+Lw%?%? %{g}]'
````

##### Screen Basics

Execute this command

```bash
screen
```

This will create a screen session with name pts-0.hostname.

To detach the screen press `Ctrl+a d`

Most of commands follow `Ctrl+a` as a prefix.

Now since we are detached from screen we can list all currently running screens using `screen -ls`

We can name a new screen created using this command.

```bash
screen -S cowscrem
```

This creates a screen session with name cowscream.

To reattach to a screen we can type 

```bash
screen -r <screen_pid> or screen -r <screen_name>
```



To send a command to screen session and execute a command

```bash
screen -X -S 12108 quit
```

sends kill signal to screen with `pid 12108`

Next Screen window : `Ctrl+a n`

Previous Screen window : `Ctrl+a p`

Kill screen window : `Ctrl+a k`

Create a new screen window : `Ctrl+a c`

Kill all windows : `Ctrl+a \`

List all windows : `Ctrl+a ""`

To rename the screen window `Ctrl+a A`

##### Splits :)

Vertical Split : `Ctrl+a |`

Horizontal Split: `Ctrl+a S`

Move to next pane : `Ctrl+a tab`

To open a window on next pane : `Ctrl+a <window_no>` or `Ctrl+a ""`

removing a pane : `Ctrl+a x`

##### Directly running program in a screen

```
screen -d -m python counter.py
```

Note: screen will terminate as soon as your program finishes.

additionally u can list all options 

`Ctrl+a ?`

and you can run names of commands

`Ctrl+a :screenname cowsayshello`

say u wanna resize the horizontal pane to 30 chars or use percentage

`Ctrl+a :resize -h 30`

`Ctrl+a :resize -h 30%`

##### Common Issue

Cannot make directory `/run/screen` : Permission denied , this happens due to sudden restart or process ``/etc/rcS.d/s70screen-cleanup` is running via upstart much earlier than expected to run, and is failing to correctly clean up that directory.

It can be fixed with command 

```bash
sudo /etc/init.d/screen-cleanup start
```


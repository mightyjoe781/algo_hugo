+++

Title = "Getting More Help"
weight = 1

+++

Well these notes are not enough to carry all the information necessary for one to learn FreeBSD that is why user should seek out two reliable sources for assistance.

1. FreeBSD Mailing Lists. 
2. FreeBSD forums.

Do note what you are asking before sending in your question, because audience can up to tens  of thousands. People take their valuable time to answer your question and effort in making the answers to most of these question available elsewhere. So do your homework beforehand and chances are you will get your answers.

General FreeBSD support community simply isn't motivated to help those who won't help themselves or can't follow simple instructions.

### **Man Pages**

*Man pages* (short for *manual pages*) are primordial way of presenting Unix documentation. They might seem difficult because when they were written average user was a C programmer and, as a result the pages were written by programmers for programmers.

Try this on a shell `man vi`.

So a common issue new users face that is how to find the right man page, In such cases you can use `apropros(1)` and `whatis(1)` that support basic keyword searches on man.

do note : `man -k` emulates `apropos(1)`, while `man -f` emulates `whatis(1)`

### FreeBSD.org

The official website contains a variety of information about general FreeBSD administration, installation, and management. Their Handbook are excellent piece of text.

### Asking for Help

Your questions with no context or no information related to system before asking them will either get ignored or you will be asked to collect it.

How to collect data of computer.

- The output of `uname -a` is the most basic information about Operating System version and platform you can provide.
- Any error from output. Be as complete as possible, and include messages from the console or from your logs, especially `/var/log/messages` and any application specific logs.
- Messages about the hardware problems should include a copy of `/var/run/dmesg.boot`

These notes are meant to be used as a reference for personal usage, you are still welcomed to contribute to improve it for greater good for all.


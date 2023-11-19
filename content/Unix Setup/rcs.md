+++

Title = "VCS"

weight = 8

tags = ["git","github","rcs"]

+++

In software engineering, **version control** (also known as **revision control, source control, **or **source code management**) is class of systems responsible for managing changes to computers programs, documents, large websites, or other collections of information.

Changes are usually identified by a number or letter code termed as "revision number".

### Source-management models

Traditional VCS uses a centralized model where all revision control function take place on a shared server. Centralized VCS solves problem of overwriting of code by two programmers by using one of two different "source-management models" : file locking and version merging.

**Atomic Operation**

An operation is *atomic* if system is left in a consistent even if operation is interrupted. The *commit* operation is critical in this sense. Not all VCS have atomic commits, notably **CVS** lacks this feature.

**File locking**

Simplest method of preventing "concurrent access" problems involves locking files so that only one developer at a time has write access to the central repository copies of those files.

Once a dev "checks out" a file, others can read that file, but no one else may change the file until dev "checks in" the updates version (or cancels the checkout).

File locking has both merits and drawbacks, it provides some protection against difficult merge conflicts when a user is making radical changes to many sections of large file. However if files are left locked for too long (happens usually in big organization) people will by pass lock system and end up creating very difficult manual merge. These tools may or may not make it easy to see who had a file checked out.

**Version Merging**

Most VCS allow multiple dev to edit the same file at the same time. The first dev to "check in" changes to the central repository always succeed. The system may provide facilities to merge further changes into the central repository, and preserve the changes from the first dev when other dev checks out.

Merging two files can be a very delicate operation, and usually possible if the data structure is simple, as in text files.

The concept of a *reserved edit* can provide an optional means to explicitly lock a file for exclusive write access, even when a merging capability exists.

**Baselines, labels and tags**

Most revision control tools will use only one of these similar terms (baseline, label, tag) to refer to action of identifying a snapshot or record a snapshot of project.

#### Distributed revision control

DRCS takes a peer to peer approach, as opposed to client-server based. Rather than single, central repository on which clients synchronize, each peer's working copy of the codebase is a bona-fide repository. DRCS exchange patches for synchronizations from peer to peer.

### RCS

---

RCS is an early implementation of VCS. It is a set of UNIX commands that allow multiple users to develop and maintain program code or documents.

RCS was first released in 1982 by Walter E Tichy at Purdue University. It was an alternative tool for then popular (SCCS) which was nearly the first version control software tool (developed in 1972 by early Unix developer). RCS is currently maintained by GNU Project.

An innovation in RCS is the adoption of *reverse deltas.*

#### Usage

RCS revolves around the usage of "revision groups" or sets of files that have been checked-in via `co` (checkout) and `ci` (check-in) commands. By default a checked in file is removed and replaced with a ",v" file (so `foo.rb` becomes `foo.rb,v`) which can then be checkout by anyone with access to revision group. RCS files reflect the main file with additional metadata on its first lines. Once checked in, RCS stores revisions in a tree structures that can be followed so that user can revert a file to a previous form if necessary.

- Pros
  - Simple structure and easy to work with
  - Not dependent on a central repository
- Cons
  - There is little security, in the sense that the version history can be edited.
  - Only one user can work on a file at a time.

Working file is what you normally view and edit and its content can be extracted from RCS file (called as *instantiating a working file*).

RCS file is separate file, conventionally placed in the subdirectory RCS, wherein RCS commands organize the initial and subsequent *revisions* of working files, associating with each revision a unique revision number. RCS file is also know as "comma-v file".

A *revision number* is branch number followed by an integer, and a *branch number* is an odd numbers separated by dot.

RCS file contains two pieces of information used to implement its *access control policy*. The first is a list of usernames. If non-empty, only those users listed can modify RCS file (via RCS commands). The second is a list of locks, i.e. association between a username and a revision numbers.

#### Quick Tour

---

Suppose you have a file `f.c` that you wish to put under control of RCS. If you have not already sone so, make an directory with the command.

```bash
mkdir RCS
```

The invoke the checkin command;

```bash
ci f.c
```

This creates an RCS file in directory RCS, stores `f.c` into it as revision 1.1, and deletes `f.c`. It also asks you for a description.

To get back the working file `f.c` in the previous example, use the checkout command

```bash
co f.c
```

This command extracts the latest revision from RCS file and writes it into `f.c`. If you want to edit `f.c` you must lock it as you check it out

```bash
co -l f.c
```

You can edit `f.c`. Suppose after some editing you want to know what changes you have made

```bash
rcsdiff f.c
```

You can check in again by invoking

```bash
ci f.c
```

The increments the revision number properly. If `ci` complains with the message

```bash
ci error: no lock set by your name
```

then you have tried to check in a file even though you didn't lock it when you checked it out. Of Couse its too late now to do the checkout with locking, because another checkout would overwrite your modifications. Instead invoke :

```
rcs -l f.c
```

This command will lock latest revision for you, unless somebody got ahead of you already. In this case, you'll negotiate with that person.

If your RCS file is private, i.e. if you are the only one depositing changes, strict locking is not needed and you can turn it off

```
rcs -U f.c	#disable strict locking
rcs -L f.c	#enable strict locking
```

To avoid deletion of working file during checking invoke one of

```
ci -l f.c # checkin + locked checkout
ci -u f.c # checkin + unlocked checkout
```

You can give `ci` the number you want assigned to a checked-in revision. Assume all revision were numbered 1.1 , 1.2, 1.3, .. and you would like the release 2. Either of commands

```bash
ci -r2 f.c
ci -r2.1 f.c
```

assigns number 2.1 to the new revision.

You can checkout any of revision using similar commands

```bash
co -r2 f.c
co -r2.1 f.c
```



#### Environment

---




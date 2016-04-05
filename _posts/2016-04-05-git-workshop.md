#### **Introduction.**
Everyone struggles with git in the beginning. This xkcd comic: https://xkcd.com/1597/  My personal story as an example goes here.  A joke about hilariously intimidating git error messages.

#### To master git one needs to understand the data model git uses
 Learning fancy commands and going through "git recipies" and stackoverflow examples only got me even more confused. What turns things around in my experience is understanding the internals of how git organizes stuff.

 Once I understood the key principles, everything else came into place. Even inconsistent git command syntax is no longer such a problem (see e.g. http://stevebennett.me/2012/02/24/10-things-i-hate-about-git/ on examples how bad it is). So let's do that.

####  Git as a huge hash table [git object store]
 Let's invent our own CVS! It will turn out to be git in the end, what a surprise.
   - Let's store files under filenames of their content's hash! This way it is always super easy to check if the file is already stored there! **[blob objects]**
   - To make it a proper CSV we have to store lists of file hashes and filenames somewhere too. Where should we put them? Why not just store them as plain text files within the same system? **[tree objects]**
   - Commits are just text files with hash links too **[commit objects]**

These points are illustrated with the use of the following commands:
```bash

 # saving a git object
 echo 'blah-blah' | git hash-object -w --stdin
 # decoding any git object
 git cat-file -p [blob|tree|commit]
```

The idea here is to show that git is just this data structure (the Merkel DAG), nothing else. And that all the other functionality that git is usually associated with (such as diffs and diff counts, fancy merging alorithms and so on) is only auxiliary and is built on top of it.

#### Crucial git concepts
  - **Branches** are just labels you can put on any commit you like. We saw that the commit is just a five line text file. That means once you learn to write them you can make whatever git history you like.
  - **Immutable object storage** Once a git object is saved it stays there by default no matter what. For example, every misspelled commit is still there. Advantages of immutable objects and addressing by content.

#### Examples (probably the tutorial-like part?)
Let's practice!
  - Let's create a commit that is a the merge of the two other commits and the content is from the third one
  - Investigating a file history via cli
  - Let's do what rebase command does manually.
  - Maybe make a commit with fifty parents? :)

Also, a discussion on tools people use for help with git

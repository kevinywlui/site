---
title: "Developing for Sagemath"
date: 2019-08-01T16:22:11-07:00
draft: false
tags: ["sagemath", "git", "trac"]
---

For my first post, I'll explain how to begin developing for
[Sagemath](http://sagemath.org). This post is essentially a
[gist](https://gist.github.com/kevinywlui/6c494f8dbbb2c667d07fc101e8d3b19c/)
that I shared with members of the UW Sage Seminar. This is a short version of
the official guide.

# Official Sagemath guide

- https://doc.sagemath.org/html/en/developer/index.html

---

# Setting up local development copy

The first task will be to obtain a local development copy of Sagemath.

- Clone Sagemath and checkout the develop branch

```
git clone https://github.com/sagemath/sage
cd ~/sage
git checkout develop
```

- Build Sagemath. I had access to a 48 thread machine so I used all 48 threads.
  It was a shared computer so I also assigned the build process a nice level of
  19\. This will take between 30 minutes to 6 hours so we'll move on to the next
  step while this is going.

```
cd ~/sage
export MAKE='make -j 48'
nice -n 19 make build
```

---

# Developing via trac

Sagemath development is done through the Sagemath trac server:
<https://trac.sagemath.org/>. We now setup our local git repository to interact
with this server easily.

- The new way to get a Sagemath-trac account is to use your Github account.
  Click on "Github Login" on the main Sagemath-trac page.

- The Sagemath people made a cool tool called `git-trac-command`
  (<https://github.com/sagemath/git-trac-command>). This tool provides the `git
  trac` subcommand helps our local git repository interface with the
  Sagemath-trac server. Install this package.

```
cd
git clone https://github.com/sagemath/git-trac-command
cd ~/git-trac-command
python setup.py install --user
``` 

- We can now authenticate using username/token. The username is list on the
  Sagemath-trac page. It is also `gh.yourgithubname`. The token is listed under
  account preferences.

```
cd ~/sage
git trac config --user USERNAME --token TOKEN
```

- We also need to authenticate with ssh keys. So first generate the keys.

```
cd
ssh-keygen # follow the prompts or spam <ENTER>
cat .ssh/id_rsa.pub
```

- The output of the previous command is your ssh public key. Copy this over to
  your account preferences under ssh keys.

---

# Make a ticket!!

- Hit the new ticket button.
- Fill in what it asks.

---

# Checkout the ticket

- Now checkout the ticket in your local development copy.

```
cd ~/sage
git trac checkout <ticket number>
```

---

# Make a commit

- Perform some edits.
- `git add filename` to tell git to track these edits.
- `git commit -m "<one line description>"` to make a commit.
- `git trac push` to send to the trac server.
- When ready, modify the ticket and set to "Need Review".

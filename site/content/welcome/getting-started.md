---
title: Getting Started
date: 2017-01-04T15:04:10.000Z
description: bla bla bla
---
## Create ssh key demo

```sh
ssh-keygen -t rsa -b 4096 -C "<AMEDIA_EMAIL>"
```

Add this key to your [github account](https://help.github.com/en/articles/adding-a-new-ssh-key-to-your-github-account).

## Setup a ssh key for github

Add to ***~/.ssh/config***

```sh
Host github.com
        Hostname github.com
        User git
```

If your private key file is named something other than `id_rsa`, add `IdentityFile ~/.ssh/<private-key-file>` after `User`.

## You need access to the jump server

To deploy to environments in the linpro world you need access to jump.api.no.

### Requirements

* A username
* public ssh key (maybe use the one you made for github)

Your public key (contents of the `<my-key>.pub` file in `~/.ssh`) must be added to the server configuration and synced. Tag @ops on #devops-talk (Slack) and someone will help you.

Add the following to ***~/.ssh/config***

```sh
Host jump.api.no
        Hostname jump.api.no
        User <USERNAME>
        ForwardAgent yes
```

If your private key file is named something other than `id_rsa`, add `IdentityFile ~/.ssh/<private-key-file>` after `ForwardAgent`.

To use your local keys on the remote server (jump.api.no), you need to setup ssh-agent and add your key.

```sh
ssh-agent
ssh-add ~/.ssh/amedia
```

Try logging on

```sh
ssh jump.api.no
```

Once on the server, try logging onto github, to test key forwarding. It is assumed the key you added via ssh-add, is the same key you use to access github.

```sh
ssh git@github.com
```

You have to run

```sh
ssh-agent
ssh-add ~/.ssh/<private-key-file>
```

Each time the computer is rebooted. Here is a neat tool to help you out. Create the file `~/.ssh/add.sh` and put this in there

```sh
#!/bin/bash

ssh-agent
ssh-add ~/.ssh/amedia
```

Now, open `~/.bash_profile`, or the profile config for your operating system and add an alias for this script

```sh
alias fix-ssh='. ~/.ssh/add.sh'
```

Make sure the script is executable by you

```sh
chmod u+x ~/.ssh/add.sh
```

Now run the alias in the terminal

```sh
fix-ssh
```

Enter password and verify access by following the above steps.

## References

* [How to generate ssh key](https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
* [How to add an ssh key to github](https://help.github.com/en/articles/adding-a-new-ssh-key-to-your-github-account)
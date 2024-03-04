---
title: "Namespacing SSH Credentials with Git"
date: 2024-03-04
draft: false
categories:
  - Code
tags:
  - Git, SSH, Security
---

![ManyKeys](/images/key-pile.png?width=10px)

## Why Namespace SSH Credentials with Git?

Why would you ever want to namespace your SSH credentials with Git? If you have multiple accounts on the same Git
hosting service, you might want to use different SSH keys for each account. This is a common scenario for developers who
have both personal and work accounts on GitHub, GitLab, or Bitbucket. This post will show you how to set up SSH
namespaces with Git, so you can use different SSH keys for different accounts on various hosting services.

## Overview

To successfully namespace SSH credentials with Git required several steps. Before we detail these steps, let's first
look at the high-level overview of the process. We begin by explaining the meaning of a _ssh namespace_, then outline
the general flow of the process, including the call stack layers of both `git` and `ssh`. If you want to skip this
overview and begin setting up your namespaces, you can jump to the next section.

### SSH Namespaces

An SSH namespace is essentially a description of which SSH keys to use for which repositories. Specifically, it is a
combination of:

- A git host (e.g. `github.com`, `gitlab.com`, `bitbucket.org`)
- A namespace within that host (e.g. a username or organization name)
- An SSH key to use for that namespace
- One or more repositories within that namespace
- A local directory containing all the relevant repositories

### General Flow of Git and SSH

The general flow of the process is outlined in the diagram below. It begins with a call to `git`, which then uses
configuration files to determine a namespace. This namespace is then used to determine how to modify the url for the
subsequent call to `ssh`. The `ssh` call then uses the url to determine which key to use.

![General Flow](/images/ssh-namespacing3.svg)

Where the numbered steps correspond to the steps in the following section.

## Setting Up Namespaces: 4 Steps

For this walkthrough, we will implement 3 namespaces, two for GitHub and one for GitLab. All source code will be stored
locally under the `~/code` directory, under which we will create subdirectories for each namespace.

### Step 0: Prerequisites

Before we begin, we need to establish the context for the 3 namespaces. We will use the following as examples of the
Host/Account/Email/Repo combinations that might motivate the use of namespaces:

- Name: `personal`
    - Host: GitHub
    - Account: `personal`
    - Email: `personal@email.com`
    - Repos: `repo1` and `repo2`
- Name: `work`
    - Host: GitHub
    - Account: `organization`
    - Email: `work@email.com`
    - Repos: `repo3` and `repo4`
- Name: `collab`
    - Host: GitLab
    - Account: `collab`
    - Email: `collab@email.com`
    - Repos: `repo5` and `repo6`


### Step 1: Create SSH Keys and Add to Hosts

First, we need to create SSH keys for each namespace. We will create 3 keys, one for each namespace. We will use the
default names for the keys, `id_ed25519_{name}` and `id_ed25519_{name}.pub`, and store them in the default
location, `~/.ssh`. For creating the keys, we can use the following commands:

```bash
ssh-keygen -t ed25519 -C "{email}" -f ~/.ssh/id_ed25519_{name}
```

Where `{email}` is the email associated with the namespace. For example, for the `personal` namespace, we would use:

```bash
ssh-keygen -t ed25519 -C "personal@email.com" -f ~/.ssh/id_ed25519_personal
```

Make sure that these keys are added to the appropriate host. The GitHub and GitLab documentation provide instructions
for adding SSH keys to your account [^4] [^5].


### Step 2: SSH Key Routing

Next, we need to create an SSH config file to route the namespaces to the appropriate keys. We will create a file at
`~/.ssh/config` with the following contents:

```text
Host github.com-organization
HostName github.com
UseKeychain yes
AddKeysToAgent yes
User git
IdentityFile ~/.ssh/id_ed25519_work
IdentitiesOnly yes

Host github.com-personal
HostName github.com
UseKeychain yes
AddKeysToAgent yes
User git
IdentityFile ~/.ssh/id_ed25519_personal
IdentitiesOnly yes

Host gitlab.com-collab
HostName gitlab.com
UseKeychain yes
AddKeysToAgent yes
User git
IdentityFile ~/.ssh/id_ed25519_collab
IdentitiesOnly yes
```

This file tells SSH to use the appropriate key for each namespace. The `Host` lines define the namespace, and the
`IdentityFile` lines define the key to use for that namespace. The `IdentitiesOnly` line tells SSH to only use the
specified key for that namespace. We will use git url modification to route the namespaces to the appropriate host.

### Step 3: Git Namespace Configs

Next, we need to tell `git` how to configure each namespace. This is achieved by creating a `.gitconfig` file in the
root directory of each namespace. Specific examples are provided below for the three namespaces in this walkthrough.

The file `~/code/personal/.gitconfig` should contain the following:

```text
[user]
  email = personal@email.com
  
[url "git@github.com-personal"]
  insteadOf = git@github.com
```

The file `~/code/work/.gitconfig` should contain the following:

```text
[user]
  email = work@email.com
  
[url "git@github.com-organization"]
  insteadOf = git@github.com
```

The file `~/code/collab/.gitconfig` should contain the following:

```text
[user]
  email = collab@email.com
  
[url "git@gitlab.com-collab"]
  insteadOf = git@gitlab.com
```

These files tell `git` to modify the host name for each namespace. The `user` section specifies the email to use for
each namespace, and the `url` section specifies the namespace and the host to use for that namespace. We need one more
step, though, to make this work.

### Step 4: Git Namespace Routing

The final step is to tell `git` to use the `~/code/{name}/.gitconfig` files for each namespace. This is achieved by 
modifying the overall `~/.gitconfig` file to include the following:

```text
[includeif "gitdir:~/code/personal/"]
	path = ~/code/personal/.gitconfig
	
[includeif "gitdir:~/code/work/"]
	path = ~/code/work/.gitconfig

[includeif "gitdir:~/code/collab/"]
	path = ~/code/collab/.gitconfig
``` 

This tells `git` to use the appropriate `.gitconfig` file for each namespace. The `includeif` lines specify the
directory to use for each namespace, and the `path` lines specify the `.gitconfig` file to use for that namespace.

{{< admonition type=tip title="Note: Trailing Forward Slash in gitdir" open=true >}}
The `gitdir` path should include a trailing forward slash. This is because `git` uses the `gitdir` path to match the
directory of the repository, and the trailing forward slash behaves like a recursive glob pattern, e.g. `~code/personal/**`.
{{< /admonition >}}


## Using Namespaces

Once the namespaces are set up, you can use them as you would use any other git repository. For example, to clone a
repository in the `personal` namespace, you would use the following command:

```bash
# Change working directory
cd ~/code/personal

# Clone repository
# Clone repository
git clone git@github.com:personal/repo1.git
```

{{< admonition type=tip title="Note: Working Directory" open=true >}}
The `git` routing works based on the working directory when the command is run. This means that you need to be in the
appropriate directory for the namespace you want to use. For example, to use the `personal` namespace, you would need to
be in the `~/code/personal` directory.
{{< /admonition >}}

Thats it! You should now be able to use different SSH keys for different accounts on the same Git hosting service.

## References

This post was adapted from and inspired by several sources, with a few key changes and summary [^1] [^2] [^3].

[^1]: [Vance, Matthew. GitHub BitBucket Multiple SSH Keys](https://gist.github.com/yinzara/bbedc35798df0495a4fdd27857bca2c1)
[^2]: [Jacob, Bivil. How to manage multiple GitHub accounts on a single machine with SSH keys](https://www.freecodecamp.org/news/manage-multiple-github-accounts-the-ssh-way-2dadc30ccaca/).
[^3]: [Russell, Craig. SSH Keys with Multiple GitHub Accounts](https://medium.com/@trionkidnapper/ssh-keys-with-multiple-github-accounts-c67db56f191e)
[^4]: [GitHub adding SSH Keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
[^5]: [GitLab adding SSH keys](https://docs.gitlab.com/ee/user/ssh.html)

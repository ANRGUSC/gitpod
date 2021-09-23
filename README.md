# Integrating your repo with Gitpod

**Table of Contents**
<!-- TOC -->
- [Other Documentation/Articles/Tutorials](#other-documentationarticlestutorials)
- [Getting Started](#getting-started)
- [Tips & Tricks](#tips--tricks)
  - [Prebuilds](#prebuilds)
  - [What about projects with GUIs?](#what-about-projects-with-guis)
  - [Add an "Open in Gitpod" Button to your README](#add-an-open-in-gitpod-button-to-your-readme)
<!-- /TOC -->


## Other Documentation/Articles/Tutorials
- [Gitpod Documentation](https://www.gitpod.io/docs/): very comprehensive and easy-to-read documentation on how to customize your gitpod environments
- [Gitpodifying](https://www.gitpod.io/blog/gitpodify/): tutorial on making your github repo compatible with gitpod

## Getting Started 
Any GitHub, BitBucket, or GitLab repository can be opened in Gitpod by simply prefixing the repository's url with ```https://gitpod.io/#```.
For example, you can open this repository in github by going to [https://gitpod.io/#https://github.com/ANRGUSC/gitpod](https://gitpod.io/#https://github.com/ANRGUSC/gitpod).

This opens the container in Gitpod's default workspace, a linux container with the most common tools and software (git, python, node, etc.) pre-installed.
The real power of Gitpod, though, is that it allows you to customize your dev environment so that all required dependencies, packages, and configurations are preinstalled (with the correct versions) and ready-to-go.

For example, suppose we're working on a python project that requires python 3.9 or higher and has a few python package dependencies, say matplotlib, pandas, and networkx. 
The python package networkx's drawing function also requires graphviz-dev, a system-level dependency, to be installed. 
On linux systems the command to install this package would be:

```bash
sudo apt-get install graphviz-dev
```

Traditionally, if someone wanted to run our code, they'd have to go through a set of setup instructions that involve installing dependencies, setting configurations & variables, etc. 
This process can be problematic when user's are working on different operating systems, have different versions of software installed, etc.
In Gitpod, we specify the entire dev environment by adding just two configuration files to our repository: ```.gitpod.yml``` and ```.gitpod.Dockerfile```.
This completely encapsulates our environment in a Docker container, avoiding traditional setup pitfalls.

For our example, we could write our ```.gitpod.yml``` and ```.gitpod.Dockerfile``` as follows.

First, we can extend the default gitpod workspace and install our project-specific dependencies in the ```.gitpod.Dockerfile```:
```Dockerfile
# .gitpod.Dockerfile

FROM gitpod/workspace-full

# Install graphviz system-level dependency
RUN sudo apt-get update && sudo apt-get install -y graphviz-dev

# Install python 3.9.6 and enable it as the default
RUN pyenv install 3.9.6 && pyenv global 3.9.6
```

Then, we can specify startup tasks, VSCode extensions, ports, etc. in the ```.gitpod.yml``` file:
```yml
# gitpod.yml

# Use our customized Dockerfile instead of the default gitpod workspace
image:
  file: .gitpod.Dockerfile
  
# Startup tasks 
# "init" tasks get executed for every commit to the repo - good for dependency installation-type tasks
tasks:
  - init: pip install matplotlib pandas networkx
  
# We can add VSCode extensions to be installed and enabled in our environment
# This is a python project, so we probably want the python extension installed
# You can also install packages like normal in VSCode
vscode:
  extensions:
    - ms-python.python
```


## Tips & Tricks
Collection of tips & tricks for making the most out of Gitpod

### Prebuilds
By enabling prebuilds, Gitpod automatically builds your dev environments whenever you push a commit to the repo.
This dramatically reduces the time to open your project in Gitpod when you're ready to develop. 
Enable prebuilds in just a few easy steps by following [this guide](https://www.gitpod.io/docs/prebuilds).


### What about projects with GUIs?
If your GUI is browser-based then there's nothing more to do! 
Any port that's exposed on the system can be viewed in-IDE or in your browser in another window or tab.
All you need to do is go to the *Remote Explorer* tab on the left-hand side of your IDE. 
Then just click the *open in browser* or *open in preview* buttons next to the exposed port you want to view.

If your GUI is not browser-based, you're still not out of luck.
Gitpod offers an x11-vnc variant of their default workspace image which opens a virtual desktop in a browser tab for displaying all GUI components.
It's not perfect (it doesn't work with matplotlib yet, but their working on it) but it should support most GUI applications.
All you need to do is modify the your ```.gitpod.Dockerfile``` file to be based on the ```gitpod/workspace-full-vnc``` rather than ```gitpod/workspace-full```.

```Dockerfile
# .gitpod.Dockerfile

FROM gitpod/workspace-full-vnc

...
```

### Add an "Open in Gitpod" Button to your README
To add a button like this:

[![Open In Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/ANRGUSC/gitpod)

to your readme, just add the following to your README.md:
```md
[![Open In Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#<your repo here>)
```

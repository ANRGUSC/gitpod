# Integrating your repo with GitPod

Any GitHub, BitBucket, or GitLab repository can be opened in GitPod by simply prefixing the repository's url with ```https://gitpod.io/#```.
For example, you can open this repository in github by going to [https://gitpod.io/#https://github.com/ANRGUSC/gitpod](https://gitpod.io/#https://github.com/ANRGUSC/gitpod).

This opens the container in GitPod's default workspace, a linux container with the most common tools and software (git, python, node, etc.) pre-installed.
The real power of GitPod, though, is that it allows you to customize your dev environment so that all required dependencies, packages, configurations, etc. are preinstalled (with the correct versions) and ready-to-go.

For example, suppose we're working on a python project that requires python 3.9 or higher and has a few python package dependencies, say matplotlib, pandas, and networkx. 
The python package networkx's drawing function also requires graphviz-dev, a system-level dependency, to be installed. 
On linux systems the command to install this package would be:

```bash
sudo apt-get install graphviz-dev
```

Traditionally, if someone wanted to run our code, they'd have to go through a set of setup instructions that involve installing dependencies, setting configurations & variables, etc. 
This process can be problematic when user's are working on different operating systems, have different versions of software installed, etc.
In GitPod, we specify the entire dev environment by adding just two configuration files to our repository: ```.gitpod.yml``` and ```.gitpod.Dockerfile```.
This completely encapsulates our environment in a Docker container, avoiding traditional setup pitfalls.

For our example, we could write our ```.gitpod.yml``` and ```.gitpod.Dockerfile``` as follows.

First, we can extend the default gitpod workspace and install our project-specific dependencies:
```Dockerfile
FROM gitpod/workspace-full

RUN sudo apt-get update && sudo apt-get install -y graphviz-dev
RUN pyenv install 3.6.15 && pyenv global 3.6.15
```

Then, we can specify some startup tasks to execute once the image is built:
```yml
# gitpod.yml
image:
  file: .gitpod.Dockerfile
tasks:
  - init: pip install matplotlib pandas networkx
```





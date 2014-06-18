# devtest-puppet-rbenv

You'll need to have downloaded and installed the following:
* VirtualBox
* Vagrant

For additional dependencies and setup see the Install section below.

# Install
```bash
$ sudo gem install bundler
$ sudo bundle
```
To check whether or not your system has all of the dependencies necessary to run the Vagrant environments:

```bash
$ rake deps
Checking environment dependencies...
...
Congratulations! Everything looks a-ok.
```

If the above step fails on available Vagrant modules, run:

```bash
$ sudo rake setup
$ rake deps
```

# Usage
To start up the environment: 

```bash
$ rake r10k
$ vagrant up 
```
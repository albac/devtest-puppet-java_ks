# puppet-module-devtest-skeleton

You'll need to have downloaded and installed the following:
* VirtualBox
* Vagrant

For additional dependencies and setup see the Install section below.

# Install
I highly recommend you look at the [rbenv](https://github.com/sstephenson/rbenv) for managing your 
account's Ruby environment a manor which is as sane as possible. Assuming you have either rbenv or
rvm installed:

```bash
$ gem install bundler
$ bundle
```
To check whether or not your system has all of the dependencies necessary to run the Vagrant environments:

```bash
$ rake deps
Checking environment dependencies...
...
Congratulations! Everything looks a-ok.
```

If the above step fails, unless the output tells you to do something differently, I recommend running:

```bash
$ rake setup
$ rake deps
```

# Usage

## Populating the environment with the desired Puppet modules
The following command will use librarian-puppet to deploy the modules specified
in puppet/Puppetfile into your puppet/modules directory. 

```bash
$ rake modules
```

An examples Puppetfile, for instance for testing Presto Puppet module, might look something like this:

```yaml
forge 'https://forge.puppetlabs.com'

mod 'puppetlabs/stdlib', '3.2.1'
mod 'puppetlabs/postgresql', '3.3.3'
mod 'puppetlabs/inifile', '1.0.3'

mod 'puppetlabs/apache',
  :git => 'git@git.vchslabs.vmware.com:vchs/puppetlabs-apache',
  :ref => 'master'

mod 'vchs/epel',
  :git => 'git@git.vchslabs.vmware.com:vchs/puppet-module-epel',
  :ref => '0.0.6'

mod 'vchs/multi_validate_re',
  :git => 'git@git.vchslabs.vmware.com:vchs/puppet-multi_validate_re',
  :ref => 'v0.1.0'

mod 'vchs/rbenv',
  :git => 'git@git.vchslabs.vmware.com:vchs/puppet-rbenv',
  :ref => 'v1.0.0'

mod 'vchs/rails',
  :git => 'git@git.vchslabs.vmware.com:vchs/puppet-rails',
  :ref => 'v1.0.0'

mod 'vchs/sphinx',
  :git => 'git@git.vchslabs.vmware.com:vchs/puppet-sphinx',
  :ref => 'vmware_v1.0.1'

mod 'vchs/ntp',
  :git => 'git@git.vchslabs.vmware.com:vchs/puppet-ntp',
  :ref => '3.0.4'

mod 'vchs/vchs',
  :git => 'git@git.vchslabs.vwmare.com:vchs/puppet-vchs',
  :ref => 'master'

mod 'vchs/presto',
  :git => 'git@git.vchslabs.vmware.com:vchs/puppet-presto',
  :ref => 'master'
```

At this point you are ready to go into puppet/modules and start creating feature branches
for development of your Puppet modules. 

__WARNING: Once you start development on your feature branches you DO NOT want to run
'rake modules' again or you will almost certainly lose the work you have done on the Puppet
modules as librarian-puppet will attempt to completely rebuild the module directory!___

## Starting your test VM
```bash
$ vagrant up
$ vagrant ssh
```

## Running Puppet apply to test your new module code
In the spawned VM, you can run 'puppet apply' against the smoketest scripts for your modules:

```bash
$ sudo su -
# puppet apply --modulepath=/vagrant/puppet/modules /vagrant/puppet/modules/<your module>/tests/init.pp --debug
```

Note that the above command can be used to do multiple Puppet runs over a long-running VM. If you'd like to
test on a fresh OS you rebuild the VM by logging out of the VM and:

```bash
$ vagrant destroy -f
$ vagrant up
$ vagrant ssh
...
```

It takes some time to rebuild the VM so you'll want to minimize the number of fresh rebuild cycles. Also, if you
find yourself doing a lot of VM rebuilds then it is time to start investigating the following technologies:

### For local testing
* [rspec](http://rspec.info/)
* [beaker-rspec](https://github.com/puppetlabs/beaker-rspec)

### For centralized automated testing
* some form of [CI system]() 
  * [Jenkins](http://jenkinsci.org) is the most popular onsite solution.
  * [TravisCI](http://travisci.org) is a good place to start for cloud-base CI through most find they outgrow it quickly.

# Additional resources

The links below are repos which have been created using this module as a base:

* [devtest-puppet-presto](https://git.vchslabs.vmware.com/vchs/puppet-presto)
* [devtest-puppet-rbenv](https://git.vchslabs.vmware.com/vchs/puppet-rbenv)
* [devtest-puppet-rails](https://git.vcshlabs.vmware.com/vchs/puppet-rails)

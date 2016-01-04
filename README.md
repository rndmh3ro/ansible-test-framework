# Testing Ansible Roles

This framework provides the necessary files and configurations to easily setup your environment for testing ansible-roles.
It uses test-kitchen, vagrant and serverspec to test your roles on multiple operating systems.

# Prerequesites

Install the following software:

- Git
- Ruby
- VirtualBox
- Vagrant

# Usage

Create a directory for your role you want to test (called `ansible_role` in the following example).
Git-clone the testing-framework into your newly created directory and change into it:
```
# basic setup
mkdir ansible_role
git clone https://github.com/rndmh3ro/ansible-test-framework ansible_role/
cd ansible_role/
```

Create an empty role with `ansible-galaxy`.
Run inside your role-directory, replace `ansible_role` with the name you gave the directory.

```
# create empty ansible role
ansible-galaxy init -p ../ --force ansible_role
```

Install test-kitchen, serverspec, the provisioner, driver and all its dependencies, with the help of [bundler]:
```
# Install software and dependencies
gem install bundler
bundle install
```

Customize your testing-setup. 
Replace the default name `ansible-test-framework` with the name of your role (in this example `ansible_role`) in two places:
- `default.yml` -> replacement should be in the `roles_path`.
- `.kitchen.yml` -> replacement should be the first item after `roles`.

You can also use this `sed`-command to replace the occurences.
Just replace `your_role` in the command with the name of your role.

```
# replace ansible-test-framework with your role-name in:
sed 's/ansible-test-framework/your_role/g' default.yml .kitchen.yml
```

Write your ansible-role now!

```
# writing tests
## IN REPO
  mkdir -p test/integration/default/serverspec/localhost
  echo "require 'serverspec'" >> test/integration/default/serverspec/spec_helper.rb
  echo "set :backend, :exec" >> test/integration/default/serverspec/spec_helper.rb
## IN REPO

http://kitchen.ci/docs/getting-started/writing-server-test
https://github.com/neillturner/kitchen-ansible#test-kitchen-serverspec
```

```
# using the framework to test an existing role


# fast test on one machine
bundle exec kitchen test default-ubuntu-1204

# test on all machines
bundle exec kitchen test

# for development
bundle exec kitchen create default-ubuntu-1204
bundle exec kitchen converge default-ubuntu-1204
```


[test-kitchen]: https://github.com/test-kitchen/test-kitchen
[vagrant]: https://www.vagrantup.com/
[VirtualBox]: https://www.virtualbox.org/
[rake]: https://github.com/ruby/rake
[serverspec]: http://serverspec.org/
[kitchen-ansible]: https://github.com/neillturner/kitchen-ansible
[kitchen-vagrant]: https://github.com/test-kitchen/kitchen-vagrant
[kitchen-sync]: https://github.com/coderanger/kitchen-sync
[kitchen-transport-rsync]: https://github.com/unibet/kitchen-transport-rsync
[thor-foodcritic]: https://github.com/reset/thor-foodcritic
[hardening.io]: http://hardening.io/
[git]: https://www.git-scm.com/
[bundler]: http://bundler.io/

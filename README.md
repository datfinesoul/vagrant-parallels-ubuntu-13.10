### Building a new vagrant parallels box from definitions (ubuntu-13.10-server-amd64)

Forked from https://github.com/mkoryak/vagrant-parallels-ubuntu-13.10-lts.git

**Requirements**:

 - you should have parallels installed (tested with 9)
 - http://www.parallels.com/downloads/desktop/ Download Parallels Virtualization SDK
    If Homebrew is installed, might need to create the following symlink
    sudo ln -fs /Library/Frameworks/ParallelsVirtualizationSDK.framework/Libraries/Python/2.7/prlsdkapi /Library/Python/2.7/site-packages/
 - make sure that you have ruby 1.9.3 installed
 - go to [http://downloads.vagrantup.com/](http://downloads.vagrantup.com/) and install it

Make a new dir

If needed create Gemfile

```
ruby "1.9.3"
gem "veewee"
```

mkdir -p definitions/ubuntu-13.10-lts
cp <THIS REPO>/definitions/parallels-ubuntu-13.10-server-amd64-clean/\* definitions/ubuntu-13.10-lts/

comment out 
shell_exec optimize_command
in ~/.rvm/gems/ruby-1.9.3-p448/gems/veewee-0.3.12/lib/veewee/provider/parallels/box/export.rb

ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -p 22 -l vagrant 10.211.55.10
sudo dhclient eth0
Just add under sudo to /etc/sysctl.conf these lines:
Code:
#
#Disable ipv6
net.ipv6.conf.all.disable_ipv6=1
net.ipv6.conf.default.disable_ipv6=1
net.ipv6.conf.lo.disable_ipv6=1

```
vagrant plugin install vagrant-parallels
sudo gem install veewee
# This doesn't work
bundle exec veewee parallels define 'ubuntu-13.10-lts' 'git://github.com:datfinesoul/vagrant-parallels-ubuntu-13.10-lts.git/definitions/parallels-ubuntu-13.10-server-amd64-clean'
bundle exec veewee parallels build 'ubuntu-13.10-lts'  --workdir=.
bundle exec veewee parallels export 'ubuntu-13.10-lts' --workdir=.
```

Now you have a vagrant parallels box.

To import it into vagrant type:

```
vagrant box add 'ubuntu' './ubuntu-13.10-lts.box'
```

To use it:

```
vagrant init 'ubuntu'
vagrant up --provider=parallels
vagrant ssh
```

### Build another version/distro using this repo

You can build an ubuntu 12.10 or an ubuntu 13.10 and possibly even a different distro using this repo as a starting point.

You will need to pull down this repo and use one of the folders in the the defintions/ folder as a starting point. The 2 folders in there are:

- parallels-ubuntu-13.10-server-amd64-clean = a clean install containing dev tools and ruby
- parallels-ubuntu-13.10-server-amd64 = an install containing puppet, chef, dev tools and ruby

you will need to open the <code>defintion.rb</code> file and edit these 2 lines to point to the correct iso image:

```
:iso_file => "ubuntu-13.10.3-server-amd64.iso",
:iso_src => "http://releases.ubuntu.com/13.10/ubuntu-13.10.3-server-amd64.iso"
```

after that, you can build this defintion with veewee:  
```
bundle exec veewee parallels define 'box_name' 'path/to/your/defintion/dir'
bundle exec veewee parallels build 'box_nane'  --workdir=.
bundle exec veewee parallels export 'box_name' --workdir=.
```


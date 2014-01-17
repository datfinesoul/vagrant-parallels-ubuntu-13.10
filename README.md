## Building a new vagrant parallels box from definitions (ubuntu-13.10-server-amd64)

Definition based on template at https://github.com/jedi4ever/veewee

## Requirements

- Parallels 9.x
- Parallels 9.x Virtualization SDK
  http://www.parallels.com/downloads/desktop/
- If Homebrew Python is installed, you might need to run the following:
  `sudo ln -fs /Library/Frameworks/ParallelsVirtualizationSDK.framework/Libraries/Python/2.7/prlsdkapi /Library/Python/2.7/site-packages/`
- Veewee
  https://github.com/jedi4ever/veewee
- Ruby 1.9.3
- Vagrant 1.4.3+
  http://www.vagrantup.com/downloads.html

## Steps / Overview

- Check out this repository
- `bundle install`
- There is a bug with parallels compacting images atm, so you need to comment out the following line in a veewee file located in your ruby directory. (In my case the base is `~/.rvm/gems/ruby-1.9.3-p448/`
  `shell_exec optimize_command`
  in
  `gems/veewee-0.3.12/lib/veewee/provider/parallels/box/export.rb`

```
bundle exec veewee parallels define 'ubuntu-13.10-server-amd64' './definitions/ubuntu-13.10-server-amd64'
bundle exec veewee parallels build 'ubuntu-13.10-server-amd64'  --workdir=.
bundle exec veewee parallels export 'ubuntu-13.10-server-amd64' --workdir=.
```

This should produce a parallels box.

## Notes
There currently is a bug in the Parallels guest additions, which causes some images not to boot.  I'm using my own vagrant-parallels plugin to get around this, but this might not work for everyone.

`https://s3.amazonaws.com/glg_starphleet/vagrant-parallels-0.2.1.gem`


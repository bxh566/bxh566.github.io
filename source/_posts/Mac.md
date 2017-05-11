---
title: Mac
date: 2017-05-11 10:26:46
tags: note
---
### check port with pid
```
lsof -n -i4TCP:1234 | grep LISTEN
```

### brew
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
#brew cask
brew tap caskroom/cask
#old versions
brew tap caskroom/versions
#useage
brew list
brew cask list
brew install java
brew cask install vagrant
#upgrade
brew update
brew cask install --force vagrant
```
### remove Java
```
sudo rm -rf /Library/Java/*
sudo rm -rf /Library/PreferencePanes/Java*
sudo rm -rf /Library/Internet\ Plug-Ins/Java*
```
### jenv
```bash
brew install jenv
mkdir ~/.jenv/versions
echo 'eval "$(jenv init -)"' >> ~/.bash_profile
jenv add java_home
jenv local 1.6: set current folder use java 1.6
jenv versions
```


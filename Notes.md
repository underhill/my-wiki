# Ubuntu dev box

## Packages
Update index
```
sudo apt-get update
```

Upgrade
```
sudo apt-get upgrade
```

Search
```
apt-cache search search_term
```

Info
```
apt-cache show package_name
```

Installed
```
sudo apt-get -y install vim
sudo apt-get -y install htop
sudo apt-get -y install git
sudo apt-get install build-essential
```

## JDK
Download oracle jdk tarball
```
tar zvf jdk*tar.gz
sudo mkdir /usr/java
sudo mv jdk-VERSION /usr/java
cd /usr/java
sudo ln -s jdk-VERSION latest
```

A better way but uses 3rd party repo
```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer
```

## Chrome
This does not work - get an error about non-existent repo
```
sudo apt-get install libxss1 libappindicator1 libindicator7
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome*.deb
```

A better way
```
sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
sudo wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
sudo apt-get update
sudo apt-get install google-chrome-stable
```

## Dev tools
Install into /apps and add paths to bashrc
```
sudo mkdir /apps
```
Tools
```
wget http://apache.mirrors.hoobly.com/maven/maven-3/3.2.3/binaries/apache-maven-3.2.3-bin.tar.gz
sudo tar zxCf /apps apache-maven-3.2.3-bin.tar.gz
wget http://dl.bintray.com/groovy/maven/groovy-binary-2.3.6.zip
sudo unzip -d /apps groovy-binary-2.3.6.zip
```

Setup maven - create settings.xml
```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                  http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <servers>
    <server>
      <id>deployment</id>
      <username>...</username>
      <password>...</password>
      <configuration></configuration>
    </server>
  </servers>

</settings>
```

Jenkins
Latest
```
wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo "deb http://pkg.jenkins-ci.org/debian binary/" >> /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
```

Stable
```
wget -q -O - http://pkg.jenkins-ci.org/debian-stable/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo "deb http://pkg.jenkins-ci.org/debian-stable binary/" >> /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
```

Install jenkins plugins
* Git client
* Git
* Groovy Postbuild
* Hudson Groovy builder
* Maven Release
* SCM API
* SSH Credentials
* Workspace Cleanup

Copy maven settings to jenkins - will need to do this for each slave
```
cp ~dev/.m2/settings.xml ~jenkins/.m2/
chown jenkins: ~jenkins/.m2/settings.xml
```
Java home is `/usr/lib/jvm/java-7-oracle/`

Set maven and groovy home

Set ssh key for github access


# OS changes

Grub - change default to Windows
```
sudo cp /etc/default/grub /etc/default/grub.bak
sudo vim /etc/default/grub
# look for GRUB_DEFAULT=0
# or check /boot/grub/grub.cfg for os name then
# GRUB_DEFAULT="Windows 7 (loader) (on /dev/sda1)"
sudo update-grub
```

Disable shopping suggestions in Unity dash scopes
```
gsettings set com.canonical.Unity.Lenses disabled-scopes "['more_suggestions-amazon.scope', 'more_suggestions-u1ms.scope', 'more_suggestions-populartracks.scope', 'music-musicstore.scope', 'more_suggestions-ebay.scope', 'more_suggestions-ubuntushop.scope', 'more_suggestions-skimlinks.scope']"
```

Ditch unity for gnome
```
sudo apt-get install gnome-session-flashback
```

Get rid of guest
In Gnome
```
sudo apt-get remove gdm-guest-session
```
In Unity
```
sudo sh -c 'printf "[SeatDefaults]\nallow-guest=false\n" >/usr/share/lightdm/lightdm.conf.d/50-no-guest.conf'
```

Setup ssh - gen keys if needed
```
ssh-keygen -t rsa -C "your_email@example.com"
```
Upload to github

Setup ssh config
```
Host github.com
  User git
  IdentityFile ~/.ssh/id_rsa

Host *-relic.rhcloud.com
  IdentityFile ~/.ssh/id_rsa
```

Test ssh
```
ssh -T git@github.com
```

Setup [ssh-agent](http://bose.utmb.edu/Compu_Center/ssh/SSH_HOWTO.html) - add to crontab
```
@reboot ssh-agent -s | grep -v echo > $HOME/.ssh-agent
```

Add to bashrc
```
[ -z "$SSH_CLIENT" ] && . $HOME/.ssh-agent

alias keyon="ssh-add -t 10800"
alias keyoff='ssh-add -D'
alias keylist='ssh-add -l'
```


## Misc
Install gollum - https://github.com/gollum/gollum
```
sudo apt-get install ruby ruby-dev libicu-dev zlib1g-dev
sudo gem install gollum
cd ~/sandbox
git init my-wiki
setup mywiki github
gollum 
```

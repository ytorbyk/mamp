## Homebrew

#### Intall brew
```bash
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

#### Open  `~/.profile` or `~/.bash_profile`
```ini
# create ~/bin folder
# create the file if it does not exist
# add this into the file
export PATH="/usr/local/bin:/usr/local/sbin:$PATH:/Users/<your-user>/bin"
```

#### Verify brew
```bash
$ brew --version
# result something like this
Homebrew 1.3.6
Homebrew/homebrew-core (git revision 9f071; last commit 2017-11-04)
```

#### Add Extra Brew Taps and Updating
```bash
$ brew tap homebrew/php
$ brew update
```

#### Autoinstall services
```bash
$ brew services list
```

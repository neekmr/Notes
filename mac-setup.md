# Setup
- notification
- mouse keyboard
- updates settings
- updates
- chrome.
- install homebrew
- cakebrew
- finder settings.
- smooth scroll.
- bandwidth meter.
- uninstaller.
- dock - mission control.
- sublime
- dock settings.`defaults write com.apple.dock autohide-delay -float 0; killall Dock`
- dock spacers.`defaults write com.apple.dock persistent-apps -array-add '{"tile-type"="spacer-tile";}'; killall Dock`.
- launchpad icons.`defaults write com.apple.dock springboard-rows -int 8;defaults write com.apple.dock springboard-columns -int 10;killall Dock`
- acquia dev desktop
- install homebrew - /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
- brew install composer
- brew install zsh
- install ohmyzsh - sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
- change theme - vim ~/.zshrc , bureau
- composer global require drush - composer global require drush/drush
- add composer to zshrc - export PATH="$HOME/.composer/vendor/bin:$PATH"
- git config set global - git config --global user.email "nee.gsync@gmail.com", git config --global user.name "Neeraj Kumar"
- brew install php - installs the latest version 
- php jit compiler error - zzz-myphp.ini in /usr/local/etc/php/7.3/conf.d - 
	; My php.ini settings
	; Fix for PCRE "JIT compilation failed" error
	[Pcre]
	pcre.jit=0
- drush to work with acquia devdesktop - export PATH="/Applications/DevDesktop/"$PHP_ID"_x64/bin:/Applications/DevDesktop/mysql/bin:/Applications/DevDesktop/tools:$PATH"
- remove date and time from snapshots - defaults write com.apple.screencapture "include-date" 0 , killall SystemUIServer
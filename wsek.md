# Wsek Process

### Composer download
1. `composer create-project drupal-composer/drupal-project:8.x-dev wsek --no-interaction`

### Git
* set git global config name and email.
* `global ignore` files like .DS_Store
  * `~/.gitignore` and `git config --global core.excludesfile ~/.gitignore`
* `git init` `git add .` `git commit -m "initial commit"`


### Github upload
* add ssh keys `https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent`
* set up ssh config
```
$ cd ~/.ssh/
$ touch config
$ subl -a config
```
```
#activehacker account
Host github.com-activehacker
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa_activehacker

#jexchan account
Host github.com-jexchan
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa_jexchan
```
* Add keys to github.

### Aws Setup
* change region to **N.virginia**

#### Rds
* Mysql
* enbale free tier
* chose latest version
* db instance identifier **wsekdbi**
* set username - alphanumeric.
* set password - strong.
* default vpc.
* subnet group default.
* Public - **NO**
* AZ- no preference.
* create new **SG**
* dbname - **wsekdb**
* port - 3306.
* parameter group - def
* option group - def
* IAM db authentication - disable.
* **Backup** - 35 days.
* backup window - no preference.
* copy tags to snapshots - yes.
* **Enhanced monitoring** - disabled.
* Log exports - not selected any of the 3 - error, general, slow.
* **Maintenance** - enable auto minor upgrade, maintenance window - none.
* delete protection - enabled.
  
#### Efs
* 

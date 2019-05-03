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

#### IAM
* Change password to something really strong.
* Enable mfa using google authenticator.
* **Add-User**
	* username - strong
	* only console access
	* Custon password
	* Unckeck require password reset.
* **Group** - selfroot
* create **no permissions boundry** - assign permissions to users within groups
* Manage **group permissions** to assign access to users
* **Password Policy** - check all options - 8, 30, 23
* Create **Key-Pair**
*

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
* create
* vpc - default.
* select all 6 
* encrypt - yes
	* default master key
* life cycle management - yes

#### BeanStalk
* name - wsek
* tags - no
* php
* sample
	* default configuration
* create

#### Build
* wsekbuild
* enable build badge
* no additional configuration
* add github
* select build status.
* select webhook
	* no other option
* custom image
* linux
* other
	* wodby/drupal-php
* disable elevated privileges
* new service role
* **TimeOut**- 4mins, 30mins.
* No certificate
* default vpc
* no subnet
* default sg

#### IMP - Get the s3 bucket name -> make build -> add pipeline
#### IMP - DO NOT use VPC for build

#### Pipeline
* wsekpipe
* new service role
* allow aws to create services
* default location.
* github - repo - master.
* github webhook option
* **Built** - codecommit - create new
	* wsekbuild
	* no description, no tags
	* custom image - linux - other registry
		* wodby/drupal-php
	* Privileged - no
	* set timeout- 4min, 30min
	* default options
	* Use **buildspec.yml** generally
		* **NOW** use commands.
	* Cloudwatch logs - default yes, s3 optional no.
* **Deploy** - Elastic beanstalk
	* App name
	* Environment
* **Initial build** will fail coz no buildspec yml file, okay.

#### SG setup
* Create sg for **Efs** and attach
	* efswsek-security-group, Efs security group, default vps
	* create
	* Edit **Inbound** rule
		* add nfs tcp 2049 custom to efs and then add that sg to beanstalk in **bs configuration**
		* **SAME** for RDS.
	* Remove **default** and add newly created sg.

#### EC2 keypair for ssh
* 

#### Beanstalk Configuration setup.
* /web and all default
* Check all security group leaving the default.

### NOTE
* **DONT** give access to database unless the code is right.(using security groups)

### ERRORS
* **EFS** mount deploy error - sg was not added to bs

# Wsek Process

## IMP
* Install drupal on HTTPS
* disable cloudfront cache before install

## LOCAL SIDE

### Setup Git
* brew install git
* set username email
* global **gitignore** file
	* vim ~/.gitignore_global
	* git config --global core.excludesfile ~/.gitignore_global

### Download project
* `composer create-project drupal-composer/drupal-project:8.x-dev some-dir --no-interaction`
* git commit
* git push

## .EBEX Procedure
* Make the pipeline
* commit `dev.config` file to repo
* remove `settings.php` from `.gitignore`
* do not install on local
* Install drupal on live
* get `Hash`
* Put in all the `variables`
* Enable all the `sg` and `keys` in beanstalk environment
* commit `efs-mount.config` and `settings.php` file to repo
* Run the build

### Local Install
* composer install

### Live Install
* **DISABLE** cache in cloudfront
* `composer install --no-dev` - buildspec.yml file.

### Ebextensions
* `mkdir .ebextensions`
* `vim dev.config`
* `vim efs-mount.config`

### Note - put sample html, get the ssl and then install drupal.

### Composer download
1. `composer create-project drupal-composer/drupal-project:8.x-dev wsek --no-interaction`
2. `git commit initial`
3. add the **config** folder with permissions **755**

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
* Create google 2fa for every account.

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
* Log exports - select all 3 - error, general, slow.
* **Maintenance** - enable auto minor upgrade, maintenance window - none.
* delete protection - enabled.
  
#### Efs
* Create EFS security group.
* create and attach sg
* enable encryption
* enable lifecyle management
* vpc - default.
* select all 6 
* encrypt - yes
	* default master key
* life cycle management - yes

#### BeanStalk
* name - wsekbs
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
	* push option
* custom image
* linux
* other
	* wodby/drupal-php
* disable elevated privileges
* new service role
* **TimeOut**- 5mins, 10mins.
* No certificate
* **Enable semantic versioning** buildspecfile.
* Path - no.
* **Name** build id
* default vpc
* no subnet
* default sg
* cloudwatch log yes.

#### IMP - Get the s3 bucket name -> make build -> add pipeline
#### IMP - DO NOT use VPC for build
#### IMP - Check if existing POLICIES

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
* **END** will start the build automatically, make sure all the files exist in the code. mainly the **dev.config** file for and **buildspec.yml**.

#### SG setup
* Create sg for **Efs** and attach
	* efswsek-security-group, Efs security group, default vps
	* create
	* Edit **Inbound** rule
		* add nfs tcp 2049 custom to efs and then add that sg to beanstalk in **bs configuration**
		* **SAME** for RDS.
	* Remove **default** and add newly created sg.

#### EC2 keypair for ssh

#### First Run
* will show forbidden coz /web not set.
* change **software**to /web and **instances** to iclude the sg of rds and efs.

#### Beanstalk Configuration setup.
* /web and all default
* Check all security group leaving the default.

### NOTE
* **DONT** give access to database unless the code is right.(using security groups)
* make **config/sync/.gitkeep** for the config files.

### ERRORS
* **EFS** mount deploy error - sg was not added to bs

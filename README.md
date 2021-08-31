# ansible_pipeline

## Installing Email Extended
Go to Manage Jenkins > Manage Plugins > click on tab Available and search for “Email Extension”.
If you find, install it. If you don’t find it, search in Installed tab because it can be installed.

## Configuring Email Extended
To send email, the plugin needs the smtp configured.
Go to Manage Jenkins > Configure System > search for “Extended E-mail Notification”.
Then:
Configure the smtp.

## Steps to create password:
Go to your account settings (https://myaccount.google.com/) -->> Security -->> Under signing in to Google -->> App Password -->> Enter your credentials to login to your account -->> Select 'App' and 'Device' -->> Generate.

Copy and paste the password somewhere.

You can use this password instead of your account password.

## Upgrade jenkins
```
cd /usr/lib/jenkins/
sudo mv jenkins.war jenkins.war.old
sudo wget https://updates.jenkins.io/download/war/2.289.3/jenkins.war
sudo /etc/init.d/jenkins restart
```

## Download the ec2.py and ec2.ini
```
curl -O https://raw.githubusercontent.com/ansible/ansible/stable-1.9/plugins/inventory/ec2.ini
curl -O https://raw.githubusercontent.com/vshn/ansible-dynamic-inventory-ec2/master/ec2.py
```

## Create credentials 
 
|ID                    |Name                               |Kind                          |Description|
| -------------        |:-------------:                    |:-------------:               | -----:| 
|aws_secret_access_key |aws_secret_access_key	           |Secret text		              | Update |
|aws_access_key_id	   |aws_access_key_id	               |Secret text		              |Update |
|ansible_ssh	       |jenkins	                           |SSH Username with private key |Update |
|git_hub	           |josephakeni/****** (GitHub login)  |Username with password        |GitHub login|	

## Return private IP from ansible dynamic inventory on AWS
Within ec2.ini, edit this line:
```
vpc_destination_variable = ip_address
```
To this:-
```
vpc_destination_variable = private_ip_address
```

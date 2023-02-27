
[[Index]] 


  

## Update & Upgrade OS packages

```bash 

  • sudo apt-get update

  

  • sudo apt-get upgrade

``` 

## Install package

```bash 

  • sudo apt-get install

``` 

## Uninstall packages

```bash 

  • sudo apt-get remove --purge

  

  • sudo apt-get remove

``` 

# CENTOS/REDHAT COMMANDS

## Install package

```bash 

yum install httpd

``` 

## Uninstall package

```bash 

 yum remove package_name

``` 

## Update/Upgrade packages

```bash 

  • yum update

  

  • yum upgrade

  

  • yum update -y

``` 

## Get Details  

```bash 

  • rpm -qi <package_name>

  • yum info <package_name>  

```  


```bash 
[student@serverb ~]$ sudo dnf install \
rhcsa-script-1.0.0-1.noarch.rpm
[sudo] password for student: student
Last metadata expiration check: 0:11:06 ago on Wed 27 Apr 2022 05:01:59 AM EDT.
Dependencies resolved.
==========================================================================
 Package Architecture Version Repository Size
==========================================================================
Installing:
 rhcsa-script noarch 1.0.0-1 @commandline 7.5 k
Transaction Summary
==========================================================================
Install 1 Package
Total size: 7.5 k
Installed size: 593
Is this ok [y/N]: y
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
 Preparing : 1/1
 Running scriptlet: rhcsa-script-1.0.0-1.noarch 1/1
 Installing : rhcsa-script-1.0.0-1.noarch 1/1
 Running scriptlet: rhcsa-script-1.0.0-1.noarch 1/1
 Verifying : rhcsa-script-1.0.0-1.noarch 1/1

```  




[[Index]] 

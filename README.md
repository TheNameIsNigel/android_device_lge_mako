####Current COT Build Status
<a href='http://jenkins.gitmanagement.tk/job/COT%20for%20the%20LG%20Viper%204G%20LTE/'><img src='http://jenkins.gitmanagement.tk/job/COT%20for%20the%20LG%20Viper%204G%20LTE/badge/icon'></a>

####Current CM9 Build Status

-----------------------------------------------
HOW-TO BUILD:
=============
 

**This short guide assumes you're on Ubuntu 10.04**

Setting up a build environment on Ubuntu 10.04
-----------------------------------------------
Install the Java 6 JDK like this.

    $ mkdir ~/java

    $ cd ~/java

    $ git clone https://github.com/thenameisnigel/oab-java6.git

    $ cd ~/java/oab-java6
    
    $ chmod a+X oab-java.sh

    $ sudo ./oab-java.sh

    $ rm -rf ~/java

Then when its finished, follow the guide on the AOSP Page:

http://source.android.com/source/initializing.html


Getting the (right) source
--------------------------

First, we need to create directories for the build (system can be whatever you want to name your working directory):

    $ mkdir -p ~/cm-10.1

Now we'll need repo. Let's download it and make it executable:

    $ sudo curl https://dl-ssl.google.com/dl/googlesource/git-repo/repo > /usr/bin/repo

    $ sudo chmod a+x /usr/bin/repo
  

Now initialized the repository and pull the source (with my repos attached):

    $ cd ~/cm-10.1
    
    $ repo init -u git://github.com/CyanogenMod/android.git -b cm-10.1
    
    $ cd .repo
    
    $ mkdir -p local_manifests

    $ cd local_manifests
    
    $ wget https://github.com/ProjectOpenCannibal/cotr-local_manifests/blob/master/cotr-2.1.3-jb.xml --no-check-certificate
    
----

Now let's get some required stuff.

Open terminal and type the following stuff:
  
	$ geany ~/cm-10.1/.repo/local_manifests/devices.xml
	
Once that opens, copy and paste the following:

```xml
<?xml version="1.0" encoding="UTF-8"?>
  <manifest>
    <project name="thenameisnigel/android_device_lge_mako" path="device/lge/maki" remote="github" revision="cm-10.1" />
    <project name="ProjectOpenCannibal/android_device_lge_ls840" path="device/lge/ls840" remote="github" revision="cm-10.1" />
    <project name="thenameisnigel/android_kernel_lge_ls840" path="kernel/lge/ls840" remote="github" revision="ics" />
    <project name="CyanogenMod/lge-kernel-mako" path="kernel/lge/mako" remote="github" revision="cm-10.1" />
    <project name="TheMuppets/proprietary_vendor_lge" path="vendor/lge" remote="github" revision="cm-10.1" />
  </manifest>
```

Now you need to pull the required repositories so you run:

	$ cd ~/android
	$ repo sync -j8



Building CM9
-------------
Once thats done you can start compiling.

Follow the aosp instructions on setting up the build environment. - http://source.android.com/source/download.html

When the environment is setup, we need to grab a copy of Koush's ROM Manager and the Term.apk. This is necessary to build CM9.

    $ vendor/cm/get-prebuilts


Now, we build (android being your work directory path):

    $ cd ~/android

To build for the Google Mako:
    
    $ source build/envsetup.sh
    
    $ lunch 5 && make -j8


Installing CM9
---------------
If the build was successful, you can now take the update zip found in out/target/product/mako/ and flash using a custom recovery. Make sure to grab the latest Gapps to complete the experience.

When you want to rebuild with new changes to the BoardConfig.mk or after syncing to the latest CM src make sure to do the following before you recompile.

    $ make clobber



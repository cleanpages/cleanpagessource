---
title: "JDK/JRE Installation on Linux"
excerpt: "Manually install and manage multiple versions of JDK"
author_profile: false
categories:
  - JAVA
tags:
  - JDK
  - JRE
  - JRE
Layout: posts
toc: false
toc_sticky: false
---

JDK installation and management are easy and clear in Linux with help of the software tools integrated in Linux, which are `tar` and `update-alternatives`. This article consists of download, installation, and management of multiple versions of JDK.
{: style="text-align: justify;"}

### Download JDK:
The easiest way to install JDK in Debian based linux distro is by using `apt install` command:<br>
```
sudo apt install openjdk-8
```
However, this command might not allow you installing the latest JDK that has been released on Oracle website. At the time of writing this article, JDK-15 has been released, but in Debian/Ubuntu official repository, JDK-14 is the latest version.
If you want to use the latest version of JDK. You have to download JDK package from Oracle website. For example, we download JDK-15 and install it later.
{: style="text-align: justify;"}
[jdk-15.0.1_linux-x64_bin.tar.gz](https://www.oracle.com/ca-en/java/technologies/javase-jdk15-downloads.html#license-lightbox "jdk-15.0.1_linux-x64_bin.tar.gz")
### Install JDK:
Installation is done by extracting the downloaded `tar` package into the directory `/usr/lib/jvm/jdk-15`. If this directory does not exist, you could create it by using the command `mkdir`, then change to `/usr/lib/jvm/jdk-15` and extract the `tar` package by the following command with `sudo` privilege:
```
sudo tar -xvf jdk-15.0.1_linux-x64_bin.tar.gz
```

**Note:** You are allowed extracting JDK in whatever directory you want. However, you have to tell operating system which version of JDK is currently using. Next steps can help doing this.
{: .notice--warning}

### Set current/default version:
* Set JRE, `java`:
```
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk-15/bin/java" 1
```
* Set Java compiler, `javac`:
```
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk-15/bin/javac" 1
```
In the above two command lines, you could replace `/usr/lib/jvm/jdk-15` by the directory where you have extracted JDK.

### Verify the installation
We verify JRE by the command:<br>
`java --version`<br>
We get the following output, in which JRE version is set to java 15
```
java 15 2020-09-15
Java(TM) SE Runtime Environment (build 15+36-1562)
Java HotSpot(TM) 64-Bit Server VM (build 15+36-1562, mixed mode, sharing)
```
By the same way, we also verify Java compiler version:<br>
`javac --version`<br>
The output shows javac 15
```
javac 15
```
### Conclusion
`upadtes-alternatives` really helps to simplify JDK installation and the management of JDK versions without setting path in command line or editing bash scripts. We could also have different versions of JRE and Java compiler (javac). For example, we can use Java 8 compiler combined with JRE 15. However, the version of compiler should be always lower than the version of JRE.
{: style="text-align: justify;"}
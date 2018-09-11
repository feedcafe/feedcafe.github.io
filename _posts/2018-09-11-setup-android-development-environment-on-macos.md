---
layout: post
title: Setup Android Development Environment on MacOS
date: 2018-09-11 22:19 +0800
categories: [Android]
tags: [macos, android]
---

## Install Java

``` shell
brew cask install java
```

You can use the following command to verify which version it will install.
```shell
brew cask info java
```

## Install Android SDK

Goto [Android Command Line Tools](https://developer.android.com/studio/)
Scroll down to the Command line tools only section:

[Download Android Command Line Tools for Mac](https://dl.google.com/android/repository
/sdk-tools-darwin-4333796.zip)

Speed up the download process with the following command:
```shell
curl https://dl.google.com/android/repository/sdk-tools-darwin-4333796.zip \
                                -o ~/software/sdk-tools-darwin-4333796.zip
```

Unzip sdk-tools-darwin-4333796.zip to directory $HOME/Library/Android/sdk

Set PATH variable
```
export PATH=$HOME/Library/Android/sdk/tools/bin:$PATH
export PATH=$HOME/Library/Android/sdk/platform-tools:$PATH
```

Before using [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager)
to download the desired version of SDK, slight modification need to made to the sdkmanager
script to avoid [NoClassDefFoundError](https://stackoverflow.com/a/47150411/5411817),
credit goes to [Siu Ching Pong -Asuka Kenji](https://stackoverflow.com/users/142239/siu-ching-pong-asuka-kenji)

Find and replace the follow:
```shell
DEFAULT_JVM_OPTS='"-Dcom.android.sdklib.toolsdir=$APP_HOME”’
```
with:
```shell
DEFAULT_JVM_OPTS='"-Dcom.android.sdklib.toolsdir=$APP_HOME" -XX:+IgnoreUnrecognizedVMOptions --add-modules java.se.ee'
```

### Install platform-tools
Android SDK only install basic tools such as sdkmanager, apkanalyzer, monkeyrunner,
but commands such as adb, systrace are not included, using sdkmanager to install
latest version of platform-tools as follows:
```shell
sdkmanager "platform-tools" "platforms;android-28"
```

sdkmanager -- list to list installed and available packages

sdkmanager -- help for more

## References
- [How to install Java 8 on Mac](https://stackoverflow.com/questions/24342886/how-to-install-java-8-on-mac)

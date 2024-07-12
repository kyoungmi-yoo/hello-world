# Quick Guide for building a flutter-webOS app

Follow the procedure below to build your flutter app into a flutter-webOS app.

## Set up the development environment


### 1. Update Linux utilities

``` shell
$ sudo apt-get upgrade -y
$ sudo add-apt-repository ppa:git-core/ppa -y
$ sudo apt-get update
$ sudo apt-get install curl unzip cmake pkg-config file libgtk-3-0 libgtk-3-dev git ninja-build clang -y
```

> [!Note]
> This is tested on the 64-bit version noteof Ubuntu Long Term Support (LTS) releases, including:
>- Ubuntu 18.04.6 LTS (Bionic Beaver) 64-bit
>- Ubuntu 22.04.3 LTS (Jammy Jellyfish) 64-bit

### 2. Check the git version

The version should be 2.23 or higher.

``` shell
$ git --version
git version 2.45.2
```

### 3. Configure the git settings

``` shell
$ git config --global user.name "{Your Name}"
$ git config --global user.email "{Your Email}"
$ cat ~/.gitconfig
[user]
name = {Your Name}
email = {Your Email}
```

### 4. Configure the ssh settings.

``` shell
$ cat ${HOME}/.ssh/config
 
Host TBD(wall.lge.com)
User  [User Name]
Port 29448
```

## Install flutter

Install flutter **3.13.9**, which is the version supported by flutter-webOS, although the latest version of flutter supported is 3.22.2. 

You **MUST** install the same-version flutter (3.13.9) as flutter-webOS. 

``` shell
$ git clone https://github.com/flutter/flutter.git
$ cd flutter
 
// reset to flutter 3.13.9 
flutter$ git reset --hard d211f42
HEAD is now at d211f42860 [flutter_releases] Flutter stable 3.13.9 Framework Cherrypicks (#137284)
flutter$ export PATH="$PATH:${HOME}/flutter/bin"
 
// check the information about installed tools
flutter$ flutter doctor -v
flutter$ flutter precache -f
```

## Install flutter-webOS

### 1. Download and install webOS NDK

``` shell
// download webOS NDK
 
$ wget TBD (https://cart.lge.com/artifactory/flutter-webos/NDK/webos-ndk-basic-starfish-x86_64-ombre-12.sh  와 같은 webOS NDK download 받을 수 있는 주소로 변경필요)
 
// install webOS NDK
$ chmod +x webos-ndk-basic-starfish-x86_64-ombre-12.sh
$ ./webos-ndk-basic-starfish-x86_64-ombre-12.sh
webOS TV SDK installer version tc18-2023.01-12
==============================================
Enter target directory for SDK (default: /usr/local/starfish-bdk-x86_64):${HOME}/starfish-bdk-x86_64
You are about to install the SDK to "/home/worker/starfish-bdk-x86_64". Proceed [Y/n]? Y
Extracting SDK...done
Setting it up...done
SDK has been successfully set up and is ready to be used.
Each time you wish to use the SDK in a new shell session, you need to source the environment setup script e.g.
 $ . /home/worker/starfish-bdk-x86_64/environment-setup-ca9v1-starfishmllib32-linux-gnueabi
// run ndk environment setup script 
$ . /home/worker/starfish-bdk-x86_64/environment-setup-ca9v1-starfishmllib32-linux-gnueabi
export WEBOS_FLUTTER_NDK_ENV="${HOME}/starfish-bdk-x86_64/environment-setup-ca9v1-starfishmllib32-linux-gnueabi"
```

### 2. Install flutter-webOS SDK

``` shell
$ git clone TBD(ssh://wall.lge.com:29448/module/flutter-webos 와 같은 flutter-webos cli private github로 변경필요)
$ cd flutter-webos
flutter-webos$ export PATH="$PATH:${HOME}/flutter-webos/bin"
  
// check the information about the installed tools
flutter-webos$ flutter-webos doctor -v
flutter-webos$ export WEBOS_ENGINE_BASE_URL="TBD(https://cart.lge.com/artifactory/flutter-webos/releases에 대응되는 주소)"
flutter-webos$ flutter-webos precache -f
```

## Configure the flutter and flutter-webOS settings

### 1. Add `PATH` to `.bashrc`

> [!Caution]
> Restart the terminal before executing the following commands.

``` shell

$ vi ~/.bashrc
 
...
# Flutter webOS git repo
if [ -d "$HOME/flutter-webos/bin" ] ; then
    PATH="$HOME/flutter-webos/bin:$PATH"
fi
  
# Flutter git repo
if [ -d "$HOME/flutter/bin" ] ; then
    PATH="$HOME/flutter/bin:$PATH"
fi
 
# Flutter NDK environment
if [ -f "${HOME}/starfish-bdk-x86_64/environment-setup-ca9v1-starfishmllib32-linux-gnueabi" ]; then
  export WEBOS_FLUTTER_NDK_ENV="${HOME}/starfish-bdk-x86_64/environment-setup-ca9v1-starfishmllib32-linux-gnueabi"
fi
 
# webOS engine base url : This is used to download/install webOS specific files such as webos-common.zip, webos-arm-*.zip
# installed flutter-webos/flutter/bin/cache/artifacts/engine/
export WEBOS_ENGINE_BASE_URL="TBD(https://cart.lge.com/artifactory/flutter-webos/releases에 대응되는 주소)"
```

### 2. Verify that the flutter and flutter-webOS have the same version

``` shell
// check the version of flutter: 3.13.9
$ flutter --version
Flutter 3.13.9 • channel master • https://github.com/flutter/flutter.git
Framework • revision d211f42860 (9 months ago) • 2023-10-25 13:42:25 -0700
Engine • revision 0545f8705d
Tools • Dart 3.1.5 • DevTools 2.25.0
 
// check the vesion of flutter-webOS: 3.13.9
$ flutter-webos --version
Flutter 3.13.9 • Embedder webos1
Framework • revision d211f42860 (9 months ago) • 2023-10-25 13:42:25 -0700
Engine • revision 0545f8705d
Tools • Dart 3.1.5 • DevTools 2.25.0
Tools • flutter-webos revision d070e14
```

### 3. Check the information about the installed tools

Check the information about the installed tools using the `doctor` option

``` shell
// check information about the installed tools
$ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel master, 3.13.9, on Ubuntu 18.04.6 LTS 5.15.0-60-generic, locale en_US.UTF-8)
...
[✓] Chrome - develop for the web
[✓] Linux toolchain - develop for Linux desktop
....
 
$ flutter-webos doctor
Doctor summary (to see all details, run flutter doctor -v):
[!] Flutter (Channel [user-branch], 3.13.9, on Ubuntu 18.04.6 LTS 5.15.0-60-generic, locale en_US.UTF-8)
    ! Flutter version 3.13.9 on channel [user-branch] at /home/worker/flutter-webos/flutter
      Currently on an unknown channel. Run `flutter channel` to switch to an official channel.
      If that doesn't fix the issue, reinstall Flutter by following instructions at https://flutter.dev/docs/get-started/install.
    ! Warning: `flutter` on your path resolves to /home/worker/flutter/bin/flutter, which is not inside your current Flutter SDK checkout at /home/worker/flutter-webos/flutter. Consider adding
      /home/worker/flutter-webos/flutter/bin to the front of your path.
    ! Warning: `dart` on your path resolves to /home/worker/flutter/bin/dart, which is not inside your current Flutter SDK checkout at /home/worker/flutter-webos/flutter. Consider adding
      /home/worker/flutter-webos/flutter/bin to the front of your path.
...
[✓] webOS NDK toolchain - develop for webOS (ombre)
...
[✓] Chrome - develop for the web
...
```

- If Chrome is not installed, install the Linux version of [Google Chrome](https://www.google.com/chrome/dr/download/) to use **DevTool**. 

  Refer to [Start building Flutter web apps on Linux](https://docs.flutter.dev/get-started/install/linux/web).
- If the Linux toolchain is not installed, install the Linux toolchain to use Linux descktop target build. 

  Refer to [Start building Flutter native desktop apps on Linux](https://docs.flutter.dev/get-started/install/linux/desktop).


## Create a flutter-webOS project

``` shell
$ flutter-webos create --platforms webos helloworld
```

- If you want to check the GUI on other platforms, add them next to `--platforms webos` using comma (,) as a delimiter.

  See the example below.

  | Platform          | Command                                                                    |
  |-------------------|----------------------------------------------------------------------------|
  | webOS             | flutter-webos create --platforms webos helloworld                          |
  | webOS and Linux   | flutter-webos create --platforms webos,linux helloworld                    |
  
  Or you can also create a project for webOS first and add Linux, or vice versa.

  ``` shell
  $ flutter-webos create --platforms webos helloworld
  $ cd helloworld
  $ flutter-webos create --platforms linux
  ```

  ``` shell
  $ flutter-webos create --platforms linux helloworld
  $ cd helloworld
  $ flutter-webos create --platforms webos
  ```

  Refer to [Add desktop support to an existing Flutter app](https://docs.flutter.dev/platform-integration/desktop#add-desktop-support-to-an-existing-flutter-app).

- In addition, you can check the GUI on the Linux descktop target using the `run` command.

  ``` shell
  $ cd helloworld
  $ flutter run -d linux
  ```

## Build and package the flutter-webOS app 

``` shell
// move to the project directory and execute `flutter-webos build`
$ cd helloworld
 
// clean build in source directory
helloworld$ flutter-webos -v clean                       
 
// build with debug mode and make ipk
helloworld$ flutter-webos build webos --ipk --debug
 
// Or you can build with release mode build and make ipk
helloworld$ flutter-webos build webos --ipk
```

- The output will be located in `build/webos/[TARGET_ARCH]/[BUILD_MODE]/ipk` under the projec directory.

    (e.g. `build/webos/arm/release/ipk`, `build/webos/arm/debug/ipk`)

- If you used flutter-webOS plugins in your app, you need to set package dependencies. See [How to build an app with flutter-webOS plugins](#how-to-build-an-app-with-flutter-webos-plugins).

- If you want to change the app id of the ipk to be generated, change `id` in the `appinfo.json` file before making the ipk. The `appinfo.json` file is located in `webos/meta/appinfo.json` under the project directory.

## Configure the flutter-webOS device settings

### 1. Prerequisite

- Check your `appinfo.json`.

    1. The app `id` should not start with `com.webos` or `com.lge`.
    2. The value of `trustLevel` cannot be `trusted`.

    Note that this is required to install the app in Developer Mode and is not a restriction on normal flutter apps.

- Enable the dev mode of your target device.

    ``` shell
    $ touch /var/luna/preferences/devmode_enabled
    $ touch /var/luna/preferences/debug_system_apps
    $ sync && reboot -f
    ```

### 2. Add your webOS TV device as a custom device

Make sure that the device is reachable via SSH.

``` shell
$ flutter-webos config --enable-custom-devices
$ flutter-webos custom-devices add
Please enter the id you want to device to have. Must contain only alphanumeric or underscore characters.
(example: webos)
tv
Please enter the hostname or IPv4 address of the device.
(example: 192.168.0.100)
192.168.1.72
Please enter the port number for ssh connection.
(example: 22, empty for 22)
 
Please enter the path to the identity (private key) file, if necessary
(example: /tmp/dede.pem, empty for )
 
Would you like to add the custom device to the config now? [Y/n] (empty for default)
 
Successfully added custom device to config file at "/home/worker/.config/flutter/custom_devices.json".
```

- To delete the added device from the config file, execute the following:

  ```shell
  $ flutter-webos custom-devices reset
  ```
  or
  ```shell
  $ flutter-webos custom-devices delete -d tv
  ```

- Verify if the device is successfully added.

  ```shell
  // run it on your app project directory.
  $ flutter-webos devices
  3 connected devices:  
 
  Linux (desktop) • linux       • linux-x64      • Ubuntu 18.04.6 LTS 5.15.0-60-generic
  Chrome (web)    • chrome      • web-javascript • Google Chrome 126.0.6478.126
  webOS (mobile)  • tv          • flutter-tester • tc18-2023.01-12
  ```

## Install and run the flutter-webOS app

Install and run the flutter-webOS app using the `run` command

```shell
$ cd helloworld
helloworld$ flutter-webos run -d tv
Launching lib/main.dart on webOS in debug mode...
Uninstall com.flutter.app.helloworld from tv.
Success command
InstallApp : com.flutter.app.helloworld to tv.
Success command
runDebug Success
Launch Succeeded helloworld on tv
Syncing files to device webOS...                                  
 
 Flutter run key commands.
r Hot reload. 🔥🔥🔥
R Hot restart.
h List all available interactive commands.
d Detach (terminate "flutter run" but leave application running).
c Clear the screen
q Quit (terminate the application on the device).
 
💪 Running with sound null safety 💪
 
An Observatory debugger and profiler on webOS is available at: http://127.0.0.1:39379/v4aQypYL4_s=/
The Flutter DevTools debugger and profiler on webOS is available at: http://127.0.0.1:9100?uri=http://127.0.0.1:39379/v4aQypYL4_s=/
```

## How to build an app with flutter-webOS plugins

If you used flutter-webOS plulgins in your app, you need to specify the package dependencies in the `pubspec.yaml` under the app source folder. 

Refer to [Package dependencies](https://dart.dev/tools/pub/dependencies).

The following example is when using the `webos_logger` flutter-webOS plugin. 

```shell
dependencies:
 
  webos_logger:
   git:
     url: TBD (ssh://wall.lge.com:29448/module/flutter-webos-plugins 에 해당하는 flutter webOS plugin private github 주소로 변경 필요)
     path: packages/webos_logger/
     ref: master
```

- You can get a specific version using `git hash` at `ref:` or specify a version with a branch or tag.
- You can find examples of using each plugin under the `examples` folder.
- The list of flutter-webOS plugins supported by webOS TV is available at [How To Build Your Flutter App for webOS TV](http://collab.lge.com/main/display/WITHTCS/How+To+Build+Your+Flutter+App+for+webOS+TV). 

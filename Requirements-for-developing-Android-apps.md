To build and run apps on Android Simulator, you must:

- Have Apache ant installed (to find out whether you already have it, run
  `which ant` from the command line). We develop with ant 1.8.x (`ant -version`)
  http://ant.apache.org

- Download [Android SDK package](http://developer.android.com/sdk/index.html)
  for your platform. If you've run the OSX installation script, the Android SDK is installed for you via homebrew.

- Edit your shell's loading files (`~/.bashrc` or `~/.bash_profile` for bash and
   `~/.zshrc` for zsh) and add the Android SDK platform tools to your path. For
   example:

	  `export PATH=$PATH:/home/yourname/android-sdk/tools:/home/yourname/android-sdk/platform-tools`

- Open a new terminal and run the SDK manager:

	  `android`

  You should install/upgrade:

  - "Tools"
    - "Android SDK Tools" (latest revision)
    - "Android SDK Platform-tools" (latest revision)
  - Android 2.2, 2.3.x, 2.n (up to latest 2.x revision)
    - SDK Platform
    - Google APIs by Google Inc.
  - SDK Platform Android 3.x (if you want to play with Android tablet)
    - SDK Platform
    - Google APIs by Google Inc.
  - SDK Platform Android 4.x (if you want to play with Ice Cream Sandwich, although this is not officially supported by Mulberry)
    - SDK Platform
    - Google APIs by Google Inc.

It should look [something like this](images/androidsdk.png).

## Creating an Android Virtual Device

If you do not have access to an Android device, you may want to create a
virtual device for testing. To do this, run the SDK manager by running
`android` on the command line, then:

- On the "Virtual Devices" section click the "New..." button
- Name the device
- Select the Google APIs - API Level 8 (to test 2.2) as the target
- Make the SD card size 64 MB
- Click "Create AVD"

Now you can run the device by running `emulator @your-device-name -partition-size 128`

If you receive an error like:

`emulator: ERROR: unknown skin name`

You must edit your .bash_profile (or .profile, etc.) to create an environment variable:

`export ANDROID_SDK_ROOT=/usr/local/Cellar/android-sdk/r16/`

Note this path is specific to your installation of the android SDK. If you are on OS X, it should be similar to that (although r16 may be r17, 18, etc. as the version increases). If you are on another platform, the path is up to you.

To rotate the device hit 7 or 9 on your numeric keypad. If you don't have one:
CTRL-F12 to rotate to landscape, CTRL-F11 to rotate back.

### Installing to your Android Virtual Device

Start the emulator, then type:

    adb install -r /path/to/your.apk
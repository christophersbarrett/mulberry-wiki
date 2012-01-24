To build and run apps on Android Simulator, you must:

- Have Apache ant installed (to find out whether you already have it, run
  `which ant` from the command line). We develop with ant 1.8.x (`ant -version`)
  http://ant.apache.org

- Download [Android SDK package](http://developer.android.com/sdk/index.html)
  for your platform. If you've run the OSX installation script, the Android SDK is installed for you via homebrew.

- If you did not run the installation script and wish to install Android SDK on your own, the preferred location for the SDK is
  `/Developer/SDKs/android-sdk-mac_x86/`. If you do not place it in this
  directory, Mulberry _should_ detect the location of it, but no guarantees.

- Edit your shell's loading files (`~/.bashrc` or `~/.bash_profile` for bash and
   `~/.zshrc` for zsh) and add the Android SDK platform tools to your path. For
   example:

	  `export PATH=$PATH:/Developer/SDKs/android-sdk-mac_x86/tools:/Developer/SDKs/android-sdk-mac_x86/platform-tools`

- Open a new teminal and run the SDK manager:

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

- You do not need, but may choose, to install the Samples and Documentation.
  It's pretty useless and just takes up space.

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

To rotate the device hit 7 or 9 on your numeric keypad. If you don't have one:
CTRL-F12 to rotate to landscape, CTRL-F11 to rotate back.

### Installing to your Android Virtual Device

Start the emulator, then type:

    adb install -r /path/to/your.apk
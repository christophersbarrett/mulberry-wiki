To build and run apps on Android Simulator, you must:

- Have Apache ant installed (to find out whether you already have it, run
  `which ant` from the command line). We develop with ant 1.8.x (`ant -version`)
  http://ant.apache.org

- Download [Android SDK package](http://developer.android.com/sdk/index.html)
  for your platform. On OSX, the preferred location for the SDK is
  `/Developer/SDKs/android-sdk-mac_x86/`. If you do not place it in this
  directory, Mulberry _should_ detect the location of it, but no guarantees.

- Edit your shell's loading files (`~/.bashrc` or `~/.bash\_profile` for bash and
   `~/.zshrc` for zsh) and add the Android SDK platform tools to your path. For
   example:

	  `export PATH=$PATH:/Developer/SDKs/android-sdk-mac_x86/tools:/Developer/SDKs/android-sdk-mac_x86/platform-tools`

- Open a new teminal and run the SDK manager:

	  `android`

- Click "Available packages"

- Expand "Android Repository"

  You should install:

  - "Android SDK Tools" (latest revision)
  - "Android SDK Platform-tools" (latest revision)
  - SDK Platform Android 2.2, 2.3.x, 2.n (up to latest revision)
  - SDK Platform Android 3.x

  Expand "Third party Add-ons", then install:

  - Google APIs 8, 9 (up to latest revision)

  Be careful, as some tools require certain revisions, so if you see
  "Skipping 'X'; it depends on 'Y'" you'll have to go back and choose X again.

  Keep doing this until you've installed everything.

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
# Install guide

Here you can find out how to install Minigotchi on the ESP8266. Installing on the Pico is not possible as the code is meant for the ESP8266.

**For building on an ESP32, see [this](https://github.com/Pwnagotchi-Unofficial/minigotchi-ESP32/blob/development/INSTALL.md). Don't use this repository for the ESP32. The Wio Terminal is still a WIP.**

## Building using Arduino IDE

### Step 1: Configuration

- Download the latest release [here](https://github.com/Pwnagotchi-Unofficial/minigotchi/releases).

- Unzip the file

- Navigate to the `config.cpp` file in the unzipped folder, and open it up with the text editor of your choice. Configurations are always controlled here.

**If any of these lines are not in your config, such as screens, do not add them. That information is for testers on the development branch. Only modify the config values you have. Some of the data types for these values have changed, keep them as is in your config.**

- At the set of lines that says:

```cpp
// define if features will be used
bool Config::deauth = true;
bool Config::advertise = true;
bool Config::scan = true;
```

From here, you can turn off deauthing, advertising, or Pwnagotchi scanning. Since these are the main features of the minigotchi, I highly recommend you don't turn any of these off.

- With `Config::shortDelay` and `Config::longDelay`, we can adjust the speed of our Minigotchi.

```cpp
// define universal delays
int Config::shortDelay = 500;
int Config::longDelay = 5000;
```

A `Config::shortDelay` is a shorter delay used in between doing tasks, though sometimes too many of them can be problematic. A `Config::longDelay` is a longer delay which isn't used as much but it often used before and/or after something major, to allow the user to read serial/screen output. I put down the recommended default values but you can also change them to whatever you like.

- We can also enable parasite mode, which requires the [minigotchi plugin](https://github.com/matrix224/pwnagotchi_plugins/tree/main). This will allow our Minigotchi to communicate with a Pwnagotchi over serial connection to the Pwnagotchi.

```cpp
bool Config::parasite = false;
```

It's false by default, but you can enable it by making it `true`.

- After that, there should be a line that states the baud rate.

```cpp
// baud rate
int Config::baud = 115200;
```

Here, you can adjust the baud rate, which is of course optional. Make sure your serial terminal is running at the same baud rate as the Minigotchi.

- Here, we can adjust the WiFi channel we start on. This doesn't need to be modified.

```cpp
// define init channel
int Config::channel = 1;
```

- After this, we can configure our screen (Not in any version `<= 3.0.1`)

```cpp
// screen configuration
bool Config::display = false;
std::string Config::screen = "";
```

These screen types are available:

- `SSD1306`

- `WEMOS_OLED_SHIELD`

- `IDEASPARK_SSD1306`

- `SSD1305`

- `SH1106`

Set `bool Config::display = false;` to true, and `std::string Config::screen = "<YOUR_SCREEN_TYPE>";` to one of those screen types if your screen is supported.

- There should also be a line that says:

```cpp
// define whitelist
std::string Config::whitelist[] = {"SSID", "SSID", "SSID"};
```

This defines our whitelist. The Minigotchi will not deauth these access points/any networks corresponding to that access point name. We can have up to ten values here. Three were added as an example, you can remove those.

- Here, we set the channels we hop from time to time.

```cpp
// define channels
int Config::channels[13] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13};
```

Usually, this shouldn't be changed as these are the only channels we can access on the ESP8266.

- Save and exit the file when you have configured everything to your liking. Note you cannot change this after it is flashed onto the board.

### Step 2: Building and flashing

- Download the [Arduino IDE](https://https://www.arduino.cc/en/software), preferably the latest version.

- In the IDE, go to `File` > `Preferences`, then in the `Additional board manager URLS`, paste the link `https://raw.githubusercontent.com/SpacehuhnTech/arduino/main/package_spacehuhn_index.json` to add the ESP8266 board(s) to the board manager.

- Now go to `Tools` > `Board` > `Boards Manager` and search `deauther` and click `Install` on `Deauther ESP8266 Boards`

- Click `Ok` and plug the board into your computer. It should blink, and make sure it is receiving the correct amount of voltage, too much will burn it(I learned that the hard way)

- Open up the Minigotchi folder through the IDE by pushing `Ctrl+O`, or by going to `File` > `Open`, then selecting the folder the .ino is in. If other tabs don't show up, along with `minigotchi.ino`, make sure to copy and paste the files into the same directory/folder as that lone `.ino` file. It won't be able to compile if that happens.

- Install the following dependencies (with their dependencies as well) with the library manager:
  - `ArduinoJson`
  - `Adafruit GFX`
  - `Adafruit SSD1306`
  - `Adafruit SSD1305`
  - `u8g2`
  - Remove/uninstall/modify your screen library depending on your display for `Config::screen` below, some of these dependencies may have already been installed previously.
  - Sometimes the repository owner of certain libraries may require you to install it a certain way, be sure to follow their guides if needed/included.

| `SSD1306`                                                                          | `WEMOS_OLED_SHIELD`                                                                | `IDEASPARK_SSD1306`        | `SSD1305`                  | `SH1106`                   |
| ---------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- | -------------------------- | -------------------------- | -------------------------- |
| Install `Adafruit SSD1306`, remove `Adafruit SSD1306 Wemos Mini OLED` if installed | Install `Adafruit SSD1306 Wemos Mini OLED`, remove `Adafruit SSD1306` if installed | Install the `u8g2` library | Install `Adafruit SSD1305` | Install the `u8g2` library |

**Note: You will need to install all of these libraries, but I put them in a table here to organize the screens.**

- Go to `Tools` > `Board` and use one of the boards in the `Deauther ESP8266 Boards` section. (Example: `LOLIN(WEMOS) D1 mini`)

- Select your COM port/Serial port through `Tools` > `Port` where the ESP8266 is plugged in

- Click on the upload button(arrow pointing to the left). If you see any errors that you cannot solve, feel free to make an [issue](https://github.com/dj1ch/minigotchi/issues).

**OR**

- Go to `Sketch` > `Export Compiled Binary` to generate a `.bin` file to flash to the board, which also works. This can also be done with `Alt+Ctrl+S`.

**Note: if you get any errors, let me know ASAP with a GitHub issue, sending me a discord message, or perhaps even messaging me through my portfolio site.**

### Step 3: Post Install

- You can click the _Serial Monitor_ button on the top bar to see the serial monitor output. Make sure the baud rate is `115200`. You can also use https://serial.huhn.me/ as a serial monitor, I also recommend using this!

- I also highly recommend doing the following:

1. Disabling AI on your Pwnagotchi so you can change your settings without AI changing them.

2. Setting `personality.channels[]` in your Pwnagotchi's `/etc/pwnagotchi/config.toml` to match your `Config::channels[13]` so that your Minigotchi has a higher chance of finding your Pwnagotchi.

- Happy hacking!

###

## Tutorial

### Before the start

If you need to use the program but cannot compile it, you can go to the [Releases](https://github.com/MonokoTech/EasyRPG-Player-Monoko/releases) to download precompiled binaries. You need to choose the architecture that is suitable for the system you are currently using.

Open the terminal, which is cmd.exe for windows and Terminal.app for macOS.

Use the file manager to drag the [game folder](#game-folder) to the terminal.

Add `cd ` before the game folder. In cmd.exe, use `cd /d ` instead (where `/d` means switching drives).

You need to find the location of Player,
 then run either `<absolute path>/Player.exe` or `<absolute path>/EasyRPG Player.app/Contents/MacOS/EasyRPG\ Player` for macOS.

The first run will display in full screen. If you don't like it, you can press F1 and select Video -> Fullscreen as OFF.

Finally go back and press `<Save Settings>` once, and return to the game menu.

Okay, you can press TAB to bring up the ChatUi, and F3 to toggle notifications.

### P2P support

This can be done using [n3n](https://github.com/n42n/n3n) or [n2n](https://github.com/ntop/n2n).

On Linux, manually creating a TAP device can prevent n2n from automatically creating a TAP device.
 This helps in running n2n as a non-root user.

```
edge -f -d <tap-device-name> \
	-c community -k happyn \
	-l "supernode-address:port"

Running as a non-root user requires specifying this: -u $(id -u) -g $(id -g)
Always use the supernode via TCP: -S2
Automatically disable multicast: -e auto
Prevent some meaningless prints: --no-port-forwarding
```

Windows can use [happynwindows](https://github.com/happynclient/happynwindows/releases).
 Currently, the n2n on Windows does not support -S2 option.

If you need to stay connected to the supernode, you can use ncat to broadcast empty messages at 1 second intervals.
 To get ncat from [nmap](https://nmap.org/download).

Broadcast at intervals example (255.255.255.255 can be replaced with 10.226.123.255 if metrics are not set):
```
# Linux/MacOS (Save as .sh file)
while true; do echo | ncat -u 255.255.255.255; sleep 1; done

:: Windows (Save as .bat file)
:loop
echo | ncat -u 255.255.255.255
timeout /t 1 >nul
goto loop
```

The community name `community` and the password `happyn` are the default configurations for happynwindows.
 Using this community name will generate a LAN ip of 10.226.123.0/24.
 (The community name determines the LAN ip).

On Windows, if [ForceBindIP](https://r1ch.net/projects/forcebindip) does not work, you can adjust the metrics of the interfaces.

Windows interface metric settings:
```
:: Use `netsh interface show interface` to list interfaces
:: Use `route print` to check metrics
:: Systems in different languages will display interface names based on your language
:: The tap of n2n will have a lower metric

:: Default interface
netsh interface ipv4 set interface interface="Ethernet" metric=500
:: n2n tap
netsh interface ipv4 set interface interface="Ethernet 2" metric=1
```

**Note that using a P2P connection may lead to IP leakage. Therefore, you can use a proxy in front.**

Use `ncat -u 127.0.0.1 5644` to check the status, press Enter to print.

When you see "pSp", it means that p2p intranet penetration is not supported and that it is forwarded through a supernode.

If the P2P connection has been established, start the epmp server, and then the other party can connect using `!c 10.226.123.x`.

The ideal state of applying the client/server model in P2P is having only two people.
 Better epmp P2P support will be available in the future.

### Client commands

`!help`

Show usage.

`!server [on, off]` (alias !srv)

You can run the server with this command.

As long as Player.exe can be run, the server can be started.

Servers can be started within a LAN (e.g. same WiFi or VPN), whether on mobile or computer.

`!crypt [Password]`

Toggle connection encryption. (See [crypt](#crypt))

`!connect [address]` (alias !c)

You can connect to the server with this command.

Enter the LAN IP, public IP, or domain name to connect to the server.

The status remains as "Connected", then it can be considered available.

`!disconnect` (alias !d)

Disconnect.

`!name [text]`

If you set !name \<unknown\> it will revert to the empty name.

`!chat [LOCAL, GLOBAL, CRYPT]` [CRYPT Password]

Switch chat visibility.

Enter !chat [CRYPT](#crypt) [password] and a key will be generated.

After that, only clients with the same password can see the chat messages.

If the current visibility is CRYPT, it can be changed to other visibility, such as GLOBAL.

Enter !chat CRYPT again, no password is required and you can switch back to CRYPT.

`!log [LOCAL, GLOBAL, CRYPT, VERBOSE]`

Enter a visibility to !log to hide or show messages in that visibility.

`!immersive` (alias !imm)

Toggle the immersive mode.

You can save the settings for the above commands via F1 -> \<Save Settings\>.
 The settings of the !connect, !name, !chat, and !immersive commands will be saved.

Some commands may not be listed. You can check with `!help`

### Dedicated server

Player.exe --server --bind-address 0.0.0.0[:port] --config-path /path/to/folder

Player.exe needs to be renamed with the corresponding executable filename based on your system.

Standalone server binary:

```
usage: easyrpg-player-server [options]
-n, --no-heartbeats
-a, --bind-address
-A, --bind-address-2
-U, --max-users
```

The -A, --bind-address-2 option can be ignored. It is a legacy from when it was used to start an IPv6 server.

Now you can bind both IPv4 and IPv6 simultaneously using a format like [::]:6500

For WebSocket, the server port only supports unencrypted and uncompressed WebSocket protocols.

Quickly set up from source code on Ubuntu Server:
```
# Compile
sudo apt-get install ca-certificates build-essential cmake git libfmt-dev screen
cmake -B build -DBUILD_CLIENT=off -DBUILD_SERVER=on -DCMAKE_BUILD_TYPE=Release
cmake --build build

# Run in the background
screen -S epmp
./build/easyrpg-player-server
# Press Ctrl+A D
# If you need to switch back, enter `screen -r`
# Press Ctrl+A ESC to start copy mode, and use Ctrl+U/D to scroll screen, and press Q to cancel

# Debug the server
sudo apt install gdb
# Set -DCMAKE_BUILD_TYPE=Debug, then compile
# gdb --args ./build/easyrpg-player-server
```

### Compile on linux

Arch Linux
```
# Install compilation tools
pacman -S gcc-libs cmake ccache

# Install dependencies of liblcf
pacman -S expat icu

# Install Player dependencies
# The first line is required, the second line is optional
pacman -S sdl2 pixman libpng zlib fmt
pacman -S harfbuzz mpg123 wildmidi libvorbis opusfile libsndfile libxmp speexdsp fluidsynth

# Compile
# You can decide whether to compile a dedicated server by setting `-DBUILD_CLIENT=on -DBUILD_SERVER=off`
cmake -B build -DBUILD_CLIENT=on -DBUILD_SERVER=off -DCMAKE_BUILD_TYPE=RelWithDebInfo -DCMAKE_CXX_COMPILER_LAUNCHER=ccache -DPLAYER_MULTIPLAYER=on -DPLAYER_BUILD_LIBLCF=on -DPLAYER_BUILD_LIBLCF_GIT="https://github.com/MonokoTech/EasyRPG-liblcf" -DPLAYER_BUILD_LIBLCF_BRANCH="player-epmp-dev"

cmake --build build -j${$(getconf _NPROCESSORS_ONLN):-2}
```

### Compile on macOS

```
# Install Homebrew
See: https://brew.sh

# Install compilation tools
brew install cmake ccache

# Install dependencies of liblcf
brew install expat icu4c

# Install Player dependencies
# The first line is required, the second line is optional
brew install sdl2 pixman libpng zlib fmt
brew install freetype mpg123 wildmidi libvorbis opusfile libsndfile speexdsp

# Compile
ICU_ROOT=$(brew --prefix)/opt/icu4c cmake -B build -DPLAYER_MULTIPLAYER=on -DCMAKE_BUILD_TYPE=RelWithDebInfo -DPLAYER_WITH_OPUS=off -DCMAKE_CXX_COMPILER_LAUNCHER=ccache -DPLAYER_MULTIPLAYER=on -DPLAYER_BUILD_LIBLCF=on -DPLAYER_BUILD_LIBLCF_GIT="https://github.com/MonokoTech/EasyRPG-liblcf" -DPLAYER_BUILD_LIBLCF_BRANCH="player-epmp-dev"
cmake --build build -j${$(getconf _NPROCESSORS_ONLN):-2}
```

### Compile on Windows

Refer to: [Guide: How To Build EasyRPG Player on windows](https://community.easyrpg.org/t/guide-how-to-build-easyrpg-player-on-windows/1174)

### More compilation examples

You can check out the [cross-platform-compilation.yml](/.github/workflows/cross-platform-compilation.yml) file.


## Cheatsheet

### Game folder

The game folder should include many .lmu files and may also include RPG\_RT.exe

### CRYPT

This uses symmetric-key encryption to ensure that the traffic cannot be snooped.

### Translations

Download the [master.zip](https://github.com/MonokoTech/translations-yno/archive/refs/heads/master.zip) file and extract.

Find the language folder you need in `TyRPG-translations-yno`, then copy it to the game folder.
 Rename the copied folder to `Language` and restart the Player. A new option will appear in the menu and select the language.

To lock the language, pass --language \<name\> to the player. The required name is in the Language folder.

### Frame limiter

To enable the frame limiter, you need to turn off V-Sync.

For older laptops, it is recommended to limit the frame rate to below 20 fps
 to minimize fan noise.

Press F1 -> Turn V-Sync Off -> Change Frame Limter to 20

### Audio mute

Press the M key to toggle mute on and off.

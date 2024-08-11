This simple tutorial shows how to build a Windows executable from inside Windows Subsystem for Linux (WSL). In the WSL, Debian 12 will be used.
The software that will be built is [`ffmpeg`](https://ffmpeg.org/).

Open Windows Terminal. If no Linux is installed in WSL, run the following command to install Debian in WSL:

```terminal
wsl --install Debian
```

Or, simply run `wsl` to start currently installed Linux on WSL.

WSL will eventually launch Linux terminal. Update the OS && install build tools:

```bash
sudo apt update
sudo apt -y upgrade
sudo apt -y install build-essential
```

Install & configure [`Git`](https://git-scm.com/). Make sure to use your name and email address below:

```bash
sudo apt -y install git
git config --global user.name "Your name"
git config --global user.email x@y.com
git config --global init.defaultBranch main
```

Install cross compilation GCC compiler & necessary tools:

```bash
sudo apt install gcc-mingw-w64-x86-64-win32 mingw-w64-tools
```

Now run the following commands to build ffmpeg for Windows:

```bash
# Install other software necessary to compile ffmpeg
sudo apt install nasm

# Clone ffmpeg repository
git clone https://git.ffmpeg.org/ffmpeg.git ffmpeg
cd ffmpeg

# Configure ffmpeg
./configure --arch=x86_64 --target-os=mingw32 --cross-prefix=x86_64-w64-mingw32-

# Build ffmpeg
make -j$(nproc)

# Find Windows executables
ls -lath | grep exe
```

Exit WSL. Back in the Windows Terminal, create a directory & copy the binaries. Make sure to fix the source and destination locations.

```terminal
exit
$ffmpeg_dir = "C:\ffmpeg"
mkdir $ffmpeg_dir
copy .\ffmpeg\ffmpeg.exe $ffmpeg_dir
copy .\ffmpeg\ffprobe.exe $ffmpeg_dir
```

Now `ffmpeg` is accessible from `C:\ffmpeg\ffmpeg.exe`.

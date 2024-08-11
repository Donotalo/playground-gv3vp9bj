This simple tutorial shows how to build a Windows executable from inside Windows Subsystem for Linux (WSL). In the WSL, Debian will be used.
The software that will be built is [`ffmpeg`](https://ffmpeg.org/). This tutorial assumes the audience is comfortable using both Windows &
Linux terminals.

Open Windows Terminal. If no Linux is installed in WSL, run the following command to install Debian in WSL:

```terminal
wsl --install Debian
```

Or, simply run `wsl` to start currently installed Linux on WSL. The Linux commands below assumes Debian is installed.

WSL will eventually launch Linux terminal. Update the OS & install build tools:

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

Install GCC cross compiler & necessary tools:

```bash
sudo apt install gcc-mingw-w64-x86-64-win32 mingw-w64-tools
```

It will install `x86_64-w64-mingw32-gcc` in `/usr/bin/` which is the cross compiler. It can be used to compile other `gcc` based projects too. Run the
following commands to download the source code and configure `ffmpeg` for Windows:

```bash
# Install other software necessary to compile ffmpeg
sudo apt install nasm

# Clone ffmpeg repository
git clone https://git.ffmpeg.org/ffmpeg.git ffmpeg
cd ffmpeg

# Configure ffmpeg
./configure --arch=x86_64 --target-os=mingw32 --cross-prefix=x86_64-w64-mingw32-
```

The above command will build a very minimal version of `ffmpeg` which won't have much usefulness (like it doesn't support H.265 encoding). Use the
following command to enable some useful libraries:

```bash
# Install dependent software
sudo apt install libass-dev

# Configure with necessary library support
./configure --arch=x86_64 --target-os=mingw32 --cross-prefix=x86_64-w64-mingw32- --enable-libass  --enable-libbluray --enable-libdav1d --enable-libdavs2  --enable-libfdk-aac --enable-libkvazaar --enable-libmp3lame  --enable-libopencv  --enable-libopenh264  --enable-librav1e --enable-libtwolame --enable-libvvenc  --enable-libx264  --enable-libx265 --enable-openssl --enable-gpl --enable-nonfree
```

Build `ffmpeg` for windows:

```bash
# Build ffmpeg
make -j$(nproc)

# Find Windows executables
ls -lath | grep exe
```

Exit WSL. Back in the Windows Terminal, create a directory & copy the binaries. Make sure to fix the source and destination locations in the commands
below.

```terminal
exit
$ffmpeg_dir = "C:\ffmpeg"
mkdir $ffmpeg_dir
copy .\ffmpeg\ffmpeg.exe $ffmpeg_dir
copy .\ffmpeg\ffprobe.exe $ffmpeg_dir
```

`ffmpeg` is accessible from `C:\ffmpeg\ffmpeg.exe`.

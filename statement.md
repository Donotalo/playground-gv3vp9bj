This Tutorial shows a journey of building Windows binary from inside Windows Subsystem for Linux (WSL). In the WSL, Debian 12 will be used. The software that will be built is [`ffmpeg`](https://ffmpeg.org/).

Open Windows Terminal and run the following command to install Debian in WSL:

```Terminal
wsl --install Debian
```

After the installation, Linux terminal will be started.

Update the OS:

```bash
sudo apt update
sudo apt -y upgrade
```

Install [`Git`](https://git-scm.com/):

```bash
sudo apt -y install git
```

```bash
# Install cross compiler and tools
sudo apt install gcc-mingw-w64-x86-64-win32  mingw-w64-tools

# Install other software necessary to compile ffmpeg
sudo apt install nasm

# Clone ffmpeg repository
git clone https://git.ffmpeg.org/ffmpeg.git ffmpeg
cd ffmpeg

# Configure ffmpeg
./configure --arch=x86_64 --target-os=mingw32 --prefix="C:\ffmpeg" --cross-prefix=x86_64-w64-mingw32-

# Build ffmpeg
make -j$(nproc)

# Find Windows executables
ls -lath | grep ffmpeg
```

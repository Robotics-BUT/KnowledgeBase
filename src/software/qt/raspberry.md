# Getting Qt to work on Raspberry Pi

## Installing from repositories

## Building from source
This tutorial was inspired by the tutorial [here](https://www.tal.org/tutorials/building-qt-512-raspberry-pi).
We will be using Qt 5.15 and skipping useless modules. Also gamepad and multimedia with GStreamer should be working out of the box.

Install preprequisities by running
```bash
sudo apt-get update
sudo apt-get install git gdb cmake gcc build-essential libfontconfig1-dev libdbus-1-dev libfreetype6-dev libicu-dev libinput-dev libxkbcommon-dev libsqlite3-dev libssl-dev libpng-dev libjpeg-dev libglib2.0-dev libraspberrypi-dev
sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly gstreamer1.0-plugins-bad libgstreamer-plugins-bad1.0-dev gstreamer1.0-pulseaudio gstreamer1.0-tools gstreamer1.0-alsa libasound2-dev pulseaudio libpulse-dev libsdl2-dev libgles2-mesa-dev libgbm-dev libgl1-mesa-dev libglu1-mesa-dev mesa-common-dev libx11-dev
```

Download the sources from [here](http://download.qt.io/official_releases/qt/) in our case we'll be using `wget` and downloading the 5.15.1 version
```bash
wget http://download.qt.io/official_releases/qt/5.15/5.15.1/single/qt-everywhere-src-5.15.1.tar.xz
```

Decompress the archive by running
```bash
tar xf qt-everywhere-src-5.15.1.tar.xz
```

Setup build configurations for the Raspberry Pi
```bash
git clone https://github.com/oniongarlic/qt-raspberrypi-configuration.git
cd qt-raspberrypi-configuration && make install DESTDIR=../qt-everywhere-src-5.15.1
cd ..
```

Prepare the build environment
```bash
mkdir -p build
cd build
PKG_CONFIG_LIBDIR=/usr/lib/arm-linux-gnueabihf/pkgconfig:/usr/share/pkgconfig ../qt-everywhere-src-5.15.1/configure -platform linux-rpi4-v3d-g++ -v -opengl es2 -eglfs -no-gtk -no-xcb -opensource -confirm-license -release -reduce-exports -force-pkg-config -nomake examples -no-compile-examples -skip qtwayland -skip qtwebengine -skip qt3d -skip qtlocation -skip qtscript -skip qtsensors -skip qtserialport -skip qtspeech -skip qtcharts -skip qtvirtualkeyboard -skip qtquick3d  -qt-pcre -no-pch -ssl -evdev -system-freetype -fontconfig -glib -prefix /opt/Qt5.15 -qpa eglfs
```

And build it!
```bash
make -j4
sudo make install
```

### Notes
It is recommended to use swap - at least 1 GB. 
It is also recommended to use `tmux` for running the build for cases of SSH disconnections.
You should also cool the Raspberry Pi properly, a small fan is enough to lower the CPU temperature by 20 C!.
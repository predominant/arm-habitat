# arm-habitat

## Wifi

On disk image.

In `/boot/wpa_supplicant.conf`:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="networkname"
    psk="secret_password"
    key_mgmt=WPA-PSK
}
```

## SSH

On disk image.

`touch /boot/ssh`

## Memory

On disk image.

Change GPU memory to lowest (16Mb)

In `/boot/config.txt`

```
gpu_mem=16
```

## AFTER Boot

```
sudo su
dd if=/dev/zero of=/swapfile bs=1M count=512
mkswap /swapfile
swapon -v /swapfile
```

In `/etc/fstab`

```
/swapfile     swap         swap     pri=1               0     0
```

```sh
# System tools
sudo su -
apt-get update -y
apt-get install -y git tmux libarchive-dev autoconf libtool

# I don't know if both of these are required
apt-get install -y libsodium23 libsodium-dev

apt-get install -y libzmq-dev
apt-get install -y libzmq5

# libssl-dev is the wrong version on Debian Buster
#apt-get install -y libssl-dev
# This didn't help either
#apt-get install -y libssl1.0.2
apt-get install -y libssl1.0-dev
export OPENSSL_LIB_DIR=/usr/lib/arm-linux-gnueabihf

# Rust install (Proceed with default installation)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env

### OPTIONAL (If using sccache wrapper for rust compilation)
#cargo install sccache --features="all"

# Clone Protobuf
git clone https://github.com/protocolbuffers/protobuf.git
pushd protobuf
git submodule update --init --recursive
./autogen.sh
./configure
make && make install
popd

# Clone Habitat
git clone https://github.com/habitat-sh/habitat.git
pushd habitat/components/hab
export BUILD_PKG_TARGET=arm-linux
cargo build --release
popd
```

# References

* [RPi Network setup](https://howchoo.com/g/ndy1zte2yjn/how-to-set-up-wifi-on-your-raspberry-pi-without-ethernet)
* [RPi SSH Setup](https://howchoo.com/g/ote0ywmzywj/how-to-enable-ssh-on-raspbian-jessie-without-a-screen)
* [RPi Linux From Scratch](http://intestinate.com/pilfs/beyond.html#addswap)
* [Cloning SDCard on Mac](https://computers.tutsplus.com/articles/how-to-clone-raspberry-pi-sd-cards-using-the-command-line-in-os-x--mac-59911)
* [Shell Share](https://shellshare.net/)

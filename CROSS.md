
```sh
sudo su
apt-get install -y docker.io
systemctl start docker

apt-get install -y libc6-armel-cross libc6-dev-armel-cross binutils-arm-linux-gnueabi libncurses5-dev
apt-get install -y gcc-arm-linux-gnueabihf g++-arm-linux-gnueabihf
apt-get install -y flex texinfo unzip help2man libtool

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
rustup target add arm-unknown-linux-gnueabihf
cargo install cross

git clone https://github.com/habitat-sh/habitat.git
cd habitat

export BUILD_PKG_TARGET=arm-linux
cross build --target arm-unknown-linux-gnueabihf --release
```


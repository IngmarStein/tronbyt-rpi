# tronbyt-rpi

A daemon to fetch images from Tronbyt and display on a matrix LED display connected to a Raspberry Pi.

## Installation

In order to avoid flickering, follow these steps (taken from https://github.com/hzeller/rpi-rgb-led-matrix?tab=readme-ov-file#troubleshooting):

- Set `dtparam=audio=off` in `/boot/firmware/config.txt`
- Add `isolcpus=3` at the end of `/boot/firmware/cmdline.txt`
- Run

```
cat <<EOF | sudo tee /etc/modprobe.d/blacklist-rgb-matrix.conf
blacklist snd_bcm2835
EOF
sudo update-initramfs -u
```
- Reboot

Now build the Tronbyt client:

```sh
# Install dependencies
sudo apt-get update
sudo apt-get -y install git libwebp-dev libssl-dev

# Clone repository
git clone https://github.com/IngmarStein/tronbyt-rpi.git

# Build
cd tronbyt-rpi
make
```

## Running

```sh
# The Tronbyt URL ends in /next
sudo ./tronbyt ${TRONBYT_URL}
```

If you want to start this at startup, create `/etc/systemd/system/tronbyt.service` with the following content:

```
[Unit]
Description=Tronbyt

[Service]
ExecStart=<PATH_TO_TRONBYT> <TRONBYT_URL>
Restart=always

[Install]
WantedBy=multi-user.target
```

To start the newly created service, run `sudo systemctl start tronbyt`.

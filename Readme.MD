# Install boinc on debian and use ati

## 2020.05.14
I have installed boinc on my debian, and I find I can use ati to accelerate the calculation.

## Basic install
Follow the boinc website step, we just need to run:
```
sudo apt-get install boinc-client boinc-manager
```
(You can find more details at `https://boinc.berkeley.edu/wiki/Installing_BOINC`)

## OpenCL configuration
1.Download the `AMDGPU-PRO drivers` at https://www.amd.com/en/support/kb/release-notes/rn-pro-lin-20-q1
- Please pay attention that this link may not up-to-date, you can just search it at http://google.com/ with `AMDGPU-PRO drivers`
- Debian also use ubuntu version
2.Unpack the file, `unar` is one of my favorate command:
```
unar amdgpu-pro-19.50-990351-ubuntu-18.04.tar.xz
```
3.`cd amdgpu-pro-19.50-990351-ubuntu-18.04`
4.Install the openCL part of this driver by run:
```
./amdgpu-pro-install --opencl=legacy,pal --headless
```
5.Wait it complete and restart the computer

## Disable the officail start-up
Due to some strange problems, I strongle recommend you to stop it:
1. Disable the office boinc start-up:
If your system use `systemctl` to manager the startup applications, you can input:
```
sudo systemctl disable boinc-client.service
```
to disable the boinc official startup

## Enable your start-up
1. Basic prepare work
1.1 Firstly you need to choice your `boinc work directory`, 
the default directory is `/var/lib/boinc-client`(If you use `ls -all` at `/var/lib` you will find a link of `/var/lib/boinc-client` named `boinc`), and you can use this but I choice a home directory named `boinc` at `/home/boinc`:
```
sudo mkdir /home/boinc
sudo mkdir /home/boinc/boinc_work_dir
sudo chown -R boinc:boinc /home/boinc
```
- Note: `/home` directory may cause some safety problems!!

1.2 Test your system
Your should test your whole system right now by:
```
cd /home/boinc/boinc_work_dir
boinc
```
And the terminal will show just like:
```
Fri May 15 16:28:07 2020 |  | OpenCL: AMD/ATI GPU 0: AMD Radeon Graphics (driver version 3004.6, device version OpenCL 1.2 AMD-APP (3004.6), 2042MB, 2042MB available, 576 GFLOPS peak)
```
If you can just get `no usable gpu found`, you should run `OpenCL configuration` part again
If you get the message around `amd opencl`, it means you can use amd gpu now, input `sudo pkill -9 boinc` to stop this boinc progress.

2. Write your own start-up script
Input :
```
sudo vim /etc/systemd/system/boinc-user.service
```
to create the script, and fill it with:
```
[Unit]
Description=Sakura Frp Client
Wants=network-online.target
After=network-online.target syslog.target

[Service]
Type=simple
ExecStart=/usr/bin/boinc --dir /home/boinc/boinc_work_dir

[Install]
WantedBy=multi-user.target

```
- Please pay attention to the `--dir /home/boinc/boinc_work_dir`, this must be where you chose to be boinc work directory.
Save it and run:
```
sudo systemctl daemon-reload
sudo systemctl enable boinc-user.service
```
The first command reload the list and the next command enable our own boinc start-up script

3. Change the `boincmgr` data directory
You can just run:
```
boincmgr -d /home/boinc/boinc_work_dir
```
To start it or you can edit the `Path=...` part of `/usr/share/applications/boinc-manager.desktop`
- I use xfce4, if you can not find `/usr/share/applications/boinc-manager.desktop`, maybe you didn't install it or the matter of different desktop environment.
- Sometimes `boincmgr` can not connect to your `boinc`, try to stop `boinc` by run `sudo systemctl stop boinc-user.service`, and empty the `gui_rpc_auth.cfg` in your boinc work directory with `sudo`

## Enjoy it

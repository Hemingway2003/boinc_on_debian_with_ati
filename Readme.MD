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

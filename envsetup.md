sudo apt update<br>
sudo apt install build-essential libncurses-dev bison flex pahole libssl-dev libelf-dev libqt5gui5 libqt5widgets5 libqt5script5 libqt5xml5 libqt5x11extras5 libqt5dbus5 qtbase5-dev libqt5svg5-dev libncurses5-dev libcap-dev libacl1-dev libcrypt-dev libselinux1-dev libseccomp-dev systemd-coredump hwloc<br>

#for ebpf, xdp (cilium docs)<br>
sudo apt-get install -y make gcc libssl-dev bc libelf-dev libcap-dev clang gcc-multilib llvm libncurses5-dev git pkg-config libmnl-dev bison flex graphviz<br>

<br>#for perf src install from kernel src
<br>sudo apt install libzstd1 libdwarf-dev libdw-dev binutils-dev libcap-dev libelf-dev libnuma-dev python3 python3-dev python-setuptools libssl-dev libunwind-dev libdwarf-dev zlib1g-dev liblzma-dev libaio-dev libtraceevent-dev debuginfod libpfm4-dev libslang2-dev systemtap-sdt-dev libperl-dev binutils-dev libbabeltrace-dev libiberty-dev libzstd-dev<br>

Fonts in Ubuntu<br>
https://superuser.com/questions/1335155/patched-fonts-not-showing-up-on-gnome-terminal<br><br>

changing configs
use pop os!
1. kernel config

https://www.reddit.com/r/linuxquestions/comments/1dvp62g/comment/lf7cqtn/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button

https://stackoverflow.com/questions/67670169/compiling-kernel-gives-error-no-rule-to-make-target-debian-certs-debian-uefi-ce/72528175#72528175

https://unix.stackexchange.com/questions/717619/linux-mint-5-15-0-47-generic-error-out-of-memory-when-booting
```
cp /boot/config-<kversion> .config 
make olddefconfig
sudo make -j  $(nproc)
sudo make modules_install -j $(nproc)
sudo make install -j $(nproc)
sudo update-initramfs -c -k <kernelversion>  #smtimes these 2 are not needed
sudo update-grub
```



2. ebpf configs <br>
https://maofeichen.com/setup-the-extended-berkeley-packet-filter-ebpf-environment/
<br>bcc installation error modules not found <br>
https://github.com/iovisor/bcc/issues/4583

3. bpftrace<br>
install bcc from source <br>
https://github.com/bpftrace/bpftrace/issues/1855 <br>
https://arkivm.github.io/bpf/2021/05/31/compiling-bcc-bpftrace/<br><br>

install libbpf from source<br>
```
git clone https://github.com/libbpf/libbpf
cd src
make
sudo make install
```
I am getting segfaults on every bpftrace command, i tried solving by searching for similar issues: #664 #853, no change.
I had troubles in the apt's package manager's bpftrace, so then i compiled from source also.
 
```
➜  ~ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.04.4 LTS
Release:	22.04
Codename:	jammy
➜  ~ uname -r
6.5.0-45-generic
```
```
➜  ~ bpftrace --v
bpftrace v0.21.0-86-geb73

```
This is the dmesg output of the segfault
```
[ 2478.902182] bpftrace[12230]: segfault at 6ffffffe0 ip 00007f79ff59d934 sp 00007ffc0ff2a508 error 4 in libc.so.6[7f79ff428000+195000] likely on CPU 11 (core 5, socket 0)
[ 2478.902190] Code: 00 00 00 0f 1f 00 f3 0f bc c0 48 83 ef 7f 48 01 f8 c5 f8 77 c3 90 f3 0f bc c0 48 83 ef 5f 48 01 f8 c5 f8 77 c3 90 48 83 cf 1f <c5> fd 74 4f e1 c5 fd d7 c1 c4 e2 6a f7 c0 85 c0 0f 84 1a ff ff ff

```
Working with appimages(release pages) has done it!


4. Tracing kernel drivers using gdb
   <br>URL: https://www.youtube.com/watch?v=aAuw2EVCBBg   

5. bpftool <br>
Building perf from `tools/perf` using the medium article below removes the bpftools's prompt to install linux-tools
<br>https://medium.com/@manas.marwah/building-perf-tool-fc838f084f71<br>



6. vtune debug symbols<br>
   use vtune docs. good!<br>
   https://askubuntu.com/questions/487222/how-to-install-debug-symbols-for-installed-packages/1434174#1434174<br>

sudo apt update
sudo apt install build-essential libncurses-dev bison flex libssl-dev libelf-dev
sudo apt install libqt5gui5 libqt5widgets5 libqt5script5 libqt5xml5 libqt5x11extras5 libqt5dbus5 qtbase5-dev libqt5svg5-dev libncurses5-dev
sudo apt install libcap-dev libacl1-dev libcrypt-dev libselinux1-dev libseccomp-dev systemd-coredump hwloc


changing configs

1. kernel config

https://www.reddit.com/r/linuxquestions/comments/1dvp62g/comment/lf7cqtn/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button

https://stackoverflow.com/questions/67670169/compiling-kernel-gives-error-no-rule-to-make-target-debian-certs-debian-uefi-ce/72528175#72528175

https://unix.stackexchange.com/questions/717619/linux-mint-5-15-0-47-generic-error-out-of-memory-when-booting

cp /boot / config-<kversion> .config
make olddefconfig
sudo make -j  $(nproc)
sudo make modules_install -j $(nproc)
sudo make install -j $(nproc)
sudo update-initramfs -c -k <kernelversion>
sudo update-grub




2. ebpf configs
https://maofeichen.com/setup-the-extended-berkeley-packet-filter-ebpf-environment/

https://github.com/iovisor/bcc/issues/4583
bcc installation error modules not found 



we can use `gcore pid` to generate coredump of running processes!
Got 800m coredumps

how to generate coredumps
echo "/tmp/%e.%p" | sudo tee /proc/sys/kernel/core_pattern 
ulimit -c unlimited 
this updates RLIMIT_CORE which is a soft resource limit!
now in tmp we can find the coredumps

to use the coredumps we can do gdb <binaryfile> <coredumpfile>

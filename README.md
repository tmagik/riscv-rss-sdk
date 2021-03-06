![logo](./conf/logo.png)

# RISC-V Spike Simulator SDK

In the recent version of the riscv-tools and freedom-u-sdk, both of them removed the support of the spike simulator, and tutorials about running Linux on spike is using static compiled busybox, Spike is the simplest simulator of RISC-V, it has a very clear description of the instructions, you can apply your ideas and check it for a quick try. The **R**ISC-V **S**pike **S**imulator SDK wants to help people to test design on Spike with Linux environment easily, the basic framework is based on Freedom U SDK version 1.0.

This SDK follows the newest Linux Kernel, GNU toolchain and Spike, the functions of each folder in the project are as follows:

|       Folder        |      Description       |    Version    |
| :-----------------: | :--------------------: | :-----------: |
|      buildroot      |    Build initramfs     |  2020.02-git  |
|        linux        |      Linux Kernel      |     5.4.8     |
| riscv-gnu-toolchain | GNU Compiler Toolchain |   gcc 9.2.0   |
|     riscv-tools     | Simulator & Bootloader | master branch |
|        conf         |     config for SDK     |               |

## Quickstart
Build dependencies on Ubuntu 16.04/18.04:
```bash
$ sudo apt-get install device-tree-compiler autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev
```
```bash
git submodule update --init --recursive
# NOTICE: DO NOT SET $RISCV and MAKE SURE NO ORIGIN RISCV TOOLCHAIN IN YOUR $PATH
make
```

## Software Development Flow
1. build rootfs   
Using `buildroot` to help you to build a rootfs, you can add config in `conf/buildroot_initramfs_config` to add package, and You can copy your pre-compiled elf in `work/initramfs_sysroot` to access in Spike.
And remember, the basic idea of this SDK is to deconfig all the device, because spike is very limited support for devices, specically the Ethernet.
2. compile Linux  
There're some config you need to notice:
	```
	CONFIG_HVC_RISCV_SBI=y
	CONFIG_VT_CONSOLE=n
	```
3. Spike
The default account and password is root@phantom, following is the terminal log on Linux 5.4.7.
```
bbl loader

          RISC-V Spike Simulator SDK
                    
              vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv
                  vvvvvvvvvvvvvvvvvvvvvvvvvvvv
rrrrrrrrrrrrr       vvvvvvvvvvvvvvvvvvvvvvvvvv
rrrrrrrrrrrrrrrr      vvvvvvvvvvvvvvvvvvvvvvvv
rrrrrrrrrrrrrrrrrr    vvvvvvvvvvvvvvvvvvvvvvvv
rrrr Linux rrrrrrr    vvvvvvvvvvvvvvvvvvvvvvvv
rrrrrrrrrrrrrrrrrr    vvvvvvvvvvvvvvvvvvvvvvvv
rrrrrrrrrrrrrrrr      vvvvvvvvvvvvvvvvvvvvvv  
rrrrrrrrrrrrr       vvvvvvvvvvvvvvvvvvvvvv    
rr                vvvvvvvvvvvvvvvvvvvvvv      
rr            vvvvvvvvvvvvvvvvvvvvvvvv      rr
rrrr      vvvvvvvvvvvvvvvvvvvvvvvvvv      rrrr
rrrrrr      vvvvvvvvvvvvvvvvvvvvvv      rrrrrr
rrrrrrrr      vvvvvvvvvvvvvvvvvv      rrrrrrrr
rrrrrrrrrr      vvvvvvvvvvvvvv      rrrrrrrrrr
rrrrrrrrrrrr      vvvvvvvvvv      rrrrrrrrrrrr
rrrrrrrrrrrrrr      vvvvvv      rrrrrrrrrrrrrr
rrrrrrrrrrrrrrrr      vv      rrrrrrrrrrrrrrrr
rrrrrrrrrrrrrrrrrr          rrrrrrrrrrrrrrrrrr
rrrrrrrrrrrrrrrrrrrr      rrrrrrrrrrrrrrrrrrrr
rrrrrrrrrrrrrrrrrrrrrr  rrrrrrrrrrrrrrrrrrrrrr        
                                                         
         
  This SDK is based on SiFive Freedom U SDK.
					
				 Phantom0308
			 ICSR, ZheJiang University

[    0.000000] OF: fdt: No chosen node found, continuing without
[    0.000000] OF: fdt: Ignoring memory range 0x80000000 - 0x80200000
[    0.000000] Linux version 5.4.8 (phantom0308@Pavilion) (gcc version 9.2.0 (GCC)) #1 SMP Sun Jan 5 21:59:34 CST 2020
[    0.000000] initrd not found or empty - disabling initrd
[    0.000000] Zone ranges:
[    0.000000]   DMA32    [mem 0x0000000080200000-0x00000000ffffffff]
[    0.000000]   Normal   empty
[    0.000000] Movable zone start for each node
[    0.000000] Early memory node ranges
[    0.000000]   node   0: [mem 0x0000000080200000-0x00000000ffffffff]
[    0.000000] Initmem setup node 0 [mem 0x0000000080200000-0x00000000ffffffff]
[    0.000000] software IO TLB: mapped [mem 0xfa3fb000-0xfe3fb000] (64MB)
[    0.000000] elf_hwcap is 0x112d
[    0.000000] percpu: Embedded 13 pages/cpu s24216 r0 d29032 u53248
[    0.000000] Built 1 zonelists, mobility grouping on.  Total pages: 516615
[    0.000000] Kernel command line: 
[    0.000000] Dentry cache hash table entries: 262144 (order: 9, 2097152 bytes, linear)
[    0.000000] Inode-cache hash table entries: 131072 (order: 8, 1048576 bytes, linear)
[    0.000000] Sorting __ex_table...
[    0.000000] mem auto-init: stack:off, heap alloc:off, heap free:off
[    0.000000] Memory: 1987072K/2095104K available (3329K kernel code, 234K rwdata, 866K rodata, 5439K init, 247K bss, 108032K reserved, 0K cma-reserved)
[    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=4, Nodes=1
[    0.000000] rcu: Hierarchical RCU implementation.
[    0.000000] rcu: 	RCU event tracing is enabled.
[    0.000000] rcu: 	RCU restricting CPUs from NR_CPUS=8 to nr_cpu_ids=4.
[    0.000000] rcu: RCU calculated value of scheduler-enlistment delay is 10 jiffies.
[    0.000000] rcu: Adjusting geometry for rcu_fanout_leaf=16, nr_cpu_ids=4
[    0.000000] NR_IRQS: 0, nr_irqs: 0, preallocated irqs: 0
[    0.000000] riscv_timer_init_dt: Registering clocksource cpuid [0] hartid [0]
[    0.000000] clocksource: riscv_clocksource: mask: 0xffffffffffffffff max_cycles: 0x24e6a1710, max_idle_ns: 440795202120 ns
[    0.000005] sched_clock: 64 bits at 10MHz, resolution 100ns, wraps every 4398046511100ns
[    0.011145] printk: console [hvc0] enabled
[    0.011395] Calibrating delay loop (skipped), value calculated using timer frequency.. 20.00 BogoMIPS (lpj=100000)
[    0.012000] pid_max: default: 32768 minimum: 301
[    0.012350] Mount-cache hash table entries: 4096 (order: 3, 32768 bytes, linear)
[    0.012785] Mountpoint-cache hash table entries: 4096 (order: 3, 32768 bytes, linear)
[    0.013980] rcu: Hierarchical SRCU implementation.
[    0.014490] smp: Bringing up secondary CPUs ...
[    0.016070] smp: Brought up 1 node, 4 CPUs
[    0.016615] devtmpfs: initialized
[    0.017270] random: get_random_bytes called from setup_net+0x38/0x186 with crng_init=0
[    0.017765] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 19112604462750000 ns
[    0.018335] futex hash table entries: 1024 (order: 4, 65536 bytes, linear)
[    0.018935] NET: Registered protocol family 16
[    0.021965] vgaarb: loaded
[    0.022255] SCSI subsystem initialized
[    0.022690] usbcore: registered new interface driver usbfs
[    0.023025] usbcore: registered new interface driver hub
[    0.023415] usbcore: registered new device driver usb
[    0.023740] pps_core: LinuxPPS API ver. 1 registered
[    0.024030] pps_core: Software ver. 5.3.6 - Copyright 2005-2007 Rodolfo Giometti <giometti@linux.it>
[    0.024565] PTP clock support registered
[    0.025150] clocksource: Switched to clocksource riscv_clocksource
[    0.026370] NET: Registered protocol family 2
[    0.026905] tcp_listen_portaddr_hash hash table entries: 1024 (order: 2, 16384 bytes, linear)
[    0.027415] TCP established hash table entries: 16384 (order: 5, 131072 bytes, linear)
[    0.028060] TCP bind hash table entries: 16384 (order: 6, 262144 bytes, linear)
[    0.028680] TCP: Hash tables configured (established 16384 bind 16384)
[    0.029095] UDP hash table entries: 1024 (order: 3, 32768 bytes, linear)
[    0.029515] UDP-Lite hash table entries: 1024 (order: 3, 32768 bytes, linear)
[    0.030005] NET: Registered protocol family 1
[    0.030315] PCI: CLS 0 bytes, default 64
[    0.045100] workingset: timestamp_bits=62 max_order=19 bucket_order=0
[    0.049650] io scheduler mq-deadline registered
[    0.049915] io scheduler kyber registered
[    0.090900] Serial: 8250/16550 driver, 4 ports, IRQ sharing disabled
[    0.092110] libphy: Fixed MDIO Bus: probed
[    0.092465] ehci_hcd: USB 2.0 'Enhanced' Host Controller (EHCI) Driver
[    0.092845] ehci-pci: EHCI PCI platform driver
[    0.093145] usbcore: registered new interface driver usb-storage
[    0.093635] usbcore: registered new interface driver usbhid
[    0.093960] usbhid: USB HID core driver
[    0.094310] NET: Registered protocol family 17
[    0.096070] Freeing unused kernel memory: 5436K
[    0.096335] This architecture does not have kernel memory protection.
[    0.096710] Run /init as init process
Starting syslogd: OK
Starting klogd: OK
Running sysctl: OK
Starting mdev... OK
modprobe: can't change directory to '/lib/modules': No such file or directory
Saving random seed: [    0.382350] random: dd: uninitialized urandom read (512 bytes read)
OK
Starting network: OK

Welcome to Buildroot
buildroot login: root
root
Password: phantom

# cd /
cd /
# ls
ls
bin      init     linuxrc  opt      run      tmp
dev      lib      media    proc     sbin     usr
etc      lib64    mnt      root     sys      var
# poweroff
poweroff
# Stopping network: OK
Saving random seed: [  571.142295] random: dd: uninitialized urandom read (512 bytes read)
OK
Stopping mdev... stopped process in pidfile '/var/run/mdev.pid' (pid 67)
OK
Stopping klogd: OK
Stopping syslogd: OK
umount: can't unmount /: Invalid argument
The system is going down NOW!
Sent SIGTERM to all processes
Sent SIGKILL to all processes
Requesting system poweroff
[  573.195315] reboot: Power down
```


Finally, if you have any suggestions for this SDK, please *push* it !
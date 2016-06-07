# OpenBSD-APU2
This repo contains the necessary configs to create a WIFI router out of [PC Engine's APU2](http://pcengines.ch/apu2c4.htm) running OpenBSD >=5.9.

The APU2 is a fanless board with 4x 1Ghz CPUs and 4GB of RAM (AMD GX-412TC SOC, amd64 intruction set) - quite capable. Well, actually it's totally overkill for a router but anyway, it's still cheaper then the alternatives. 

## Why? 

Well frankly, we were tired of unreliable, inpotent routers with unknown (crappy) security posture. Our objective was to setup this thing once and forget about it - not a techy-powertrip.

## Instructions
- The APU2 is setup as such and cost 245$can:
  - board: [apu2c4](http://pcengines.ch/apu2c4.htm) - 4x 1Ghz, 4 GB RAM, 3 1000baseT, 2 USB, 1 SATA, 2 mPCI, etc
  - wifi: [wle200nx](http://pcengines.ch/wle200nx.htm) - A B G N*, 2 antenna
  - hd: [msata16d](http://pcengines.ch/msata16d.htm)
- Follow [Elad's instructions](https://github.com/elad/openbsd-apu2) to install OpenBSD on the APU2.
- The config files are pretty much self explanatory. Really, if you don't know what it does... RTFM or it's simply not for you. OpenBSD's doc is quite simple and complete.

```
$ sysctl hw.sensors.km0.temp0
hw.sensors.km0.temp0=65.38 degC
$ top -nC 
load averages:  1.02,  0.97,  0.95    barricade.mantor.org 21:53:22
29 processes: 28 idle, 1 on processor  up 14:01
CPU0 states:  0.1% user,  0.0% nice,  0.0% system,  0.2% interrupt, 99.7% idle
CPU1 states:  0.0% user,  0.0% nice,  0.0% system,  0.1% interrupt, 99.9% idle
CPU2 states:  0.0% user,  0.0% nice,  0.0% system,  0.1% interrupt, 99.9% idle
CPU3 states:  0.0% user,  0.0% nice,  0.0% system,  0.0% interrupt,  100% idle
Memory: Real: 32M/206M act/tot Free: 3739M Cache: 116M Swap: 0K/890M

  PID USERNAME PRI NICE  SIZE   RES STATE     WAIT      TIME    CPU COMMAND
25775 _unbound   2    0   15M   18M idle      kqread    0:03  0.00% unbound -c /var/unbound/etc/unbound.conf
27903 _ntp       2  -20 1324K 1680K sleep     poll      0:02  0.00% ntpd: ntp engine
    1 root      10    0  460K  556K idle      wait      0:01  0.00% /sbin/init
16962 _pflogd    4    0  684K  424K sleep     bpf       0:01  0.00% pflogd: [running] -s 160 -i pflog0 -f /var/log/pflog
23489 _dhcp      2    0  784K  676K sleep     poll      0:00  0.00% dhclient: em0
25966 root       2    0  908K 1452K idle      select    0:00  0.00% /usr/sbin/sshd
11277 root       2    0  772K 1152K idle      poll      0:00  0.00% /usr/sbin/cron
20998 _syslogd   2    0 1064K 1428K sleep     kqread    0:00  0.00% /usr/sbin/syslogd
12137 root       2  -20  836K 1684K idle      poll      0:00  0.00% /usr/sbin/ntpd
26146 root       2    0 1204K 2316K idle      kqread    0:00  0.00% /usr/sbin/httpd
18485 _smtpq     2    0 1568K 2164K idle      kqread    0:00  0.00% smtpd: queue
29389 root       2    0 1064K 1324K idle      netio     0:00  0.00% syslogd: [priv]
29919 _dhcp      2    0  720K 1412K idle      poll      0:00  0.00% /usr/sbin/dhcpd em1 em2 athn0
12300 root       2    0 1632K 2120K idle      kqread    0:00  0.00% /usr/sbin/smtpd
$ dmesg
OpenBSD 5.9 (GENERIC.MP) #1888: Fri Feb 26 01:20:19 MST 2016
    deraadt@amd64.openbsd.org:/usr/src/sys/arch/amd64/compile/GENERIC.MP
real mem = 4261076992 (4063MB)
avail mem = 4127739904 (3936MB)
mpath0 at root
scsibus0 at mpath0: 256 targets
mainbus0 at root
bios0 at mainbus0: SMBIOS rev. 2.7 @ 0xdffb7020 (7 entries)
bios0: vendor coreboot version "88a4f96" date 03/11/2016
bios0: PC Engines apu2
acpi0 at bios0: rev 2
acpi0: sleep states S0 S1 S2 S3 S4 S5
acpi0: tables DSDT FACP SSDT APIC HEST SSDT SSDT HPET
acpi0: wakeup devices PWRB(S4) PBR4(S4) PBR5(S4) PBR6(S4) PBR7(S4) PBR8(S4) UOH1(S3) UOH3(S3) UOH5(S3) XHC0(S4)
acpitimer0 at acpi0: 3579545 Hz, 32 bits
acpimadt0 at acpi0 addr 0xfee00000: PC-AT compat
cpu0 at mainbus0: apid 0 (boot processor)
cpu0: AMD GX-412TC SOC, 998.30 MHz
cpu0: FPU,VME,DE,PSE,TSC,MSR,PAE,MCE,CX8,APIC,SEP,MTRR,PGE,MCA,CMOV,PAT,PSE36,CFLUSH,MMX,FXSR,SSE,SSE2,HTT,SSE3,
  PCLMUL,MWAIT,SSSE3,CX16,SSE4.1,SSE4.2,MOVBE,POPCNT,AES,XSAVE,AVX,F16C,NXE,MMXX,FFXSR,PAGE1GB,LONG,LAHF,CMPLEG,
  SVM,EAPICSP,AMCR8,ABM,SSE4A,MASSE,3DNOWP,OSVW,IBS,SKINIT,TOPEXT,ITSC,BMI1
cpu0: 32KB 64b/line 2-way I-cache, 32KB 64b/line 8-way D-cache, 2MB 64b/line 16-way L2 cache
cpu0: ITLB 32 4KB entries fully associative, 8 4MB entries fully associative
cpu0: DTLB 40 4KB entries fully associative, 8 4MB entries fully associative
cpu0: smt 0, core 0, package 0
mtrr: Pentium Pro MTRR support, 8 var ranges, 88 fixed ranges
cpu0: apic clock running at 99MHz
cpu0: mwait min=64, max=64, IBE
<snip cpu1-3...>
```

## Performance
ubench is not the most exhaustive benchmark for a router but it gives an idea of where the APU2 stands:
```
Ubench CPU:   288530
Ubench MEM:    37347
--------------------
Ubench AVG:   162938
```

## Caveats
- 802.11n is not supported on OpenBSD
- still testing the stability - so far so good

## License
BSD

## Authors
- Danny Fullerton - Mantor Organization
- Jean-Francois Rioux - Mantor Organization

# OpenBSD-APU2
This repo contains the necessary configs to create a WIFI router out of [PC Engine's APU2](http://pcengines.ch/apu2c4.htm) running OpenBSD >=5.9.

The APU2 is a fanless board with 4x 1Ghz CPUs and 4GB of RAM (AMD GX-412TC SOC, amd64 intruction set) - quite capable. Well, actually it's totally overkill for a router but anyway, it's still cheaper then the alternatives. Oh and it use an open source firmware: [coreboot](https://www.coreboot.org).
![front panel open top](https://raw.githubusercontent.com/northox/openbsd-apu2/master/front-open.jpeg)

It's stable. We've been using it for a few months now and had very few hiccups. The biggest limitation is the support for 802.11ac and poor performance of 802.11n.

## Why? 
Well frankly, we were tired of unreliable, inpotent routers with unknown ([crappy](https://www.helpnetsecurity.com/2019/09/17/vulnerabilities-iot-devices/)) security posture. Our objective was to setup this thing once and forget about it - not a techy-powertrip.

## Instructions
- The APU2 is setup as such and cost 245$cad:
  - board: [apu2c4](http://pcengines.ch/apu2c4.htm) - 4x 1Ghz, 4 GB RAM, 3 1000baseT, 2 USB3, 1 SATA, 2 mPCI, etc
  - wifi: [wle200nx](http://pcengines.ch/wle200nx.htm) - A B G N, 2 antenna
  - hd: [msata16d](http://pcengines.ch/msata16d.htm)
- Follow [Elad's instructions](https://github.com/elad/openbsd-apu2) to install OpenBSD on the APU2.
- The config files are pretty much self explanatory. Really, if you don't know what it does... RTFM or it's simply not for you. OpenBSD's doc is quite simple and complete.
- Execute this for a status web page: `echo '*/5 * * * * /usr/local/sbin/update_webpage' >> /var/cron/tabs/root`

## Status (update_webpage)
```
$ uname -a
OpenBSD barricade.mantor.org 5.9 GENERIC.MP#1888 amd64
$ sysctl hw.sensors.km0.temp0
hw.sensors.km0.temp0=62.75 degC
$ top -nC 666
load averages:  0.62,  0.52,  0.56    barricade.mantor.org 13:15:01
33 processes: 32 idle, 1 on processor  up 12 days, 22:27
CPU0 states:  0.2% user,  0.0% nice,  0.1% system,  0.2% interrupt, 99.4% idle
CPU1 states:  0.0% user,  0.0% nice,  0.1% system,  0.1% interrupt, 99.8% idle
CPU2 states:  0.0% user,  0.0% nice,  0.1% system,  0.1% interrupt, 99.8% idle
CPU3 states:  0.0% user,  0.0% nice,  0.0% system,  0.0% interrupt, 99.9% idle
Memory: Real: 48M/247M act/tot Free: 3698M Cache: 123M Swap: 0K/890M

  PID USERNAME PRI NICE  SIZE   RES STATE     WAIT      TIME    CPU COMMAND
14875 _unbound   2    0   27M   32M sleep     kqread    1:03  0.00% unbound -c /var/unbound/etc/unbound.conf
31702 _ntp       2  -20 1300K 1656K sleep     poll      0:50  0.00% ntpd: ntp engine
30018 _pflogd    4    0  688K  508K sleep     bpf       0:25  0.00% pflogd: [running] -s 160 -i pflog0 -f /var/log/pflog
26667 root       2    0  916K 1440K idle      select    0:11  0.00% /usr/sbin/sshd
 4198 _syslogd   2    0 1060K 1444K sleep     kqread    0:10  0.00% /usr/sbin/syslogd
27374 root       2    0  760K 1104K sleep     poll      0:09  0.00% /usr/sbin/cron
 2301 _dhcp      2    0  768K  680K idle      poll      0:03  0.00% dhclient: em0
    1 root      10    0  464K  564K idle      wait      0:02  0.00% /sbin/init
 2454 _dhcp      2    0  708K 1400K idle      poll      0:01  0.00% /usr/sbin/dhcpd athn0
15368 root       2    0 3552K 3292K idle      poll      0:01  0.00% sshd: northox [priv]
30674 _smtpd     2    0 1676K 2784K idle      kqread    0:00  0.00% smtpd: pony express
  497 root       2    0 1576K 2180K idle      kqread    0:00  0.00% /usr/sbin/smtpd
 2020 _smtpq     2    0 1676K 2380K idle      kqread    0:00  0.00% smtpd: queue
30436 _smtpd     2    0 1540K 2364K idle      kqread    0:00  0.00% smtpd: lookup
 2118 _smtpd     2    0 1648K 2324K idle      kqread    0:00  0.00% smtpd: control
16322 root       2    0 1060K 1268K idle      netio     0:00  0.00% syslogd: [priv]
15619 root       2    0  624K  608K idle      netio     0:00  0.00% pflogd: [priv]
16011 _smtpd     2    0 1308K 2084K idle      kqread    0:00  0.00% smtpd: scheduler
24867 root       2  -20  804K 1664K idle      poll      0:00  0.00% /usr/sbin/ntpd
 1856 root       2    0  648K  548K idle      poll      0:00  0.00% dhclient: em0 [priv]
  252 root       2    0  864K 1876K idle      kqread    0:00  0.00% /usr/sbin/httpd
 1211 _smtpd     2    0 1376K 1988K idle      kqread    0:00  0.00% smtpd: klondike
31513 www        2    0  884K 1884K idle      kqread    0:00  0.00% httpd: server
11225 www        2    0  744K 1616K idle      kqread    0:00  0.00% httpd: logger
18802 _ntp       2    0  688K 1424K idle      poll      0:00  0.00% ntpd: dns engine
 7354 root       3    0  336K 1120K idle      ttyin     0:00  0.00% /usr/libexec/getty std.115200 tty00
$ list-dhcpd-leases
192.168.1.16  55:f9:35:43:32:11  "iPad"
192.168.1.17  78:44:87:13:56:6e  "android-35375e65e664b13d"
192.168.1.21  74:86:23:1d:a6:61  "laserbeak"
$ ^D
```

## dmesg
```
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
```
Ubench CPU:   288530
Ubench MEM:    37347
--------------------
Ubench AVG:   162938
```

```
$ openssl speed md5 sha1 sha256 sha512 des des-ede3 aes-128-cbc aes-192-cbc aes-256-cbc rsa2048 dsa2048
LibreSSL 2.3.2
built on: date not available
options:bn(64,64) rc4(8x,int) des(idx,cisc,16,int) aes(partial) idea(int) blowfish(idx) 
compiler: information not available
The 'numbers' are in 1000s of bytes per second processed.
type             16 bytes     64 bytes    256 bytes   1024 bytes   8192 bytes
md5               4565.65k    16729.20k    49299.48k    96037.93k   132775.72k
sha1              4481.83k    15422.45k    40502.69k    68271.20k    86173.85k
des cbc          11858.55k    12263.08k    12356.64k    12401.29k    12530.22k
des ede3          4589.16k     4710.53k     4785.33k     4759.05k     4765.51k
aes-128 cbc      14778.42k    15650.49k    16148.75k    44138.14k    44958.02k
aes-192 cbc      12427.81k    13167.48k    13322.04k    37420.23k    38020.68k
aes-256 cbc      10770.22k    11232.15k    11495.93k    32585.99k    32885.03k
sha256            5399.91k    12679.80k    22259.16k    27965.75k    30307.68k
sha512            4807.39k    19008.23k    29792.79k    41931.95k    47916.40k
                  sign    verify    sign/s verify/s
rsa 2048 bits 0.009785s 0.000316s    102.2   3168.6
                  sign    verify    sign/s verify/s
dsa 2048 bits 0.003073s 0.003696s    325.4    270.5
```

## Caveats (6.1)
- athn limited 802.11n performance
- 802.11ac is not supported

## License
BSD

## Authors
- Danny Fullerton - Mantor Organization
- Jean-Francois Rioux - Mantor Organization

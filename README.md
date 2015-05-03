
VolDiff: Malware Memory Footprint Analysis
==========================================

VolDiff is a bash script that runs Volatility plugins against memory images captured before and after malware execution. It creates a report that highlights system changes based on memory (RAM) analysis.

VolDiff is a simple yet powerfull malware analysis tool that enables malware analysts to quickly identify IOCs and understand advanced malware behaviour.

Use Directions
----------------

1. Capture a memory dump of a clean Windows system and save it as "baseline.vmem". This image will serve as a baseline for the analysis.

2. Execute your malware sample on the same system, then take a second memory dump and save it as "infected.vmem".

3. Run VolDiff:
<pre>
./VolDiff.sh path/to/baseline.vmem path/to/infected.vmem profile
</pre>
"profile" should be "Win7SP0x86" or "Win7SP1x64" etc.

VolDiff will save the output of a selection of Volatility plugins for both memory images (baseline and infected), then it will create a report to highlight notable changes (new processes, network connections, injected code, drivers etc).


Sample Output
---------------
<pre>
 _    __      ______  _ ________
| |  / /___  / / __ \(_) __/ __/
| | / / __ \/ / / / / / /_/ /_  
| |/ / /_/ / / /_/ / / __/ __/  
|___/\____/_/_____/_/_/ /_/     

Volatility analysis report generated by VolDiff v0.9.4.
Download the latest version from https://github.com/aim4r/VolDiff/.

Suspicious new netscan entries
=========================================================================

Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created
0x3da3f618         TCPv4    172.16.108.128:139             0.0.0.0:0            LISTENING        4        System         
0x3dad8008         TCPv4    172.16.108.128:49167           62.24.131.168:80     CLOSED           924      svchost.exe    
0x3fc7b630         TCPv4    172.16.108.128:49164           65.55.50.157:443     CLOSED           924      svchost.exe    
0x3fc8b5f0         TCPv4    172.16.108.128:49165           62.24.131.168:80     CLOSED           924      svchost.exe    
0x3fdf2348         TCPv4    172.16.108.128:49168           87.236.215.151:80    CLOSED           2108     explorer.exe   


Suspicious new pslist entries
=========================================================================

Offset(V)  Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit                          
---------- -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------
0x8508ed40 malwr.exe              2908   4020      4       64      1      0 2015-05-02 19:32:45 UTC+0000                                 
0x872ee030 taskeng.exe            2080    916      6       85      1      0 2015-05-02 19:32:56 UTC+0000                                 
0x8507f030 kmaxqsj.exe            2300   4044     10      243      1      0 2015-05-02 19:33:07 UTC+0000 


Suspicious new psscan entries
=========================================================================

Offset(P)          Name                PID   PPID PDB        Time created                   Time exited                   
------------------ ---------------- ------ ------ ---------- ------------------------------ ------------------------------
0x000000003daee030 taskeng.exe        2080    916 0x3ebed4c0 2015-05-02 19:32:56 UTC+0000                                 
0x000000003fc6e780 cmd.exe            2484   1540 0x3ebed340 2015-05-02 19:33:48 UTC+0000   2015-05-02 19:33:48 UTC+0000  
0x000000003fc7f030 kmaxqsj.exe        2300   4044 0x3ebed520 2015-05-02 19:33:07 UTC+0000                                 
0x000000003fc8ed40 malwr.exe          2908   4020 0x3ebed540 2015-05-02 19:32:45 UTC+0000                                 
0x000000003fda2518 conhost.exe        2876    376 0x3ebed560 2015-05-02 19:33:48 UTC+0000   2015-05-02 19:33:48 UTC+0000  


Suspicious new malfind entries
=========================================================================

Process: kmaxqsj.exe Pid: 2300 Address: 0x400000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 165, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x00400000  4d 5a 90 00 03 00 00 00 04 00 00 00 ff ff 00 00   MZ..............
0x00400010  b8 00 00 00 00 00 00 00 40 00 00 00 00 00 00 00   ........@.......
0x00400020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x00400030  00 00 00 00 00 00 00 00 00 00 00 00 80 00 00 00   ................

0x400000 4d               DEC EBP
0x400001 5a               POP EDX
0x400002 90               NOP
0x400003 0003             ADD [EBX], AL
0x400005 0000             ADD [EAX], AL
0x400007 000400           ADD [EAX+EAX], AL
0x40000a 0000             ADD [EAX], AL
0x40000c ff               DB 0xff
0x40000d ff00             INC DWORD [EAX]
0x40000f 00b800000000     ADD [EAX+0x0], BH
0x400015 0000             ADD [EAX], AL
0x400017 004000           ADD [EAX+0x0], AL
0x40001a 0000             ADD [EAX], AL
0x40001c 0000             ADD [EAX], AL
0x40001e 0000             ADD [EAX], AL
0x400020 0000             ADD [EAX], AL
0x400022 0000             ADD [EAX], AL
0x400024 0000             ADD [EAX], AL
0x400026 0000             ADD [EAX], AL
0x400028 0000             ADD [EAX], AL
0x40002a 0000             ADD [EAX], AL
0x40002c 0000             ADD [EAX], AL
0x40002e 0000             ADD [EAX], AL
0x400030 0000             ADD [EAX], AL
0x400032 0000             ADD [EAX], AL
0x400034 0000             ADD [EAX], AL
0x400036 0000             ADD [EAX], AL
0x400038 0000             ADD [EAX], AL
0x40003a 0000             ADD [EAX], AL
0x40003c 800000           ADD BYTE [EAX], 0x0
0x40003f 00               DB 0x0


Suspicious new timeliner entries
=========================================================================

1970-01-01 00:00:00 UTC+0000|[PE HEADER (exe)]| kmaxqsj.exe| Process: kmaxqsj.exe/PID: 2300/PPID: 4044/Process POffset: 0x3fc7f030/DLL Base: 0x00400000
1970-01-01 00:00:00 UTC+0000|[PE HEADER (exe)]| malwr.exe| Process: malwr.exe/PID: 2908/PPID: 4020/Process POffset: 0x3fc8ed40/DLL Base: 0x00400000
2015-04-18 23:40:59 UTC+0000|[PROCESS]| svchost.exe| PID: 640/PPID: 516/POffset: 0x3dd6bb70
2015-04-18 23:41:00 UTC+0000|[PROCESS]| svchost.exe| PID: 1228/PPID: 516/POffset: 0x3da31968
2015-05-02 19:32:00 UTC+0000|[NETWORK CONNECTION]| 172.16.108.128:1900 -> *:*| 3084/UDPv4//0x3db82140
2015-05-02 19:32:00 UTC+0000|[NETWORK CONNECTION]| 172.16.108.128:58385 -> *:*| 3084/UDPv4//0x3d8473f0
2015-05-02 19:32:00 UTC+0000|[NETWORK CONNECTION]| fe80::4847:83c0:c352:feac:1900 -> *:*| 3084/UDPv6//0x3fd62f50
2015-05-02 19:32:00 UTC+0000|[NETWORK CONNECTION]| fe80::4847:83c0:c352:feac:58383 -> *:*| 3084/UDPv6//0x3db2ba68
2015-05-02 19:32:45 UTC+0000|[PROCESS]| malwr.exe| PID: 2908/PPID: 4020/POffset: 0x3fc8ed40
2015-05-02 19:32:56 UTC+0000|[PROCESS]| taskeng.exe| PID: 2080/PPID: 916/POffset: 0x3daee030
2015-05-02 19:33:07 UTC+0000|[PROCESS]| kmaxqsj.exe| PID: 2300/PPID: 4044/POffset: 0x3fc7f030
2015-05-02 19:33:39 UTC+0000|[PROCESS]| SearchFilterHo| PID: 2168/PPID: 2608/POffset: 0x3fa12030
2015-05-02 19:33:39 UTC+0000|[PROCESS]| SearchProtocol| PID: 3852/PPID: 2608/POffset: 0x3fa10b68
2015-05-02 19:33:48 UTC+0000|[NETWORK CONNECTION]| 0.0.0.0:0 -> *:*| 1228/UDPv4//0x3db4c1a0
2015-05-02 19:33:48 UTC+0000|[NETWORK CONNECTION]| 0.0.0.0:5355 -> *:*| 1228/UDPv4//0x3fccf320
2015-05-02 19:33:48 UTC+0000|[NETWORK CONNECTION]| 0.0.0.0:5355 -> *:*| 1228/UDPv4//0x3fccf6b0
2015-05-02 19:33:48 UTC+0000|[NETWORK CONNECTION]| :::0 -> *:*| 1228/UDPv6//0x3db4c1a0
2015-05-02 19:33:48 UTC+0000|[NETWORK CONNECTION]| 172.16.108.128:68 -> *:*| 756/UDPv4//0x3fdcacd8
2015-05-02 19:33:48 UTC+0000|[NETWORK CONNECTION]| :::5355 -> *:*| 1228/UDPv6//0x3fccf320
2015-05-02 19:33:48 UTC+0000|[PROCESS]| cmd.exe| PID: 2484/PPID: 1540/POffset: 0x3fc6e780 End: 2015-05-02 19:33:48 UTC+0000
2015-05-02 19:33:48 UTC+0000|[PROCESS]| conhost.exe| PID: 2876/PPID: 376/POffset: 0x3fda2518 End: 2015-05-02 19:33:48 UTC+0000


Suspicious new svcscan entries
=========================================================================

Process ID: 876
Service State: SERVICE_RUNNING
Binary Path: C:\Windows\System32\svchost.exe -k LocalSystemNetworkRestricted
Process ID: 924
Service State: SERVICE_RUNNING
Binary Path: C:\Windows\system32\svchost.exe -k netsvcs


Suspicious new cmdline entries
=========================================================================

malwr.exe pid:   2908
Command line : "Z:\Malware\Malware Samples\malwr.exe"
************************************************************************
taskeng.exe pid:   2080
Command line : taskeng.exe {4240277E-D433-456A-B44E-4BD07C4DB325}
************************************************************************
kmaxqsj.exe pid:   2300
Command line : "C:\Users\victim\AppData\Local\Temp\kmaxqsj.exe"
************************************************************************


Suspicious new mutantscan entries
=========================================================================

Offset(P)              #Ptr     #Hnd Signal Thread           CID Name
------------------ -------- -------- ------ ---------- --------- ----
0x000000003da022f8        3        2      1 0x00000000           HGFSMUTEX
0x000000003da3e120        2        1      1 0x00000000           5827a689a8a470200835d840817112f0
0x000000003daaab90        2        1      1 0x00000000           WininetProxyRegistryMutex
0x000000003df68d60        2        1      0 0x855d8248 2108:1140 41df362a3f3d701bb5b5749a3e43f484
0x000000003e171b68        5        4      1 0x00000000           d3b1bbc7-c020-4056-9ded-7c6f40b5a2fc
0x000000003f984208        2        1      0 0x852c41d0 2108:3712 ad1751de900a1713cecd716adfda611f
0x000000003f99a228        2        1      1 0x00000000           WininetStartupMutex
0x000000003f9ddef8        2        1      0 0x872aabe0  2108:668 cb16681dee85a67993f0759da19566be
0x000000003fcd69a0        2        1      1 0x00000000           WininetConnectionMutex


No notable changes to highlight from the following plugins
===========================================================================

consoles
deskscan
drivermodule
driverscan
driverirp
devicetree
callbacks

</pre>

Recomended Options
-------------------
A number of options is available in VolDiff and can be used as follows:

<pre>
./VolDiff.sh path/to/baseline.vmem path/to/infected.vmem profile [Options]
</pre>

The recommanded options to use with VolDiff are:

 `--process-checks` : check process parent/child relationships, execution path, unusual loaded DLLs etc.
 
 `--registry-checks` : spot changes in registry keys that are commonly used by malware for persistence.
 
 `--string-checks` : search for suspicious strings in memory (IPs, domains, emails etc).

The options above are experimental (may generate false positives), and will slow down VolDiff execution.

Use `--help` to view all the available options: 
<pre> ./VolDiff.sh --help </pre>

Licence
--------
Free open source software. 

Tested using Volatility 2.4 (vol.py) and Windows 7 memory images.

Feedback and bugs
-------------------
Please submit feedback, report bugs, or send ideas that you want to see implemented to houcem.hachicha[@]gmail.com.

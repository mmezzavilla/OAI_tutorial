# OAI_tutorial

Run gNodeB (B210) with this command: 

```
cd ~/openairinterface5g/cmake_targets/ran_build/build
sudo ./nr-softmodem -O ../../../targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf --gNBs.[0].min_rxtxtime 6 --rfsim
```

Then, run UE in debg mode (also a B210):

```
sudo gdb --args ./nr-uesoftmodem -r 106 --numerology 1 --band 78 -C 3619200000 --uicc0.imsi 001010000000001 --rfsim
```

Then, run the gdb debugger. To start, for example, set a breakpoint in the function that performs SSB scanning
```
b nr_initial_sync.c:nr_scan_ssb
```
Or:
```
b nr-ue.c:UE_thread
```

# OAI_tutorial

Run gNodeB (B210) with this command: 

```
sudo ./nr-softmodem -O ../../../targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band41.fr1.52PRB.usrpb210.conf --gNBs.[0].min_rxtxtime 6 --continuous-tx
```

Then, run UE in debg mode (also a B210):

```
sudo gdb --args ./nr-uesoftmodem -r 52 --numerology 0 --band 41 -C 2593350000 --uicc0.imsi 001010000000001 --ssb 192
```

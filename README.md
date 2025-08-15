# OAI Initial Access Tutorial

Run gNodeB (B210) with this command: 

```
cd ~/openairinterface5g/cmake_targets/ran_build/build
sudo ./nr-softmodem -O ../../../targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf --gNBs.[0].min_rxtxtime 6 --rfsim
```

Then, run UE in debg mode (also a B210):

```
sudo gdb --args ./nr-uesoftmodem -r 106 --numerology 1 --band 78 -C 3619200000 --uicc0.imsi 001010000000001 --rfsim
```

Then, run the gdb debugger. To start, set a breakpoint in the function that performs SSB scanning (b stands for _break_; type w, or _where_, to see where you are in the code):
```
b nr_initial_sync.c:nr_scan_ssb
```
Now, check the SSB scan args (p stands for _print_):
```
p *(nr_ue_ssb_scan_t *)arg
```
You should see something like this: 
```
$1 = {gscnInfo = {gscn = 0, ssRef = 0, ssbFirstSC = 516}, foFlag = 0, targetNidCell = -1, rxdata = 0x7fffbc0008e0, fp = 0x7fffdea96c10, proc = 0x7fffb8025548, nFrames = 2, halfFrameBit = 0, symbolOffset = 0, ssbIndex = 0, ssbOffset = 0, 
  nidCell = 0, freqOffset = 0, syncRes = {cell_detected = false, rx_offset = 0, frame_id = 0}, pbchResult = {decoded_output = "\000\000", xtra_byte = 0 '\000'}, pssCorrPeakPower = 0, pssCorrAvgPower = 0, adjust_rxgain = 0, 
  ans = 0x7fffd37fc4c0}
```
Then, execute the next line by typing n, which stands for _next_, and print, for example, the PSS sequence number:
```
p pss_sequence
```


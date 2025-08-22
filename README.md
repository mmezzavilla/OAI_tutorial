# OAI Initial Access Tutorial

Run gNodeB (B210) with this command: 

```
cd ~/openairinterface5g/cmake_targets/ran_build/build
sudo ./nr-uesoftmodem -r 273 --numerology 1 -C 3450720000 --band 78 --uicc0.imsi 001010000000001 --ue-fo-compensation --usrp-args "type=n3xx, name=ni-n3xx-3264E25, addr=192.168.10.2, clock_source=internal, time_source=internal" -O ~/openairinterface5g/targets/PROJECTS/GENERIC-NR-5GC/CONF/ue.conf --ue-nb-ant-rx 1 -d --ssb 1518 --ue-rxgain 60 --ue-txgain 20 --device.recplay.subframes-record 1 --device.recplay.subframes-file /tmp/iqfile.dat --device.recplay.use-mmap 1 --device.recplay.subframes-max 20000
```

Then, run UE in debug mode, and replay the IQ samples stored in the previous step:

```
sudo gdb --args ./nr-uesoftmodem -r 273 --numerology 1 -C 3450720000 --band 78 --uicc0.imsi 001010000
000001 --ue-fo-compensation --usrp-args "type=n3xx, name=ni-n3xx-3264E25, addr=192.168.10.2, clock_source=internal, time_source=internal" -O ~/openairinterface5g/targets/PROJECTS/GENERIC-NR-5G
C/CONF/ue.conf --ue-nb-ant-rx 1 -d --ssb 1518 --ue-rxgain 60 --ue-txgain 20 --device.recplay.subframes-replay 1 --device.recplay.subframes-file /tmp/iqfile.dat --device.recplay.use-mmap 1
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


# ACQ400_HAPI_MATLAB

Matlab integration with HAPI, allows control of ACQ400 from Matlab(R)

0. HAPI loaded from git. Set PYTHONPATH. Best used with Anaconda.

1. Open Matlab with current folder as ACQ400_HAPI_MATLAB directory.

2. check_pypath is set correctly
From the Matlab prompt >>
```
>> check_pypath
C:/ACQ400_HAPI/acq400_hapi
```

3. edit name_UUTS.m to suit

```
global UUTS

%UUTS = {'acq2106_176'}
UUTS = {'acq2106_176','acq2106_054'}
%UUTS = {'acq2106_295','acq2106_293','acq2106_294'}

>> name_UUTS

UUTS =

  1×2 cell array

    {'acq2106_176'}    {'acq2106_054'}
```

Basic HAPI test

![Hello World](https://github.com/D-TACQ/ACQ400_HAPI_MATLAB/releases/download/v001/Screenshot.from.2021-08-03.14-17-19.png "Hello World")

```
>> acq400_hello_world
 
SUCCESS: acq2106_176 acq2106sfp,AO424ELF N=32 M=41 
SUCCESS: acq2106_054 acq2106,ACQ424ELF,ACQ424ELF,ACQ424ELF,ACQ424ELF,ACQ424ELF,ACQ424ELF 
```

4. set up clocking
Screenshot from 2021-08-03 14-17-19
Screenshot from 2021-08-03 14-19-30
```
>> sync_role(1e6)
```


5. create a waveoform in Matlab
Screenshot from 2021-08-03 14-21-05
```
>> make_waves("ramp",0.1,0.25)
filename =
    "DATA/myfile.raw"
```
Creates this file:
Screenshot from 2021-08-03 14-22-27

6. Load Waveform

Screenshot from 2021-08-03 14-23-35

```
>> load_waves()
>> load_waves()
 
load_raw 
load_uut load_uut acq2106_176 continuous 
hello 0 acq2106_176 
uut set acq2106_176 
hello 1 continuous 
source 524288 X 32 => 16777216 (buffer 16777216) 
dist_bufferlen set 4194304.0 
expand (8388608,) 8388608 int16 bytes 16777216 
exit 
quitting time 
```

7. Run a Capture
Screenshot from 2021-08-03 14-27-05

```
>> awg_mgtdram_runner()
 
fs:256 ssb:1000148 MBPS:256.037888 nbufs:80 
 
run_shot 1 
 
TIMING:func:'restart_awg' took: 2.80 sec 
spawned acq2106_054 
acq2106_054 ARMED 
acq2106_176 trigger 
TIMING:func:'trigger' took: 9.54 sec 
Capture Monitor 
  s:uut         S samples s:seconds, S:state 
  1:acq2106_054 2  1048576 
  2:acq2106_054 2  2097152 
  3:acq2106_054 5  2293760 
  4:acq2106_054 5  2293760 
  5:acq2106_054 0        0 
TIMING:func:'capture_monitor' took: 5.27 sec 
Offload Monitor 
  s uut      pull buffers 
  2:acq2106_054  13 
  4:acq2106_054  13 
  6:acq2106_054  28 
TIMING:func:'offload_monitor' took: 8.02 sec 
reaped acq2106_054 
Fetching calibration data 
TIMING:func:'save_egu' took: 17.72 sec 
TIMING:func:'run_shot' took: 54.88 sec 

```

8. Demux

```
>> demux_data('acq2106_054/0001.dat',16,128,100000)
>> whos CHx
  Name      Size                  Bytes  Class    Attributes

  CHx       1x128            1342191616  cell

```

Screenshot from 2021-08-03 14-30-42

```
simple_plot("acq2106_054", 1:2:32, 16, 10)

>> simple_plot()

Not enough input arguments.
 location, ch_mask, word_length, volts, xmin, xmax

Example run

simple_plot("acq2106_054", 1:32, 16, 10)

demux_data("acq2106_054", 1:32, 16, 10, 1000, 2000)


```

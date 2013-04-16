# pysbf

A Python module to read "Septentrio Binary Format" (SBF) files generated by Septentrio receivers.

## Dependencies

### Required:
* A C compiler (GCC prefered).
* Python 2.6+ (Make sure your C compiler can find the Python API `"Python.h"` header file)

### Optional:
* Cython v0.18+ 


## Installation

A Python distutils **setup.py** script is included.
Run this script with the Python interpreter you would normally use.  
For example,
>python2 setup.py install

## Release Notes

* Up to 100x times faster parsing (than pure Python, thanks to Cython). Python module is writen in C and complied for C-like speeds.
* All blocks documented in documentation v1.13.0 are now supported.

## Usage

TBA


## Examples

Print the block name for the first 100 blocks:

```python
import pysbf
    
with open('./dummy.sbf') as sbf_fobj:
 for blockName, block in pysbf.load(sbf_fobj, limit=100):
  print blockName
```
      
Print the azimuth & elevation for each visible satellite using the first 100 *SatVisibility* blocks:

```python
import pysbf
    
with open('./dummy.sbf') as sbf_fobj:
 for blockName, block in pysbf.load(sbf_fobj, limit=100, blocknames={'SatVisibility'}):
  for satInfo in block['SatInfo']:
   print satInfo['SVID'], satInfo['Azimuth'], satInfo['Elevation']
```

Combine with matplotlib & numpy for plots. A simple plot of CPU load vs time using the first 100 *ReceiverStatus* blocks:

```python
import matplotlib.pyplot as plt
import numpy as np
import pysbf as sbf
    
with open('./dummy.sbf') as sbf_fobj:
 cpuload = ( '{} {}\n'.format(b['TOW'], b['CPULoad']) for bn, b in sbf.load(sbf_fobj, 100, {'ReceiverStatus_v2'}) )
 data = np.loadtxt(cpuload)
 plt.xlabel('Time (ms)')
 plt.ylabel('CPU Load (%)')
 plt.plot(data[:,0], data[:,1])
 plt.show()
```

## Issues/Errors

File a issue on github @ https://github.com/jashandeep-sohi/pysbf


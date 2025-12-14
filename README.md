# data_acquisition_RIGOL_Oscillator_and_function_generator

Tools and notebooks to drive a Rigol DS1104-Z-PLUS Oscilloscope and the Rigol DG1022 function generator.

Note : 
- This code is for a linux (Fedora) machine, connection to the rigol instruments vary from OS. For windows you might need to modify the function and class within RIGOL.py.

## Lab tests : 
_in /lab/RIGOL_data_acquisition.ipynb_

Workflow and tutorials to connect and use basic functions on the function generator and oscilloscope.

## LM741 characterization :
_in /LM741/lm741.ipynb_

Verification of the following LM741 values found in it's Datasheet :
- gain/Bode, 
- slew rate, 
- overshoot, 
- clipping, 
- CMRR, offsets.

Note : 
- This file uses the /Rigol definitions and classes to run. Make sure the pyproject.toml is well installed.



## Setup & Debug



### Linux (Fedora):
1. Create the new venv :
> python -m venv HTP_venv

2. Activate the venv and install dependencies :
```
>>> source HTP_venv/bin/activate
>>> pip install -r requirements.txt
```

3. Register our venv as a Jupyter kernel:
```
>>> python -m ipykernel install --user --name=HTP_venv --display-name="Python (HTP_venv)"
>>> jupyter kernelspec list
```
(should show) : htp_venv         
...\jupyter\kernels\htp_venv

#### Pyproject.toml

1. Activate the created venv
2. Install all its requirements
> pip install e .



#### Debug :
First check to see if pyvisa is well installed and can at least read ASRL (fictive serial ports).

Then we'll try making it read physical USB ports.

```
import pyvisa
rm = pyvisa.ResourceManager('@py')
print(rm.list_resources())
print(rm)
```

If Rigol devices are not detected (linux), use : 
> lsusb

This should show :
```
Bus 001 Device 120: ID 1ab1:0588 Rigol Technologies DS1000 SERIES
Bus 001 Device 121: ID 1ab1:04ce Rigol Technologies DS1xx4Z/MSO1xxZ series
```

.

Create `udev` rules in the following file : 
>sudo nano /etc/udev/rules.d/99-usbtmc-rigol.rules

Write : 
SUBSYSTEM=="usb", ATTR{idVendor}=="1ab1", ATTR{idProduct}=="0588", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="1ab1", ATTR{idProduct}=="04ce", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="0400", ATTR{idProduct}=="09c4", MODE="0666"


.

Verify:
>vass@NightBlue:~$ cat /etc/udev/rules.d/99-usbtmc-rigol.rules

SUBSYSTEM=="usb", ATTR{idVendor}=="1ab1", ATTR{idProduct}=="0588", MODE="0666"

SUBSYSTEM=="usb", ATTR{idVendor}=="1ab1", ATTR{idProduct}=="04ce", MODE="0666"

SUBSYSTEM=="usb", ATTR{idVendor}=="0400", ATTR{idProduct}=="09c4", MODE="0666"


.

Recharge :
> sudo udevadm control --reload-rules

> sudo udevadm trigger

Check if usbtmc is loaded :
> lsmod | grep usbtmc




### Windows (Old, might not work)

Install Ni-Visa.

1. Create a new -venv :
> python -m venv HTP_venv

2. Activate the venv and install dependencies :
```
>>> .\HTP_venv\Scripts\Activate.ps1
>>> pip install -r requirements.txt
```

3. Register our venv as a Jupyter kernel:
```
>>> python -m ipykernel install --user --name=HTP_venv --display-name="Python (HTP_venv)"
>>> jupyter kernelspec list
```
(should show) : htp_venv         C:\Users\user\AppData\Roaming\jupyter\kernels\htp_venv

#### Drivers (Windows 10):

DG1022 : 
- Goto https://www.rigolna.com/download/ 
- Filter by DG1000 in the search bar.
- then Download > UltraSigma Instrument Connectivity Driver


If Rigol devices are not detected, use : 
- Device Manager (Windows)




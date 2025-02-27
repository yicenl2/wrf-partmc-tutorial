# Installation Guide

## WRF-PartMC Supported Versions
| Model    | Version                                     | Link  |
|----------|---------------------------------------------|-------|
| WRF      | 3.9.1.1 (release date: August 28, 2017)    | [Website](https://www2.mmm.ucar.edu/wrf/users/download/get_sources.html) |
| WRF-Chem | 3.9.1 (release date: August 17, 2017)      | [Website](https://www2.mmm.ucar.edu/wrf/users/download/get_sources.html) |
| WPS      | 3.9.1 (release date: August 17, 2017)      | [Website](https://www2.mmm.ucar.edu/wrf/users/download/get_sources.html) |


## Contents
* [WRF-Chem](#wrf)
* [WPS](#wps)
* [WRF-PartMC](#wrf-partmc)

## WRF-Chem Installation Guide
<span style="font-size: 13px;">_<b>New to WRF?</b> If you're just getting started with WRF or WRF-Chem, follow these steps to install WRF-Chem and WPS to explore and familiarize yourself with WRF._</span>

**1.** Download the correct version of WRF.
```shell
wget https://www2.mmm.ucar.edu/wrf/src/WRFV3.9.1.1.TAR.gz
```

**2.** Unzip the file and move to the directory.
```shell
tar -zxvf WRFV3.9.1.1.TAR.gz
cd WRFV3
```

**3.** Set environmnet variable for NETCDF.
```shell
export NETCDF=/sw/netcdf4-4.7.4-gnu-9.3.0
```
✨ TIP: You can check the path by typing `nc-config --prefix`

**4.** Configure WRF.
```shell
./configure
```
* Compiler choice: 34
* Nesting option: 1

✅ CHECK: This will generate a `configure.wrf` file.

**5.** Edit the `configure.wrf` file by applying the following change:

Change:
`DM_CC = mpicc`

To:
`DM_CC = mpicc -DMPI2_SUPPORT`

**6.** Download the correct version of WRF-Chem.
```shell
# Move to one-level upper directory
cd ../
wget https://www2.mmm.ucar.edu/wrf/src/WRFV3-Chem-3.9.1.TAR.gz
```

**7.** Extract the chemistry code into WRF directory.
```shell
tar -zxvf WRFV3-Chem-3.9.1.TAR.gz -C ./WRFV3/
```

✅ CHECK: There should be a `chem` folder inside `WRFV3` directory.

**6.** Compile the code.
```shell
./compile em_real
```

✅ CHECK: If sucessful, this will create `real.exe` and `wrf.exe` in directory `main/`

## WPS Installation Guide
🔶 Make sure that the WRF model has been compiled one directory level up (i.e., `../`) in a directory named `WRF`, `WRFV3`, etc., or specify the full path to the compiled WRF model with the environment variable `$WRF_DIR`.

**1.** Download the correct version of WPS.
```shell
wget 
```

**2.** Unzip the file and move to the directory.
```shell
tar -zxvf 
cd WPS
```

**2.** Follow the two-step build mechanism.
```shell
./configure
./compile
```

---
[⬆️ Back to Top](#overview)

[⏪ Return to Home](readme.md)
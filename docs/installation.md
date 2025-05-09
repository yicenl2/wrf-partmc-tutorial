# Installation Guide

<button onclick="window.scrollTo({ top: 0, behavior: 'smooth' });" style="position: fixed; bottom: 20px; right: 20px; background-color: #919497; color: white; border: none; padding: 8px 10px; cursor: pointer; border-radius: 5px; font-size: 30px;">
  🔝
</button>

## WRF-PartMC Related Components

| Tool        | Version  | Link  |
|-------------|----------|----------------------------------------------------------------------------------|
| WRF         | 3.9.1.1  | [🔗](https://www2.mmm.ucar.edu/wrf/users/download/get_sources.html) |
| WRF-Chem    | 3.9.1    | [🔗](https://www2.mmm.ucar.edu/wrf/users/download/get_sources.html) |
| WPS         | 3.9.1+   | <img src="https://twenty-icons.com/github.com" alt="Logo" width="18" style="vertical-align: middle;"> |
| MCIP        | 4.3+     | <img src="https://twenty-icons.com/github.com" alt="Logo" width="18" style="vertical-align: middle;"> |
| I/O API     | 3.2      | [🔗](https://www.cmascenter.org/ioapi/) |
| WRF-PartMC  | 1.0      | <img src="https://twenty-icons.com/github.com" alt="Logo" width="18" style="vertical-align: middle;"> |



## Contents
- [WRF Installation Guide](#wrf-installation-guide)
- [WPS Installation Guide](#wps-installation-guide)
- [MCIP Installation Guide](#mcip-installation-guide)
- [WRF-PartMC Installation Guide](#wrf-partmc-installation-guide)

## WRF Installation Guide

**1.** Download the correct version of WRF.
```shell
wget https://www2.mmm.ucar.edu/wrf/src/WRFV3.9.1.1.TAR.gz
```

**2.** Unzip the file and move to the directory.
```shell
tar -zxvf WRFV3.9.1.1.TAR.gz
cd WRFV3
```

**3.** Set environmental variable for NETCDF.
```shell
export NETCDF=$(nc-config --prefix)
```
**4.** Configure WRF.
```shell
./configure
```
* Compiler choice: 34
* Nesting option: 1

**5.** Edit the generated `configure.wrf` file:

* Modity `DM_CC` to include the `-DMPI2_SUPPORT` flag: `DM_CC = mpicc -DMPI2_SUPPORT`

**6.** Download the correct version of WRF-Chem.
```shell
# Move up one level in the directory
cd ../
wget https://www2.mmm.ucar.edu/wrf/src/WRFV3-Chem-3.9.1.TAR.gz
```

**7.** Extract the chemistry code into WRF directory.
```shell
tar -zxvf WRFV3-Chem-3.9.1.TAR.gz -C ./WRFV3/
```

✨ You will find a `chem/` directory inside `WRFV3/`.

**8.** Compile the code.
```shell
./compile em_real >& log.compile
```

<div style="background-color: #eafaf1; border-left: 5px solid #4CAF50; padding: 5px 10px 5px 10px;">
  <strong style="color: #4CAF50">Check:</strong> 
  
  If sucessful, this will create <code>real.exe</code> and <code>wrf.exe</code> in the <code>main/</code> directory.

  <br>

  <center><img src="assets/img/wrfbuildsuccess.jpg" class="img-wrfflow" alt="" style="width: 70%;"></center>
</div>

## WPS Installation Guide
<span style="font-size: 13px;">_Make sure that the WRF model has been compiled **one directory level up** (i.e., `../`) in a directory named `WRF`, `WRFV3`, etc., or specify the full path to the compiled WRF model with the environment variable `$WRF_DIR`._</span>

**1.** Download the latest version of WPS and move to the directory.
```shell
git clone https://github.com/wrf-model/WPS
cd WPS
```

**2.** Set environmental variable for NETCDF.
```shell
export NETCDF=$(nc-config --prefix)
```

**3.** Follow the two-step build mechanism.
```shell
./configure
./compile
```
* Compiler choice: 1

<div style="background-color: #eafaf1; border-left: 5px solid #4CAF50; padding: 5px 10px 5px 10px;">
  <strong style="color: #4CAF50">Check:</strong> 
  
  If successful, the <code>WPS/</code> directory will contain three executables: <code>geogrid.exe</code>, <code>metgrid.exe</code>, and <code>ungrib.exe</code>.
</div>

## MCIP Installation Guide 

#### 🔸 **Install the prerequisite: I/O API**
<span style="font-size: 13px;">_You can find detailed installation instructions [here](https://cjcoats.github.io/ioapi/AVAIL.html#build)._</span>

**1.** Download the source code:
```shell
wget https://www.cmascenter.org/ioapi/download/ioapi-3.2.tar.gz
```

**2.** Extract the code and set environmental variables.
```shell
mkdir ioapi-3.2
tar -xvzf ioapi-3.2.tar.gz -C ioapi-3.2
cd ioapi-3.2
export NETCDF=/sw/netcdf4-4.7.4-gnu-9.3.0
```

**3.** Build `ioapi/Makefile`.

```shell
# Move to the directory that contains `Makeinclude.*` files.
cd ioapi
cp Makefile.nocpl Makefile
```
Edit the _Makefile_:
* Set the base directory: `BASEDIR = /PATH_TO_YOUR_ioapi-3.2_DIRECTORY/`
* Modify `FFLAGS` to include the `-fPIC` flag: 
  
  `FFLAGS  = $(DEFINEFLAGS) $(FOPTFLAGS) $(OMPFLAGS) $(ARCHFLAGS) -I${IODIR} -fPIC`

* Edit make-targets (`bins`, `binclean`, `bindirs`, `binrelink`) according to your choice of BINs.

    📝 NOTE: Select appropriate binary-type(s) `BIN` according to the file-extensions of the `ioapi/Makeinclude.*`.

    <ins>*It is recommended that you choose both optimized and debug BINs*</ins>. For example:
    * `Linux2_x86_64gfort`
    * `Linux2_x86_64gfortdbg`

<center><img src="assets/img/ioapimakefile.png" class="img-wrfflow" alt="" style="width: 40%;"></center>

**4.** Similarly, build `m3tools/Makefile`.
```shell
cd ../m3tools
cp Makefile.nocpl Makefile
```

Edit the _Makefile_:
* Set the base directory: `BASEDIR = /PATH_TO_YOUR_ioapi-3.2_DIRECTORY/`

* Modify `FFLAGS` to include the `-fPIC` flag: 

    `FFLAGS = -I$(IODIR) ${MODI}$(OBJDIR) $(ARCHFLAGS) $(FOPTFLAGS) $(ARCHFLAGS) -fPIC`

* Customize the _make_-variable `LIBS` to deal with 🚨'netCDF4 library issues'🚨:
  
    `LIBS = -L${OBJDIR} -lioapi -L/sw/netcdf4-4.7.4-gnu-9.3.0/lib -lnetcdff -lnetcdf $(OMPLIBS) $(ARCHLIB) $(ARCHLIBS)`

* Edit make-targets `bins`, `binclean`, `bindirs`, `binrelink` according to your choice of BINs.


**5.** Compile _ioapi_ and _m3tools_.

* For *each* `BIN`: 
    ```shell
    cd ioapi
    export BIN='Linux2_x86_64gfort'
    make dir
    make
    cd ../m3tools
    make
    ``` 

<div style="background-color: #eafaf1; border-left: 5px solid #4CAF50; padding: 5px 10px 5px 10px;">
  <strong style="color: #4CAF50">Check:</strong> 
  
  If successful, the object directory <code>$BASEDIR/${BIN}</code> will be created. Make sure the directories is not empty.
</div>

#### 🔸 **Build and compile MCIP**

**1.** Clone the repository and move to the directory.
```shell
git clone https://github.com/USEPA/CMAQ
cd CMAQ/PREP/mcip/
```

**2.** Modify the _Makefile_ under `src/` directory (Select compiler).

* Uncomment contents under `#...gfortran` and set the following:

<center><img src="assets/img/mcipmakefile1.jpg" class="img-wrfflow" alt="" style="width: 80%;"></center>

* Comment contents under `#...Intel Fortran`

<center><img src="assets/img/mcipmakefile2.png" class="img-wrfflow" alt="" style="width: 65%;"></center>

**3.** Compile the code under `src/` directory.
```shell
make
```

<div style="background-color: #eafaf1; border-left: 5px solid #4CAF50; padding: 5px 10px 5px 10px;">
  <strong style="color: #4CAF50">Check:</strong> 
  
  If successful, the executable <code>mcip.exe</code> will be created inside the <code>src/</code> folder.
</div>

## WRF-PartMC Installation Guide

**1.** Download the source code and move to the directory.
```shell
git clone https://github.com/open-atmos/wrf-partmc
cd wrf-partmc
```

**2.** Checkout the PartMC submodule.
```shell
git submodule update --init partmc
```

✨ if you have access to MOSAIC, you may check it out by:
```shell
git submodule update --init mosaic
```

**3.** Set up the environment variables.
```shell
export MOSAIC=1
export WRF_CHEM=1
export NETCDF=$(nc-config --prefix)
```

**4.** Move to the `WRFV3/` directory and follow the processes for a standard WRF installation ([see above](#wrf-installation-guide)).
```shell
cd WRFV3
./configure
./compile em_real
```

<div style="background-color: #eafaf1; border-left: 5px solid #4CAF50; padding: 5px 10px 5px 10px;">
  <strong style="color: #4CAF50">Check:</strong> 
  
  If sucessful, this will create <code>real.exe</code> and <code>wrf.exe</code> in the <code>main/</code> directory.
</div>

<br>

<div style="background-color: #e8d9f1; border-left: 5px solid #6f42c1; padding: 5px 10px 5px 10px;">
  <strong style="color: #6f42c1">Troubleshooting:</strong> 
  <br>
  <code>Fatal Error: Cannot open module file '*.mod' for reading at (1): No such file or directory compilation terminated.</code>
  <br>
  <p><em>Solution</em>:</p>
  <ul>
    <li>Search for <code>INCLUDE_MODULES = </code> and add this line: 
      <pre><code>-I$(WRF_SRC_ROOT_DIR)/../mosaic -I$(WRF_SRC_ROOT_DIR)/../mosaic/datamodules \</code></pre>
    </li>
    <li>Search for <code>ENVCOMPDEFS =</code> and add the flag:
      <pre><code>-DPMC_USE_MOSAIC_MULTI_OPT</code></pre>
    </li>
    <li>Search for <code>LIB_MOSAIC =</code> and add the following:
      <pre><code>$(WRF_SRC_ROOT_DIR)/../mosaic/libmosaic.a</code></pre>
    </li>
    <li>Make sure to clean the direcotry <code>./clean</code> before re-compiling.
    </li>
  </ul>
</div>


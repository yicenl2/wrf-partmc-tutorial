# WRF Preprocessing System (WPS)

## Overview
The WRF Preprocessing System (WPS) consists of three components:

* [**Ungrib**](#ungrib) - Processes meteorological GRIB data.
* [**Geogrid**](#geogrid) - Creates terrestrial data from static geographic data.
* [**Metgrid**](#metgrid) - Interpolates the meteorological data onto the model domain.

## **Ungrib**

<ins style='font-size: 20px'>**Steps to run UNGRIB**</ins>

**1.** Download the GRIB data and place in a unique directory (details in [Resources](resources.md))

**2.** Link the GFS Table (`Vtable`)
```shell
ln -sf ungrib/Variable_Tables/Vtable.HRRR.bkb Vtable
```

**3.** Link the input GRIB data, for example:
```shell
./link_grib.csh ../data/hrrr_01/hrrr
```

**4.** Edit the `&share` and <span style="color: magenta;">`&ungrib`</span> sections of the `namelist.wps` file for your domain setup.

✨ TIP: You only need to pay attention to the following parameters:
* `start_date`
* `end_date`
* `interval_seconds`

**5.** Run `ungrid.exe` to create intermediate files
```shell
./ungrid.exe
```

✅ CHECK: Output will be in the format of `FILE:YYYY-MM-DD_hh`.


## **Geogrid** 

<ins style='font-size: 20px'>**Steps to run GEOGRID**</ins>

**1.** Download the terrestrial data (details in [Resources](resources.md))

**2.** Edit the `&share` and <span style="color: magenta;">`&geogrid`</span> sections of the `namelist.wps` file for your domain setup.

✨ TIP: Run `ncl util/plotgrids.ncl` to ensure your domain is in the right location.

**3.** Run `geogrid.exe`
```shell
./geogrid.exe
```

✅ CHECK: Output will be in the format of `geo_em.d<nn>.nc`.


## **Metgrid** 

📝 NOTE: Input to Metgrid is the `geo_em.d<nn>.nc` and `FILE:YYYY-MM-DD_hh`.


<ins style='font-size: 20px'>**Steps to run METGRID**</ins>

**1.** Edit the `&share` and <span style="color: magenta;">`&metgrid`</span> sections of the `namelist.wps` file for your domain setup.

**2.** Run `metgrid.exe`
```shell
./metgrid.exe
```

✅ CHECK: Output will be in the format of `met_em.d<nn>YYYY-MM-DD_hh:00:00.nc`.


## An example of `namelist.wps`
```bash
&share
 wrf_core = 'ARW',
 max_dom = 1,
 start_date = '2022-07-05_00:00:00',
 end_date   = '2022-07-07_00:00:00',
 interval_seconds = 21600,
/

&geogrid
 parent_id         =   1,
 parent_grid_ratio =   1,
 i_parent_start    =   1,
 j_parent_start    =   1,
 e_we              =  190,
 e_sn              =  133,
 geog_data_res = 'usgs_30s',
 dx = 4000,
 dy = 4000,
 map_proj = 'lambert',
 ref_lat   =  29.096169,
 ref_lon   = -95.592316,
 truelat1  =  33.0,
 truelat2  =  45.0,
 stand_lon = -97.0,
 geog_data_path = '/data/keeling/a/yicenl2/e/project_wrf/data/geog',
 ref_x = 95.0,
 ref_y = 66.0,
/

&ungrib
 out_format = 'WPS',
 prefix = 'FILE',
/

&metgrid
 fg_name = 'FILE'
/
```
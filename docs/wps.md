# WRF Preprocessing System (WPS)

<button onclick="window.scrollTo({ top: 0, behavior: 'smooth' });" style="position: fixed; bottom: 20px; right: 20px; background-color: #919497; color: white; border: none; padding: 8px 10px; cursor: pointer; border-radius: 5px; font-size: 30px;">
  🔝
</button>

## Overview
The WRF Preprocessing System (WPS) consists of three components:

* [**Ungrib**](#steps-to-run-ungrib) - Processes meteorological GRIB data.
* [**Geogrid**](#steps-to-run-geogrid) - Creates terrestrial data from static geographic data.
* [**Metgrid**](#steps-to-run-metgrid) - Interpolates the meteorological data onto the model domain.

## **Steps to run UNGRIB**

**1.** Download the GRIB data and place in a unique directory.

**2.** Link the GFS Table (`Vtable`)
```shell
ln -sf ungrib/Variable_Tables/Vtable.HRRR.bkb Vtable
```

**3.** Link the input GRIB data, for example:
```shell
./link_grib.csh ../data/hrrr_01/hrrr
```

**4.** Edit the `&share` and `&ungrib` sections of the `namelist.wps` file for your domain setup ([example](#an-example-of-namelistwps)).

✨ You only need to pay attention to the following parameters:
* `start_date`
* `end_date`
* `interval_seconds`

**5.** Run `ungrid.exe` to create intermediate files
```shell
./ungrid.exe
```

<div style="background-color: #eafaf1; border-left: 5px solid #4CAF50; padding: 5px 10px 5px 10px;">
  <strong style="color: #4CAF50">Check:</strong> 
  
  Output will be in the format of <code>FILE:YYYY-MM-DD_hh</code>
</div>

## **Steps to run GEOGRID**

**1.** Download the terrestrial data.

**2.** Edit the `&share` and `&geogrid` sections of the `namelist.wps` file for your domain setup ([example](#an-example-of-namelistwps)).

✨ Run `ncl util/plotgrids.ncl` to ensure your domain is in the right location.

**3.** Run `geogrid.exe`
```shell
./geogrid.exe
```

<div style="background-color: #eafaf1; border-left: 5px solid #4CAF50; padding: 5px 10px 5px 10px;">
  <strong style="color: #4CAF50">Check:</strong> 
  
  Output will be in the format of <code>geo_em.d01.nc</code>
</div>

## **Steps to run METGRID**

📝 NOTE: Input to Metgrid is the `geo_em.d<nn>.nc` and `FILE:YYYY-MM-DD_hh`.

**1.** Edit the `&share` and `&metgrid` sections of the `namelist.wps` file for your domain setup ([example](#an-example-of-namelistwps)).

**2.** Run `metgrid.exe`
```shell
./metgrid.exe
```

<div style="background-color: #eafaf1; border-left: 5px solid #4CAF50; padding: 5px 10px 5px 10px;">
  <strong style="color: #4CAF50">Check:</strong> 
  
  Output will be in the format of <code>met_em.d01.YYYY-MM-DD_hh:00:00.nc</code>
</div>

## An example of namelist.wps
```fortran
! An example for a single domain.
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
 geog_data_path = '/PATH_TO_THE_TERRESTRIAL_DATA/',
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
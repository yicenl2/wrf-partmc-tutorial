# WRF Preprocessing System (WPS)

## Overview
The WRF Preprocessing System (WPS) consists of three components:

* [**Ungrib**](#ungrib) - Processes meteorological GRIB data.
* [**Geogrid**](#geogrid) - Creates terrestrial data from static geographic data.
* [**Metgrid**](#metgrid) - Interpolates the meteorological data onto the model domain.

## **Ungrib**
<div style="border-left: 5px solid #FF9800; background: #FFF3E0; padding: 10px; border-radius: 5px;">
    <strong style="color: #FF9800;">NOTE</strong>
    <ul style="font-size: 14px; margin-top: 0; padding-left: 20px;">
        <li><em>Ungrib is NOT dependent on any WRF model domain.</em></li>
        <li><em>Ungrib is NOT dependent on Geogrid.</em></li>
    </ul>
</div>

<ins> **_Steps to run UNGRIB_** </ins>

**1.** Download the GRIB data and place in a unique directory (details in [Resources](resources.md))

**2.** Link the GFS Table (`Vtable`)
```shell
ln -sf ungrib/Variable_Tables/Vtable.HRRR.bkb Vtable
```

**3.** Link the input GRIB data, for example:
```shell
./link_grib.csh ../data/hrrr_01/hrrr
```

**4.** Edit the `&share` and <span style="color: magenta;">`&ungrib`</span> sections of 
the `namelist.wps` file for your domain setup.
> 💡 Tip: You only need to pay attention to the following parameters:
> * `start_date`
> * `end_date`
> * `interval_seconds`

**5.** Run `ungrid.exe` to create intermediate files
```shell
./ungrid.exe
```

<div style="border-left: 5px solid #28a745; background: #eafaf1; padding: 10px; border-radius: 5px;">
    <strong style="color: #28a745;">CHECK</strong>
    <ul style="font-size: 14px; margin-top: 0; padding-left: 20px;">
        <li><em>Output will be in the format of <code>FILE:YYYY-MM-DD_hh</code>.</em></li>
    </ul>
</div>

## **Geogrid** 

<ins> **_Steps to run GEOGRID_** </ins>

**1.** Download the terrestrial data (details in [Resources](resources.md))

**2.** Edit the `&share` and <span style="color: magenta;">`&geogrid`</span> sections 
of the `namelist.wps` file for your domain setup.
> 💡 Tip: Run `ncl util/plotgrids.ncl` to ensure your domain is in the right location before running `geogrid.exe`.

**3.** Run `geogrid.exe`
```shell
./geogrid.exe
```

<div style="border-left: 5px solid #28a745; background: #eafaf1; padding: 10px; border-radius: 5px;">
    <strong style="color: #28a745;">CHECK</strong>
    <ul style="font-size: 14px; margin-top: 0; padding-left: 20px;">
        <li><em>Output will be in the format of <code>geo_em.d&lt;nn&gt;.nc</code>.</em></li>
    </ul>
</div>



## **Metgrid** 
<div style="border-left: 5px solid #FF9800; background: #FFF8E1; padding: 15px 20px; border-radius: 5px; margin-bottom: 15px;">
  <strong style="color: #FF9800; font-size: 18px; display: block; margin-bottom: 10px;">NOTE</strong>  
  <ul style="font-size: 14px; margin-top: 0; padding-left: 20px;">
    <li>Input to Metgrid is the <code>geo_em.d&lt;nn&gt;.nc</code> and <code>FILE:YYYY-MM-DD_hh</code>.</li>
  </ul>
</div>


<ins> **_Steps to run METGRID_** </ins>

**1.** Edit the `&share` and <span style="color: magenta;">`&metgrid`</span> sections of 
the `namelist.wps` file for your domain setup.

**2.** Run `metgrid.exe`
```shell
./metgrid.exe
```

<div style="border-left: 5px solid #28a745; background: #eafaf1; padding: 10px; border-radius: 5px;">
    <strong style="color: #28a745;">CHECK</strong>
    <ul style="font-size: 14px; margin-top: 0; padding-left: 20px;">
        <li><em>Output will be in the format of <code>met_em.d&lt;nn&gt;.YYYY-MM-DD_hh:00:00.nc</code>.</em></li>
    </ul>
</div>


## An example of `namelist.wps`
```fortran
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
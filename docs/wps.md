# WRF Preprocessing System (WPS)

## Overview
The WRF Preprocessing System (WPS) consists of three components:

* [Ungrib](#ungrib) - Processes meteorological GRIB data.
* [Geogrid](#geogrid) - Creates terrestrial data from static geographic data.
* [Metgrid](#metgrid) - Interpolates the meteorological data onto the model domain.

## **Ungrib**
📌 Notes:
* _Ungrib is *NOT* dependent on any WRF model domain._
* _Ungrib is *NOT* dependent on Geogrid._

### Steps to run UNGRIB
**1.** Download the GRIB data and place in a unique directory (details in [Resources](resources.md))

**2.** Link the GFS Table (`Vtable`)
```
ln -sf ungrib/Variable_Tables/Vtable.HRRR.bkb Vtable
```

**3.** Link the input GRIB data, for example:
```
./link_grib.csh ../data/hrrr_01/hrrr
```

**4.** Edit the `&share` and <span style="color: magenta;">`&ungrib`</span> sections of 
the `namelist.wps` file for your domain setup.
> 🔔 Tip: You only need to pay attention to the following parameters:
> | `start_date` | `end_date` | `interval_seconds` |

5. Run `ungrid.exe` to create intermediate files
```
./ungrid.exe
```

✅ CHECK: Output will be in the format of `FILE:YYYY-MM-DD_hh`.

## **Geogrid** 

### Steps to run GEOGRID
1. Download the terrestrial data (details in [Resources](resources.md))

2. Edit the `&share` and <span style="color: magenta;">`&geogrid`</span> sections 
of the `namelist.wps` file for your domain setup.
> 🔔 Tip: Use `plotgrids.ncl` to ensure your domain is in the right location before running `geogrid.exe`
> ``` 
> ncl util/plotgrids.ncl 
> ```

3. Run `geogrid.exe`
```
./geogrid.exe
```

✅ CHECK: Output will be in the format of `geo_em.d<nn>.nc`.


## **Metgrid** 
📌 Notes:
* Input to Metgrid is the `geo_em.d<nn>.nc` and `FILE:YYYY-MM-DD_hh`.

### Steps to run METGRID
1. Edit the `&share` and <span style="color: magenta;">`&metgrid`</span> sections of 
the `namelist.wps` file for your domain setup.
2. Run `metgrid.exe`
```
./metgrid.exe
```

✅ CHECK: Output will be in the format of `met_em.d<nn>.YYYY-MM-DD_hh:00:00.nc`.

## An example of `namelist.wps`
```
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
## Ungrib (unpack input GRIB data)
1. Download GRIB2 data
2. Link  the GFS Table
```
ln -sf ungrib/Variable_Tables/Vtable.HRRR.bkb Vtable
```
3. Link the GRIB data
```
./link_grib.csh ../data/hrrr_01/hrrr
```
4. Edit `namelist.wps`
5. Run `ungrid.exe` to create intermediate files
Make sure all fields in the `Vtable` were found in `ungrib.log`

## Geogrid (setup model domain)
1. Download terrestrial data
2. Edit `namelist.wps`
3. Create static data for the domain
```./geogrid.exe```
4. Generate `met_em*` files (interpolate input data onto model domain) 
```./metgrid.exe```
## Steps to run WRF
1. Move to the directory in which you plan to run the code, for example:
```
cd _[path_to_wrf_directory]_/test/em_real
```

2. Link **OR** copy the `met_em*` files to this directory.
```
To link:
ln -sf _[path_to_wps_directory]_/met_em.d01.2022-07-0* .
To copy:
cp _[path_to_wps_directory]_/met_em.d01.2022-07-0* .
```
 
3. Edit the `namelist.input` file for your particular run. Detailed descriptions of the namelist parameters 
can be found [here](https://www2.mmm.ucar.edu/wrf/users/wrf_users_guide/build/html/namelist_variables.html).
 
4. Run `real.exe` (verify that the program runs correctly)
❗ _Verify that the program runs correctly:_
You should have the following output files: 
* `wrfinput_d01` - Initial condition file
* `wrfbdy_d01` - Boundary condition file
> To run this on nodes, issue the command:
```
# this is an example of using 4 nodes
mpirun -np 4 ./real.exe
```

5. Run `wrf.exe` (verify that the program runs correctly)
❗ _Verify that the program runs correctly:_
You should have the output files: `wrfout_dxx_[initial_date]`

## Debug
Run for a shorter period, look for errors in `rsl.error.000*` and `rsl.out.000*`, search for keywords such as 
`Error` | `ERROR` | `FATAL`

For a detailed error message, set the namelist parameter `debug_level` to larger numbers (e.g., 100 or 1000).

## EPA_ANTHRO_EMIS

![Traditional way WRF-Chem read binned chemical emissions](/assets/img/wrf_chem_emis.png)

### Preparation:
1. Edit namelist.input, set `chem_opt=0`, `io_style_emissions = 2`, and start and end time
2. Run real.exe to generate a `wrfinput_d01` file

1. Copy `wrfinput_d<nn>`
2. Building anthro_emis
```
export FC=gfortran
export NETCDF_DIR=/sw/netcdf4-4.7.4-gnu-9.3.0 [nc-config --prefix]
./make_anthro
```
3. Edit `namelist.input`
4. To run anthro_emis issue the command:
```
./anthro_emis < anthro_emis.inp {> anthro_emis.out} [namelist.input]
```
5. Applying NEI emissions to other years:
```
- modify epa.f90 file

Change:
dateHdr(7) = &
(/ 'all     ', 'aveday_N', 'aveday_Y', 'mwdss_N ', 'mwdss_Y ', 'week_N  ', 'week_Y'  /)
 
To:
dateHdr(8) = &
(/ 'date     ', 'aveday_N', 'aveday_Y', 'mwdss_N ', 'mwdss_Y ', 'week_N  ', 'week_Y'  , 'all' /)

- Change all instances of 7 to 8

- Rebuild anthro_emis

- Modify the smk_merge_dates_... file (.xls) to include conversions for the year you’re interested in running.  
(change columns D, K, M, V)

- save the first 8 columns as .csv -> change to .txt later (note the format!)

- change names (or make copies) of sectorlist files in the anthro emission folder so the year aligns with the years you're interested in. (e.g., search for "2017" and replace with "2022")
```

## WRF-Chem
1. Link in the wrfchemi files
```
ln -sf ../../../EPA_ANTHRO_EMIS/wrfchemi_d01_2017-07-* .
```
2. Set the following:
```
* chem_opt = 8
* io_style_emissions = 2
* emiss_opt=3 (NEI emissions for USA)
* emiss_inpt_opt = 1
* kemit = 1
* emi_inname = "wrfchemi_d<domain>_<date>"
* auxinput5_interval_m = 60
* frames_per_auxinput5 = 1 ❗ Must have
```

For debugging purpose, turn off these processes (set to 0):
```
* gaschem_onoff = 0
* aerchem_onoff = 0
* gas_drydep_opt = 0
* aer_drydep_opt = 0
* chem_opt = 0
```
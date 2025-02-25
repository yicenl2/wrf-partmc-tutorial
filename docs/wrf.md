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
# WRF Modeling System

<button onclick="window.scrollTo({ top: 0, behavior: 'smooth' });" style="position: fixed; bottom: 20px; right: 20px; background-color: #3584de; color: white; border: none; padding: 8px 10px; cursor: pointer; border-radius: 5px; font-size: 20px;">
  🔝
</button>

## Overview

Before running the WRF model, make sure that you have the meteorological data `met_em*` prepared from the WPS preprocessing system.

The WRF model has 2 steps:
* **real.exe**: Vertically interpolates the `met_em*` files and creates boundary and initial condition files.
* **wrf.exe**: Generates the model forecast.

## Steps to run WRF
**1.** Move to the directory in which you plan to run the code.
```shell
cd ~/WRF/test/em_real
```

**2.** Link _or_ copy the `met_em*` files to this directory.
```shell
# To link the files:
ln -sf ~/WPS/met_em.d01.2022-07-0* .

# To copy the files:
cp ~/WPS/met_em.d01.2022-07-0* .
```
 
**3.** Edit the `namelist.input` file for your particular run ([example](#an-example-of-namelistinput)). 
❗Always make a backup of the `namelist.input` file.
✨ Descriptions of the namelist parameters can be found [here](https://www2.mmm.ucar.edu/wrf/users/wrf_users_guide/build/html/namelist_variables.html).
 
**4.** Run `real.exe`
```shell
./real.exe
```

✨ To run on nodes, issue the command `mpirun -np 4 ./real.exe` (an example of using 4 nodes)

<div style="background-color: #eafaf1; border-left: 5px solid #4CAF50; padding: 5px 10px 1px 10px;">
  <strong style="color: #4CAF50">CHECK:</strong> 
  
  You should have the following output files: 

| File Name         | Description               |
|-------------------|---------------------------|
| `wrfinput_d01`    | Initial condition    |
| `wrfbdy_d01`      | Boundary condition   |
</div>

</br>

**5.** Run `wrf.exe`
```shell
./wrf.exe
```
<ins>*Alternativley*</ins>, you can submit a batch job with the following SLURM script:
```bash
#!/bin/tcsh
#SBATCH --job-name=[job_name]
#SBATCH -p sesempi
#SBATCH --nodes=5
#SBATCH --ntasks=100
#SBATCH --time=5-24:00:00
#SBATCH --mem-per-cpu=4000M
#SBATCH --mail-type=FAIL
#SBATCH --mail-type=END
#SBATCH --mail-user=*[your_own_email_address]*

mpirun -np $SLURM_NTASKS ./wrf.exe
```

<div style="background-color: #eafaf1; border-left: 5px solid #4CAF50; padding: 2px 10px 0px 10px;">
  <strong style="color: #4CAF50">CHECK:</strong> 
  
  Output will be in the format of <code>wrfout_d<nn>_YYYY-MM-DD_HH:mm:ss</code>.
</div>

<div style="background-color: #e8d9f1; border-left: 5px solid #6f42c1; padding: 2px 10px 0px 10px;">
  <strong style="color: #6f42c1">Troubleshooting:</strong>
  
If you encounter issues, check the `rsl.error.000*` and `rsl.out.000*` files and search for keywords such as 'Error', 'ERROR', 'FATAL'. 

For detailed error messages, it is recommended to set `debug_level` to a higher value, such as `100` or `1000`.
</div>

## An example of namelist.input
```fortran
 &time_control
 run_days                            = 5,
 run_hours                           = 0,
 run_minutes                         = 0,
 run_seconds                         = 0,
 start_year                          = 2022,
 start_month                         = 07,
 start_day                           = 04,
 start_hour                          = 00,
 start_minute                        = 00,
 start_second                        = 00,
 end_year                            = 2022,
 end_month                           = 07,
 end_day                             = 09,
 end_hour                            = 00,
 end_minute                          = 00,
 end_second                          = 00,
 interval_seconds                    = 21600
 input_from_file                     = .true.,
 history_interval                    = 60,
 frames_per_outfile                  = 1000,
 restart                             = .false.,
 restart_interval                    = 360,
 io_form_history                     = 2
 io_form_restart                     = 2
 io_form_input                       = 2
 io_form_boundary                    = 2
 debug_level                         = 0
 /

 &domains
 time_step                           = 20,
 time_step_fract_num                 = 0,
 time_step_fract_den                 = 1,
 max_dom                             = 1,
 e_we                                = 190,
 e_sn                                = 133,
 e_vert                              = 45,
 p_top_requested                     = 5000,
 num_metgrid_levels                  = 41,
 num_metgrid_soil_levels             = 9,
 dx                                  = 4000,
 dy                                  = 4000,
 grid_id                             = 1,
 parent_id                           = 0,
 i_parent_start                      = 1,
 j_parent_start                      = 1,
 parent_grid_ratio                   = 1,
 parent_time_step_ratio              = 1,
 feedback                            = 1,
 smooth_option                       = 0
 /

 &physics
 physics_suite                       = 'CONUS'
 radt                                = 30,
 bldt                                = 0,
 cudt                                = 5,
 icloud                              = 1,
 num_soil_layers                     = 4,
 num_land_cat                        = 24,
 sf_urban_physics                    = 0,
 /

 &fdda
 /

 &dynamics
 w_damping                           = 0,
 diff_opt                            = 1,
 km_opt                              = 4,
 diff_6th_opt                        = 0,
 diff_6th_factor                     = 0.12,
 base_temp                           = 290.
 damp_opt                            = 0,
 zdamp                               = 5000.,
 dampcoef                            = 0.2,
 khdif                               = 0,
 kvdif                               = 0,
 non_hydrostatic                     = .true.,
 moist_adv_opt                       = 1,   
 scalar_adv_opt                      = 1,
 gwd_opt                             = 1,
 /

 &bdy_control
 spec_bdy_width                      = 5,
 spec_zone                           = 1,
 relax_zone                          = 4,
 specified                           = .true.,
 nested                              = .false.,
 /

 &grib2
 /

 &namelist_quilt
 nio_tasks_per_group = 0,
 nio_groups = 1,
 /

```
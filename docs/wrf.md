# WRF Modeling System

## Overview

Before running the WRF model, make sure that you have the meteorological data `met_em*` prepared from the WPS preprocessing system.

The WRF model has 2 steps:
* **real.exe**: Vertically interpolates the `met_em*` files and creates boundary and initial condition files.
* **wrf.exe**: Generates the model forecast.

---
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
 
**3.** Edit the `namelist.input` file for your particular run. Detailed descriptions of the namelist parameters can be found [here](https://www2.mmm.ucar.edu/wrf/users/wrf_users_guide/build/html/namelist_variables.html).
 
**4.** Run `real.exe`
```shell
./real.exe
```

✨ TIP: **To run on nodes**, issue the command `mpirun -np 4 ./real.exe` (an example of using 4 nodes)

✅ CHECK: You should have the following output files: 

| File Name         | Description               |
|-------------------|---------------------------|
| `wrfinput_d01`    | Initial condition    |
| `wrfbdy_d01`      | Boundary condition   |

**5.** Run `wrf.exe`
```shell
./wrf.exe
```
**Alternativley**, you can submit a batch job with the following SLURM script:
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
#SBATCH --mail-user=[your_own_email_address]

mpirun -np $SLURM_NTASKS ./wrf.exe
```

✅ CHECK: Output will be in the format of `wrfout_d<nn>_YYYY-MM-DD_HH:mm:ss`.

⭕ DEBUG: If you encounter issues, check the `rsl.error.000*` and `rsl.out.000*` files and search for keywords such as 'Error', 'ERROR', 'FATAL'. 
For detailed error messages, it is recommended to set `debug_level` to a higher value, such as 100 or 1000.

❗IMPORTANT: Always make a backup of the `namelist.input` file.
# Additional Resources

<button onclick="window.scrollTo({ top: 0, behavior: 'smooth' });" style="position: fixed; bottom: 20px; right: 20px; background-color: #919497; color: white; border: none; padding: 8px 10px; cursor: pointer; border-radius: 5px; font-size: 30px;">
  🔝
</button>

## Content
* [Installation related](#installation-related)
* [WPS related](#wps-related)
* [WRF/WRF-Chem related](#wrfwrf-chem-related)
* [MCIP/SMOKE related](#mcipsmoke-related)

## Installation-related

* Build WRF and WPS on **Keeling** ([🔗 Visit site](https://computing-docs.readthedocs.io/en/latest/wrf.html))

## WPS related

##### 📖 User Guides

* WPS User's Guide ([🔗 Visit site](https://www2.mmm.ucar.edu/wrf/users/docs/user_guide_v4/v4.4/users_guide_chap3.html))

##### 🔍 Tutorials

* WPS Tutorial Presentation by Kelly Werner ([🔗 Visit site](http://140.112.69.65/research/coawst/COAWST_TUTORIAL/training_2019/monday/werner_wps.pdf))

* How to Initialize WRF with HRRR Boundary Conditions ([🔗 Visit site](https://home.chpc.utah.edu/~u0553130/Brian_Blaylock/hrrr.html))

* Get started with Cloud Storage - Downloading objects ([🔗 Visit site](https://cloud.google.com/storage/docs/downloading-objects#cli-download-object))

##### 📥 Download Data

* **About the HRRR:** The High-Resolution Rapid Refresh ([HRRR](https://rapidrefresh.noaa.gov/hrrr/)) model is a NOAA real-time 3-km resolution, hourly updated atmospheric model.

* **What are GRIB2 files?** GRIB2, or Gridded Binary Version 2, is a standard file format used by meteorologists for model data sets.

* Use [Google Cloud Platform HRRR archive](https://console.cloud.google.com/marketplace/product/noaa-public/hrrr?project=python-232920&pli=1&inv=1&invt=AbrDLw) to access past HRRR data:
    ```shell
    # download and rename the file 
    gcloud storage cp gs://high-resolution-rapid-refresh/hrrr.20220701/conus/hrrr.t00z.wrfprsf18.grib2 hrrr_2022073100f18.grib2
    ```

    ❗ Filename: `hrrr.tCCz.wrfprsfFF.grib2`, where `CC` is the **model cycle runtime** (i.e. 00, 01, 02, 03), `FF` is the **forecast hour** (i.e. 00, 03, 06, 12, 15) ([see details](https://www.nco.ncep.noaa.gov/pmb/products/hrrr/)).

    ❗ The **forecast hour** is relevant as this represents the specific time in the future for which the weather forecast is being provided.

* Download `Vtable` for HRRR `prs` data ([<img src="https://twenty-icons.com/github.com" alt="Logo" width="18" style="vertical-align: middle;" /> Vtable.hrrr.bkb](https://github.com/blaylockbk/Ute-WRF-User-Group/blob/master/Blaylock/WPS_for_HRRR/Vtable.HRRR.bkb))

* Download Geographical Input Data:
  * [WPS V3 Geographical Dataset](https://www2.mmm.ucar.edu/wrf/users/download/get_sources_wps_geog_V3.html)
    * Complete Dataset
  * [WPS V4 Geographical Dataset](https://www2.mmm.ucar.edu/wrf/users/download/get_sources_wps_geog.html)
    * Highest Resolution of each Mandatory Field
    * for Specific Applications (based on your research needs)

## WRF/WRF-Chem related

##### 📖 User Guides

* WRF User's Guide ([🔗 Visit site](https://www2.mmm.ucar.edu/wrf/users/wrf_users_guide/build/html/index.html))

* WRF V3 User's Guide ([🔗 Visit site](https://www2.mmm.ucar.edu/wrf/users/docs/user_guide_V3/contents.html))

* WRF-Chem v4.4 User's Guide ([🔗 Visit site](https://etrp.wmo.int/pluginfile.php/87070/mod_resource/content/1/Users_guide.WRF-Chemv4.4.pdf))

* WRF-Chem v3.8.1 User's Guide ([🔗 Visit site](https://repository.library.noaa.gov/view/noaa/14945))

* EPA_ANTHRO_EMIS User Guide ([🔗 Visit site](https://www.acom.ucar.edu/wrf-chem/EPA_ANTHRO_EMIS_UserGuide.pdf))

##### 🔍 Tutorials

* Basic WRF Tutorial by wrfhelp NCAR-MMM ([<img src="https://twenty-icons.com/youtube.com" alt="Logo" width="18" style="vertical-align: middle;" /> WRF Tutorial Presentations](https://www.youtube.com/playlist?list=PLJ_1sjucSSZCTNBRM4D3BfEak-XT7TKJo))

* WRF-Chem Tutorial Presentation ([🔗 Visit site](https://mce2.org/wmogurme/images/workshops/ASEAN/day3/peckham/Setup&Run_wrf-chem_tutorial.pdf))

##### 💬 Online Assistance

* WRF & MPAS-A Support Forum ([🔗 Visit site](https://forum.mmm.ucar.edu/))

* WRF-Chem Discussion Forum ([🔗 Visit site](https://www2.acom.ucar.edu/wrf-chem/discussion-forum))

##### 📚 Miscellaneous

* Best practice of `namelist.input` ([🔗 Visit site](http://www2.mmm.ucar.edu/wrf/users/namelist_best_prac_wrf.html))
  * Time & Frequency Clarification ([<img src="https://twenty-icons.com/youtube.com" alt="Logo" width="18" style="vertical-align: middle;" /> Nesting in WRF](https://www.youtube.com/watch?v=PIncq6mLh4k))

* WRF grid structure ([🔗 Visit site](https://amps-backup.ucar.edu/information/configuration/wrf_grid_structure.html))

* Reading and plotting WRF data using **wrf-python** ([🔗 Visit site](https://wrf-python.readthedocs.io/en/latest/index.html))

## MCIP/SMOKE related

##### 📖 User Guides

* SMOKE 5.1 USER MANUAL ([🔗 Visit site](https://www.cmascenter.org/help/documentation.cfm?model=smoke&version=5.1))

##### 💬 Online Assistance

* CMAS Forum ([🔗 Visit site](https://forum.cmascenter.org/))

##### 📥 Download Data

* National Emissions Inventory ([NEI](https://www.epa.gov/air-emissions-inventories/national-emissions-inventory-nei)) - Get Data ([🔗 Visit site](https://www.epa.gov/air-emissions-inventories/get-air-emissions-data-0))
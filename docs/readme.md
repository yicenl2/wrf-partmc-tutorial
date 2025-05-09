# Welcome to the WRF-PartMC Documentation!

<button onclick="window.scrollTo({ top: 0, behavior: 'smooth' });" style="position: fixed; bottom: 20px; right: 20px; background-color: #919497; color: white; border: none; padding: 8px 10px; cursor: pointer; border-radius: 5px; font-size: 30px;">
  🔝
</button>

This documentation provides a comprehensive guide to setting up and running simulations using [WRF-PartMC](https://github.com/open-atmos/wrf-partmc), a modeling system that integrates the [Weather Research and Forecasting (WRF)](https://www.mmm.ucar.edu/models/wrf) model with [PartMC](https://github.com/compdyn/partmc) (Particle-resolved Monte Carlo code for atmospheric aerosol simulation). A scientific overview of WRF-PartMC is in [Curtis et al. (2024)](https://egusphere.copernicus.org/preprints/2024/egusphere-2024-825/).

## Getting Started
* 🌱 **New to WRF/WRF-PartMC?** Start by setting up the required dependencies and compiling the model. For detailed instructions, head over to the [Installation Guide](installation.md) to get started.

* 🚀 **Already installed and compiled?** Congratulations, you're now ready to dive into WRF-PartMC! We highly recommend exploring the [WRF-ARW Online Tutorial](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/Introduction/index.php) provided by NCAR to understand the fundamentals of WRF model setup and configuration. <ins>*Alternatively*</ins>, you can explore the sections below for a concise guide on its key components and functionality. 

_**Program flow:**_
![](assets/img/anthro_emis.png)

## Contents

<span style="font-size: 13px;">❗ _All content is specifically adapted for WRF-PartMC use only!_</span>

* [Installation Guide](installation.md)
* [WPS](wps.md)
* [WRF](wrf.md)
* [WRF-Chem](wrf-chem.md)
* [Preparing Emissions with SMOKE](smoke.md)
* [WRF-PartMC](wrf-partmc.md)
* [Troubleshooting](troubleshooting.md)
* [Links & Resources](resources.md)
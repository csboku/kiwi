# WRF-Chem Configuration

Configuring a WRF-Chem simulation is primarily done through the `namelist.input` file. This file, located in the `WRFV3/test/em_real` directory, controls all aspects of the simulation, from the simulation time and domain setup to the choice of physics and chemistry options.

## `namelist.input` File Structure

The `namelist.input` file is divided into several sections, each starting with `&section_name` and ending with `/`. The most important sections for a WRF-Chem simulation are:

-   `&time_control`: Controls the simulation start and end times, the time step, and other time-related parameters. Key variables include `start_year`, `end_year`, `interval_seconds` (the interval between meteorological input files), and `time_step`.
-   `&domains`: Defines the model grid, resolution, and other domain-specific parameters. This is where you set the grid dimensions (`e_we`, `e_sn`), the horizontal resolution (`dx`, `dy`), and the number of vertical levels (`e_vert`).
-   `&physics`: Specifies the physical parameterizations to be used (e.g., microphysics, cumulus, PBL). See the [WRF-Chem Physics Options](./wrfchem_physics_options.md) guide for more details.
-   `&chem`: This is the most important section for WRF-Chem, where you define the chemical mechanism, emissions, and other chemistry-related options. See the [WRF-Chem Chemistry Options](./wrfchem_chemistry_options.md) guide for more details.
-   `&bdy_control`: Controls the boundary conditions.
-   `&grib2`: GRIB2-specific settings.
-   `&namelist_quilt`: Settings for asynchronous I/O.

## Key WRF-Chem Namelist Variables

Here are some of the most important namelist variables in the `&chem` section that you need to configure for a WRF-Chem simulation:

-   **`chem_opt`**: This is the primary switch for activating chemistry in the model. Each number corresponds to a different chemical mechanism.
    -   `0`: No chemistry.
    -   `2`: [RADM2 (Regional Acid Deposition Model 2)](./wrfchem_radm2.md)
    -   `7, 8, 35, 503`: [CBMZ (Carbon Bond Mechanism, Version Z)](./wrfchem_cbmz.md)
    -   `11`: [MOZART (Model for Ozone and Related chemical Tracers)](./wrfchem_mozart.md)
    -   `103, 105`: [RACM (Regional Atmospheric Chemistry Mechanism)](./wrfchem_racm.md)
    -   ... and many others. Refer to the WRF-Chem User's Guide for a complete list.

-   **`bio_emiss_opt`**: Controls the biogenic emissions.
    -   `0`: No biogenic emissions.
    -   `1`: MEGAN (Model of Emissions of Gases and Aerosols from Nature) with isoprene emissions.
    -   `2`: MEGAN with isoprene and monoterpene emissions.

-   **`phot_opt`**: Selects the photolysis option.
    -   `1`: Madronich photolysis scheme.
    -   `2`: Fast-J photolysis scheme.

-   **`emiss_opt`**: Controls the anthropogenic emissions.
    -   `1`: Use pre-processed emissions from a specific inventory (e.g., NEI).
    -   `2`: Use idealized emissions.

-   **`gas_drydep_opt`**: Gas-phase dry deposition option.
-   **`aer_drydep_opt`**: Aerosol dry deposition option.
-   **`chemdt`**: Chemistry time step in minutes. It's recommended to set this to be the same as the model time step (`time_step` in `&domains`).

## Example Configurations

### RADM2 Mechanism

This example shows a basic configuration for the RADM2 chemical mechanism with biogenic emissions.

```
&chem
 chem_opt = 2,
 bio_emiss_opt = 1,
 phot_opt = 1,
 emiss_opt = 1,
 gas_drydep_opt = 1,
 aer_drydep_opt = 1,
 chemdt = 0,
/
```

### MOZART Mechanism

This example shows a configuration for the MOZART chemical mechanism.

```
&chem
 chem_opt = 11,
 bio_emiss_opt = 2,
 phot_opt = 2,
 emiss_opt = 1,
 gas_drydep_opt = 1,
 aer_drydep_opt = 1,
 chemdt = 0,
/
```

**Note:** This is a simplified overview. For detailed information on all the available options and their combinations, always refer to the official WRF-Chem User's Guide for the specific version you are using.
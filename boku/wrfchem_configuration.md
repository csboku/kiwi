# WRF-Chem Configuration: A Technical Deep Dive

Configuring a WRF-Chem simulation is controlled by the `namelist.input` file. This guide provides a more technical look at the key namelist sections and variables.

## The `namelist.input` File Structure

The `namelist.input` file is a collection of Fortran namelists. Each section begins with `&section_name` and ends with `/`.

### `&time_control`

This section is critical for defining the simulation period and timing.

-   `run_days`, `run_hours`, `run_minutes`, `run_seconds`: Define the total simulation length.
-   `start_year`, `start_month`, `start_day`, `start_hour`: The simulation start time.
-   `end_year`, `end_month`, `end_day`, `end_hour`: The simulation end time.
-   `interval_seconds`: The time interval in seconds between the meteorological input files (`met_em*`). This must match the interval used when running WPS.
-   `time_step`: The model time step in seconds. A good rule of thumb is `dx` (in km) \* 6. For example, a 4km domain could use a 24-second time step.
-   `history_interval`: How often (in minutes) to write output to the `wrfout` files.

### `&domains`

This section defines the model domains.

-   `max_dom`: The number of domains (nests).
-   `e_we`, `e_sn`, `e_vert`: The dimensions of the model grid (west-east, south-north, vertical).
-   `dx`, `dy`: The grid spacing in meters.
-   `parent_grid_ratio`: The nesting ratio between a parent domain and a child domain.
-   `parent_time_step_ratio`: The time step ratio between a parent domain and a child domain.

### `&physics` and `&chem`

These sections control the physics and chemistry options. For more details, see the dedicated guides:

-   [Physics Options](./wrfchem_physics_options.md)
-   [Chemistry Options](./wrfchem_chemistry_options.md)

## Advanced Configuration and Best Practices

### Choosing a Time Step

-   The model time step (`time_step`) is critical for model stability. If it is too large, the model will crash with a `cfl` error.
-   The chemistry time step (`chemdt`) is also important. It is generally recommended to set `chemdt` to be the same as the model time step. However, for some complex mechanisms, a shorter chemistry time step may be required.

### Memory and Performance

-   The memory required by WRF-Chem depends on the domain size, number of vertical levels, and the chosen chemical mechanism.
-   Complex mechanisms like MOZART and RACM require significantly more memory than simpler mechanisms like RADM2.
-   When running in parallel, the domain is decomposed among the processors. The memory per processor depends on the size of the subdomain that each processor is responsible for.

### Example: A Nested CBMZ/MOSAIC Simulation

This example shows a more complex configuration for a two-domain nested simulation using CBMZ/MOSAIC.

```
&time_control
 run_days                            = 1,
 start_year                          = 2023, 2023,
 start_month                         = 07, 07,
 start_day                           = 15, 15,
 start_hour                          = 00, 00,
 end_year                            = 2023, 2023,
 end_month                           = 07, 07,
 end_day                             = 16, 16,
 end_hour                            = 00, 00,
 interval_seconds                    = 21600,
 time_step                           = 60,
 history_interval                    = 60,  60,
/

&domains
 max_dom                             = 2,
 e_we                                = 150, 181,
 e_sn                                = 150, 181,
 e_vert                              = 35,  35,
 dx                                  = 12000, 4000,
 dy                                  = 12000, 4000,
 parent_grid_ratio                   = 1,   3,
 parent_time_step_ratio              = 1,   3,
 i_parent_start                      = 1,   50,
 j_parent_start                      = 1,   50,
/

&chem
 chem_opt                            = 8, 8,
 bio_emiss_opt                       = 3, 3,
 phot_opt                            = 2, 2,
 emiss_opt                           = 1, 1,
 gas_drydep_opt                      = 1, 1,
 aer_drydep_opt                      = 1, 1,
 wet_scav_onoff                      = 1, 1,
 have_bcs_chem                       = .true.,
/
```

This guide provides a more in-depth look at the WRF-Chem configuration. For a complete list of all namelist variables, always refer to the `Registry/Registry.EM_COMMON` file in the WRF source code.

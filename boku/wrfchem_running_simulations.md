# Running WRF-Chem Simulations: A Technical Guide

This guide provides a more technical look at the process of running a WRF-Chem simulation, with a focus on debugging and troubleshooting.

## 1. The `real.exe` Step

The `real.exe` program is responsible for creating the initial and boundary condition files for your simulation.

-   **Input Files**: `real.exe` reads the `met_em*` files from WPS and the `namelist.input` file.
-   **Output Files**: It produces `wrfinput_d01` (initial conditions) and `wrfbdy_d01` (boundary conditions). If you are using chemical boundary conditions, it will also read `wrfbdy_d01_chem`.
-   **Common Errors**:
    -   **`ERROR: Could not find ...`**: This usually means that a required variable is missing from your `met_em*` files. Check your WPS configuration to ensure you are processing all the required meteorological fields.
    -   **`med_initialdata_input: Found no MOISTURE in input data`**: This is a common error when running with idealized data. You may need to set `force_use_old_data = .true.` in the `&grib2` namelist record.

## 2. The `wrf.exe` Step

The `wrf.exe` program is the main model executable.

-   **Input Files**: `wrf.exe` reads `wrfinput_d01`, `wrfbdy_d01`, and if specified, `wrfchemi_*` (emissions) and `wrfbdy_d01_chem` (chemical boundary conditions).
-   **Output Files**: It produces the `wrfout_*` files, which contain the model output, and the `rsl.*` log files.

## 3. Debugging Runtime Errors

When a simulation crashes, the `rsl.out.0000` and `rsl.error.0000` files are your primary source of information.

### `cfl` Errors

-   **Symptom**: The model crashes with an error message containing "cfl".
-   **Cause**: This means the Courant-Friedrichs-Lewy (CFL) condition has been violated, which indicates model instability. This is almost always caused by a time step that is too long for the given grid resolution or wind speed.
-   **Solution**:
    1.  Reduce the `time_step` in your `namelist.input` file. A good starting point is to cut it in half.
    2.  If you are using a nested domain, ensure that the `parent_time_step_ratio` is appropriate.
    3.  In rare cases, `cfl` errors can be caused by bad input data or an inappropriate physics scheme.

### Segmentation Faults

-   **Symptom**: The model crashes with a "Segmentation Fault" error.
-   **Cause**: This is a generic error that means the model tried to access memory that it shouldn't have. It can be caused by a wide range of issues, including:
    -   Incorrectly formatted input data.
    -   A bug in a specific physics or chemistry scheme.
    -   An issue with the compiler or MPI libraries.
-   **Solution**:
    1.  **Check your input files**: Carefully examine your `wrfinput`, `wrfbdy`, and `wrfchemi` files to ensure they are not corrupted and contain reasonable values.
    2.  **Simplify your configuration**: Try running with a simpler configuration (e.g., no chemistry, or a simpler chemical mechanism) to isolate the problem.
    3.  **Consult the WRF Forum**: The WRF & MPAS-A Support Forum is an excellent resource for debugging segmentation faults. It is likely that someone else has encountered the same problem.

### Other Common Errors

-   **`input_wrf: nl_get_ref_latlon: Did not find ref_lat and ref_lon in namelist`**: This means you are missing the `ref_lat` and `ref_lon` variables in the `&geog` section of your `namelist.input` file.
-   **`ERROR: Chemistry routines require ...`**: This indicates a mismatch between your input data and the chosen chemistry options. For example, some chemistry options require a specific land use dataset.

By systematically examining the log files and following these debugging steps, you can resolve most WRF-Chem runtime errors.

# WRF-Chem Running Simulations

After you have successfully configured your simulation in the `namelist.input` file, you are ready to run the WRF-Chem model. The process involves two main steps: running `real.exe` to generate the initial and boundary condition files, and then running `wrf.exe` to perform the actual simulation.

## 1. Generate Initial and Boundary Conditions (`real.exe`)

The `real.exe` program interpolates the meteorological data from your WPS output files (`met_em*`) to your model domain. It also incorporates the chemical initial and boundary conditions if you have prepared them.

To run `real.exe`, navigate to your `WRFV3/test/em_real` directory and execute the following command:

```bash
./real.exe
```

You can run this in parallel using `mpirun`:

```bash
mpirun -np <number_of_processors> ./real.exe
```

Upon successful completion, you will see two new files in your directory:

-   `wrfinput_d01`: The initial conditions for your domain.
-   `wrfbdy_d01`: The boundary conditions for your domain.

Check the `rsl.out.0000` and `rsl.error.0000` files for any errors. A successful run will show `SUCCESS COMPLETE REAL_EM INIT` at the end of the `rsl.out.0000` file.

## 2. Run the Simulation (`wrf.exe`)

Once `real.exe` has completed successfully, you can start the main simulation by running `wrf.exe`. This program integrates the model forward in time, using the initial and boundary conditions you just generated.

To run `wrf.exe`, execute the following command:

```bash
./wrf.exe
```

For parallel execution:

```bash
mpirun -np <number_of_processors> ./wrf.exe
```

The simulation will run for the duration you specified in the `&time_control` section of your `namelist.input` file.

## 3. Monitoring the Simulation

While the simulation is running, you can monitor its progress by tailing the `rsl.out.0000` file:

```bash
tail -f rsl.out.0000
```

This will show you the model output in real-time, including the current simulation time. This is useful for checking if the model is running at an acceptable speed and for spotting any obvious errors as they occur.

## 4. Checking the Output

After the simulation has finished (or if it has crashed), you will need to examine the output files to understand what happened.

-   **Log Files**: The `rsl.out.0000` and `rsl.error.0000` files are the most important for debugging. Always check the end of these files for error messages. A successful run will have a "SUCCESS COMPLETE WRF" message at the end of the `rsl.out.0000` file.

-   **Output Files**: The main output of the simulation is stored in NetCDF files named `wrfout_d<domain>_<date>`. For example, `wrfout_d01_2023-07-17_00:00:00`. These files contain all the meteorological and chemical variables at different time steps. You can use visualization software like `ncview`, `Panoply`, or scripting languages like Python with the `netCDF4` library to analyze these files.

-   **Chemistry-Specific Output**: Depending on your configuration, you might have additional output files related to chemistry, such as `wrfchemout_d<domain>_<date>`.

## Common Runtime Errors

-   **Segmentation Fault**: This is a common error that can be caused by a variety of issues, including incorrect input data or problems with the model configuration. Check your input files carefully and try simplifying your namelist to isolate the problem.
-   **`cfl` errors**: These errors (e.g., `cfl_failure`) indicate that the model has become unstable, often due to a time step that is too long for the given grid resolution. Try reducing the `time_step` in your `namelist.input` file.
-   **Input Data Errors**: Errors like `ERROR: CHEM_INIT: Chemistry routines require USGS or MODIS_NOAH land use maps` indicate a problem with your input data. Ensure your land use data is compatible with the chosen chemistry options.

By following these steps, you can successfully run a WRF-Chem simulation and start analyzing the complex interactions between weather and chemistry in the atmosphere.
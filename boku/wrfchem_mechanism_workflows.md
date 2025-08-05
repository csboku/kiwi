# WRF-Chem Mechanism Workflows: A Technical Guide

This guide provides a detailed, technical walkthrough of the intermediate steps required to run WRF-Chem with different chemical mechanisms. It is intended for users who are already familiar with the basic WRF-Chem workflow.

## 1. Anthropogenic Emissions with `prep_chem_sources`

Most WRF-Chem simulations require anthropogenic emissions. The `prep_chem_sources` tool is a powerful pre-processor for converting global emissions inventories into the format required by WRF-Chem. This section provides a detailed workflow for using `prep_chem_sources` with the EDGAR-HTAP inventory.

### Step 1: Download `prep_chem_sources` and Emissions Data

-   **`prep_chem_sources`**: This tool is available from the [Community Emissions Data System (CEDS)](https://github.com/JGCRI/CEDS) website.
-   **EDGAR-HTAP Data**: Download the sectoral gridmaps from the [EDGAR website](https://edgar.jrc.ec.europa.eu/).

### Step 2: Configure `prep_chem_sources.inp`

This namelist file controls the behavior of `prep_chem_sources`. You must edit it to match your WRF domain and emissions data.

```fortran
&RP_INPUT
! --- Domain and Date ---
 grid_type= 'lambert',
 iyear=2010,
 imon=01,
 iday=01,
 ihour=00,

! --- Emissions Data ---
 use_edgar = 3, ! 3 = HTAP
 edgar_data_dir ='/path/to/your/EMISSION_DATA/EDGAR/htap',

! --- WPS Namelist Information (must match your namelist.wps) ---
 NINEST = 1,
 NJNEST = 1,
 NKNest = 30,
 DELX = 36000.,
 DELY = 36000.,
 POLELAT = 90.0,
 POLELON = 0.0,
 STDLAT1 = 30.0,
 STDLAT2 = 60.0,
 CENTLAT = 40.0,
 CENTLON = -97.0,
/
```

### Step 3: Run `prep_chem_sources`

Execute the `prep_chem_sources.exe` program. This will generate binary output files (e.g., `EDGAR-HTAP-anthro.bin`).

### Step 4: Convert to WRF-Chem Format with `convert_emiss`

The binary output from `prep_chem_sources` must be converted to NetCDF.

1.  **Link the binary files**: In your WRF run directory, link the binary files to the names expected by `convert_emiss`.
2.  **Configure `namelist.input`**: Ensure your main `namelist.input` has the correct `emiss_opt` for your chosen mechanism.
3.  **Run `convert_emiss.exe`**: This will generate the `wrfchemi_` files that WRF-Chem can read.

## 2. MOZART Boundary Conditions with `mozbc`

When running a regional WRF-Chem simulation with the MOZART chemical mechanism, it is crucial to provide chemical boundary conditions that are consistent with the mechanism. The `mozbc` tool is used for this purpose.

### Workflow

1.  **Download `mozbc`**: The `mozbc` tool is available from the [NCAR ACOM website](https://www2.acom.ucar.edu/gcm/mozart).
2.  **Download Global Model Output**: You will need to download output from a global model simulation that used the MOZART mechanism (e.g., from a CAM-chem or WACCM simulation). These files are often available from the [NCAR Research Data Archive](https://rda.ucar.edu/).
3.  **Configure `mozbc.inp`**: You will need to edit the `mozbc.inp` file to specify the path to your global model output and other options.
4.  **Run `mozbc`**: Execute the `mozbc.exe` program. This will generate the `wrfbdy_d01_chem` file, which contains the chemical boundary conditions for your WRF-Chem simulation.
5.  **Namelist Settings**: In your `namelist.input` file, you must set `have_bcs_chem = .true.` to tell WRF-Chem to use the chemical boundary conditions.

## 3. KPP-based Mechanisms

When using a chemical mechanism that is implemented with the Kinetic Pre-Processor (KPP), there are a few additional considerations.

### `chem_opt` and the `Registry`

-   Each KPP-based mechanism has its own `chem_opt` number.
-   The species and reactions for the mechanism are defined in the corresponding `.kpp`, `.spc`, and `.eqn` files in the `chem/KPP/mechanisms` directory.
-   The species must also be defined in the `Registry/Registry.chem` file.

### Modifying a KPP Mechanism

If you want to modify a KPP-based mechanism (e.g., to add a new species or reaction), you will need to:

1.  **Modify the `.kpp`, `.spc`, and `.eqn` files**.
2.  **Modify the `Registry/Registry.chem` file** to add any new species.
3.  **Recompile WRF-Chem**. The build system will automatically re-run KPP to generate the new chemical solver.

This guide provides a high-level overview of the workflows for these common intermediate steps. For more detailed information, always refer to the user guides for the specific tools (`prep_chem_sources`, `mozbc`) and the WRF-Chem User's Guide.
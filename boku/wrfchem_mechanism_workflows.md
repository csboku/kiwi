# WRF-Chem Mechanism Workflows: A Technical Guide

This guide provides a detailed, technical walkthrough of the intermediate steps required to run WRF-Chem with different chemical mechanisms. It is intended for users who are already familiar with the basic WRF-Chem workflow.

## 1. Anthropogenic Emissions with `prep_chem_sources`

Most WRF-Chem simulations require anthropogenic emissions. The `prep_chem_sources` tool is a powerful pre-processor for converting global emissions inventories into the format required by WRF-Chem.

### Workflow

1.  **Download `prep_chem_sources`**: This tool is available from the Community Emissions Data System (CEDS) website.
2.  **Download Emissions Data**: You will need to download the emissions inventory you want to use (e.g., EDGAR, HTAP).
3.  **Configure `prep_chem_sources`**: You will need to edit the `prep_chem_sources.inp` file to specify the path to your emissions data, the chemical mechanism you are using, and other options.
4.  **Run `prep_chem_sources`**: Execute the `prep_chem_sources.exe` program. This will generate a set of intermediate files.
5.  **Convert to WRF-Chem Format**: Use the provided conversion scripts to convert the intermediate files into the `wrfchemi_` files that WRF-Chem can read.
6.  **Namelist Settings**: In your `namelist.input` file, you will need to set `emiss_opt` to use the appropriate emissions files.

## 2. MOZART Boundary Conditions with `mozbc`

When running a regional WRF-Chem simulation with the MOZART chemical mechanism, it is crucial to provide chemical boundary conditions that are consistent with the mechanism. The `mozbc` tool is used for this purpose.

### Workflow

1.  **Download `mozbc`**: The `mozbc` tool is available from the NCAR ACOM website.
2.  **Download Global Model Output**: You will need to download output from a global model simulation that used the MOZART mechanism (e.g., from a CAM-chem or WACCM simulation).
3.  **Configure `mozbc`**: You will need to edit the `mozbc.inp` file to specify the path to your global model output and other options.
4.  **Run `mozbc`**: Execute the `mozbc.exe` program. This will generate the `wrfbdy_d01_chem` file, which contains the chemical boundary conditions for your WRF-Chem simulation.
5.  **Namelist Settings**: In your `namelist.input` file, you will need to set `have_bcs_chem = .true.` to tell WRF-Chem to use the chemical boundary conditions.

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

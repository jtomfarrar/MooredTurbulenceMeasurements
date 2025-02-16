# Source code for
Zippel, S. F., Farrar, J. T., Zappa, C. J., Miller, U., St. Laurent, L., Ijichi, T., Weller, R. A., McRaven, L. T., Nylund, S., Le Bel, D. Moored Turbulence Measurements using Pulse-Coherent Doppler Sonar. J Atmos Ocean Technol. doi:https://doi.org/10.1175/JTECH-D-21-0005.1

# Abstract
Upper-ocean turbulence is central to the exchanges of heat, momentum, and gasses across the air/sea interface, and therefore plays a large role in weather and climate. Current understanding of upper-ocean mixing is lacking, often leading  models to misrepresent mixed-layer depths and sea surface temperature. In part, progress has been limited due to the difficulty of measuring turbulence from fixed moorings which can simultaneously measure surface fluxes and upper-ocean stratification over long time periods. The goal of this paper is to introduce a new method for measuring Turbulent Kinetic Energy (TKE) dissipation rates, epsilon, from long-enduring moorings. We present a direct wavenumber method for estimating dissipation rates using pulse-coherent ADCPs. We discuss optimal programming of the ADCPs, a robust mechanical design for use on a mooring to maximize data return, and data processing techniques including phase-ambiguity unwrapping, spectral analysis, and a correction for instrument response. We find the mooring-derived TKE dissipation rates compare favorably to estimates made nearby from a microstructure shear probe mounted to a glider during its two separate two-week missions for O(10^-8) < epsilon < O(10^-5) m^2 s^-3. Periods of disagreement between turbulence estimates from the two platforms coincide with differences in vertical temperature profiles, which may indicate that barrier layers can substantially modulate upper-ocean turbulence over horizontal scales of 1-10 km. We also find that dissipation estimates from two different moorings at 12.5 m, and  at  7 m are in agreement with the surface buoyancy flux during periods of strong nighttime convection, consistent with classic boundary layer theory.

# Authors
- Seth F. Zippel
- J. Thomas Farrar
- Christopher J. Zappa
- Una Miller
- Louis St. Laurent
- Takashi Ijichi
- Robert A. Weller
- Leah McRaven
- Sven Nylund
- Deborah Le Bel

# Data
Data from the SPURS moorings including the processed dissipation rates are available through NASA's PO.DAAC https://podaac.jpl.nasa.gov/
- DOI:10.5067/SPUR1-MOOR1 for SPURS-1
- DOI:10.5067/SPUR2-MOOR1 for SPURS-2

# Funding
This work was funded by NASA as part of the Salinity Processes in the Upper Ocean Regional Study (SPURS), supporting field work for SPURS-1 (NASA grant number NNX11AE84G), for SPURS-2 (NASA grant number NNX15AG20G), and for analysis (NASA grant number 80NSSC18K1494). Funding for early iterations of this project associated with the VOCALS project and Stratus 9 mooring was provided by NSF (Award numbers 0745508 and 0745442). The Stratus Ocean Reference Station is funded by the Global Ocean Monitoring and Observing Program of the National Oceanic and Atmospheric Administration (CPO FundRef number 100007298), through the Cooperative Institute for the North Atlantic Region (CINAR) under Cooperative Agreement NA14OAR4320158. Microstructure measurements made from the glider were supported by NSF (Award number 1129646).

# Dependencies, and directory structure
Code was written with MATLAB R2020a with the following toolboxes:
Signal Processing Toolbox, Statistics and Machine Learning Toolbox, Parallel Computing Toolbox. Code to create figures uses "export_fig" which is available on the Matlab file exchange (https://www.mathworks.com/matlabcentral/fileexchange/23629-export_fig)

The code has an assumed directory structure for processing Nortek Aquadopp files with standard extensions (.hdr, .v1, .a1, .c1, and .sen).
```bash
MooredTurbulenceMeasurements/
+-- src/
|   +-- processing/
|   +-- figure_preparation/
+-- data/
|   +-- [PROJ 1]
|   |    +-- raw/
|   |    |    +-- NortekFiles/
|   |    |        +-- [Nortek instrument id]/
|   |    |            +-- [Nortek instrument id].hdr
|   |    |            +-- [Nortek instrument id].v1
|   |    |            +-- [Nortek instrument id].a1
|   |    |            +-- [Nortek instrument id].c1
|   |    |            +-- [Nortek instrument id].sen
|   |    +-- interim/
|   |    +-- processed/
|   +-- [PROJ 2]
```
# How to use (processing)

Run the main processing script, "NortekHR_processing.m" to process raw aquadopp files in the directory structure (as shown above). If applying to a new data set, the user will have to rename the project folders and the instrument IDs in the directory (as seen above), and in the script "NortekHR_processing.m" (lines 21-38). 

This script is mainly used for data wrangling specific to the SPRUS project. If the user wishes to write their own data wrangling script, they can use the QC and processing functions provided here to follow the same processing method, mainly:
- Unwrap data using: "histogram_unwrap_function5.m" (needs chunk2.m)
- QC unwrapped data: "QC_nortek_velocities.m"
- Estimate TKE dissipation rate: "wavenumber_dissipation.m"

Other functions are mainly to assist with data wrangling in the main processing script "NortekHR_processing.m", including
- converting raw ascii files to .mat files for each burst "NorrtekHRAsciii2burstMat.m"
- grabbing instrument configuration in "readNortekHeader.m"
- function to help save individual files in parfor loop "parsave.m"
- concatenate individual burst and instrument ID results into single structure "grid_dissipation_data.m"

# How to use (to reproduce figures)

Make sure data files have been downloaded, including SPURS mooring TS and met files, either from PODACC or the WHOI UOP website. If you are reprocessing the dissipation data, run the processing section described above. 

Code to generate Figures 2-13, and 15 is available in blocked sections in "PrintFigures2.m". Figure 1 was drafted in Inkscape and not generated from code. Figure 14 was created in python, and is available at a separate repository here: https://github.com/jtomfarrar/SPURS1_SST_map

Specific requirements for code blocks in "PrintFigures2.m":
- Figures 5-7: requires burst level data, which can accessed by running the processing script on the raw data (as described above) or by contacting the author
- Figure 8-11: requires processed dissipation data (as described above), data from the glider deployments, and SPURS TS data (available on PODAAC, or on UOP website)
- Figure 12-13: run "BuoyancyCalcs.m" first, which makes local estimates of mixed layer depth and ocean surface buoyancy fluxes.

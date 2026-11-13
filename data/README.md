# Analog Cadence Design Library

[Repository overview](../README.md) · [Download ANALOG_DESIGN.zip](ANALOG_DESIGN.zip)

The archive contains the `ANALOG_DESIGN` library: **34 cell directories**, OpenAccess schematic/symbol data, and saved Spectre ADE states. It includes the analog circuit blocks and testbenches supporting the OP-AMP and comparator studies.

## Functional Organization

| Group | Cells and Purpose |
| --- | --- |
| Device characterization | `nmos_IV_curve`, `nmos_Vth_curve`, `pmos_IV_curve`, `pmos_Vth_curve` |
| Amplifier building blocks | `CS_amp`, `CG_amp`, `CD_amp`, `Diffamp` |
| Bias | `90nm_bias`, `90nm_bias_NMOS_input_pair`, `90nm_bias_NMOS_input_pair2` |
| OP-AMP circuits | `2stage_OP_Amp` and `2stage_OP_Amp_90nm_nmos` families |
| OP-AMP analysis | AC, DC, offset, pulse, and inverting/non-inverting testbench cells |
| Comparator circuits | `2stage_comparator`, `2stage_comparator_PMOS_input_pair` |
| Comparator analysis | `2stage_comparator_sim`, `2stage_comparator_sim_DC`, and PMOS-input simulation cell |

## Cell and View Index

| Cell | Schematic | Symbol | Saved Spectre State Directories |
| --- | :---: | :---: | ---: |
| `2stage_comparator` | Yes | Yes | 0 |
| `2stage_comparator_PMOS_input_pair` | Yes | - | 0 |
| `2stage_comparator_PMOS_input_pair_sim` | Yes | - | 0 |
| `2stage_comparator_sim` | Yes | - | 1 |
| `2stage_comparator_sim_DC` | Yes | - | 1 |
| `2stage_OP_Amp` | Yes | Yes | 0 |
| `2stage_OP_Amp_90nm_nmos` | Yes | Yes | 0 |
| `2stage_OP_Amp_90nm_nmos_copy` | Yes | Yes | 0 |
| `2stage_OP_Amp_90nm_nmos_copy2` | Yes | Yes | 0 |
| `2stage_OP_Amp_90nm_nmos_copy_sim` | Yes | - | 4 |
| `2stage_OP_Amp_90nm_nmos_copy_sim2` | Yes | - | 4 |
| `2stage_OP_Amp_90nm_nmos_copy_sim_DC_offset` | Yes | - | 1 |
| `2stage_OP_Amp_90nm_nmos_copy_sim_DC_offset2` | Yes | - | 4 |
| `2stage_OP_Amp_90nm_nmos_sim` | Yes | - | 5 |
| `2stage_OP_Amp_90nm_nmos_sim_DC_offset` | Yes | - | 4 |
| `2stage_OP_Amp_copy_sim_inverting_noninverting` | Yes | - | 1 |
| `2stage_OP_Amp_copy_sim_inverting_noninverting2` | Yes | - | 4 |
| `2stage_OP_Amp_sim_AC` | Yes | - | 1 |
| `2stage_OP_Amp_sim_DC` | Yes | - | 1 |
| `2stage_OP_Amp_sim_DC_offset` | Yes | - | 1 |
| `2stage_OP_Amp_sim_inverting_noninverting` | Yes | - | 1 |
| `2stage_OP_Amp_sim_pulse` | Yes | - | 2 |
| `2stage_OP_Amp_sim_pulse2` | Yes | - | 1 |
| `90nm_bias` | Yes | - | 0 |
| `90nm_bias_NMOS_input_pair` | Yes | Yes | 0 |
| `90nm_bias_NMOS_input_pair2` | Yes | Yes | 0 |
| `CD_amp` | Yes | - | 1 |
| `CG_amp` | Yes | - | 1 |
| `CS_amp` | Yes | - | 1 |
| `Diffamp` | Yes | - | 1 |
| `nmos_IV_curve` | Yes | - | 1 |
| `nmos_Vth_curve` | Yes | - | 1 |
| `pmos_IV_curve` | Yes | - | 1 |
| `pmos_Vth_curve` | Yes | - | 1 |

The names above are retained exactly as stored in the archive, including numbered copies. Grouping is for navigation; the index does not rename cells or alter their references.

## Saved Simulation Configuration

The `spectre_state*` folders store ADE settings such as model selection, design variables, enabled analyses, output expressions, and waveform configuration. Several OP-AMP testbench cells include separate FF, NN, and SS state directories.

| File | Purpose |
| --- | --- |
| `analyses.state` | AC, DC, and transient analysis settings |
| `modelSetup.state` | Model-library and process-section selection |
| `variables.state` | Design-variable settings |
| `outputs.state` | Selected measurements and expressions |
| `schematic/sch.oa` | OpenAccess schematic data |
| `symbol/symbol.oa` | Reusable schematic symbol |
| `cdsinfo.tag`, `.oalib`, `data.dm` | Library/design metadata |

## Working with the Library

1. Extract the archive and register the `ANALOG_DESIGN` library in a Cadence Virtuoso environment.
2. Attach the corresponding GPDK technology and resolve the referenced device libraries.
3. Open the required circuit or testbench schematic.
4. Load its saved Spectre ADE state and map model paths to the local PDK installation.
5. Select the process section, temperature, supply, and stimulus matching the documented experiment.

The project presentations provide the complete four-case analog comparison; this index describes the cells actually packaged in the archive.

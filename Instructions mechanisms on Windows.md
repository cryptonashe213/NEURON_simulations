## Compiling the NEURON mechanisms on Windows

The `.mod` files define the custom membrane mechanisms used by the simulations. Compile the appropriate set before opening a simulation.

Use the `mknrndll` utility included with the NEURON Windows installation. Select the directory containing the required `.mod` files, then click **Make nrnmech.dll**. Check that compilation finishes successfully.

### Point neurons, BallStick4, Tree and Tree6

These models use the mechanisms stored in the repository root.

1. Download and extract the repository.
2. Open `mknrndll`.
3. Select the repository root directory, containing `nax.mod` and `kdrca1.mod`.
4. Compile the `.mod` files located directly in that directory.
5. Confirm that `nrnmech.dll` has been generated there.
6. Start a fresh NEURON session with the repository root as its working directory, then open the required `.hoc` file.

The same compiled mechanisms support:

- Single-compartment models: `2cells_sinc_IClamp_clean.hoc` and `2cells_sinc_NetStim_clean.hoc`.
- BallStick4 models, including somatic and dendritic stimulation.
- The original Tree model.
- All Tree6 distance sweeps and the Tree6 frequency sweep.

Recompilation is not required when changing stimulation location, inhibitory weight or synaptic distance. It is required after modifying the `.mod` files or changing to an incompatible NEURON installation.

### Reconstructed CA1 model

CA1 uses a separate mechanism collection.

1. Open `mknrndll`.
2. Select:
   `mechanisms/hippocampus/mod/`
3. Compile the `.mod` files directly inside this directory. Do not combine them with files from the repository root or the `optimized` subdirectories.
4. Confirm that the resulting library is located at:
   `mechanisms/hippocampus/mod/nrnmech.dll`

The two CA1 scripts explicitly load this library:

- `2cells_sinc_IClamp_CA1.hoc`
- `2cells_sinc_NetStim_CA1_DEND.hoc`

Keep the `electrophysiology` and `morphology` directories in the repository root and run these scripts with that directory as the working directory.

**Avoid loading both mechanism libraries in the same session.** They contain overlapping mechanism names. If you previously compiled the simplified models, use a separate repository copy for CA1, without a compiled library in its root. Start a fresh NEURON session before running CA1.

### Troubleshooting

- **Unknown mechanism (`nax`, `kdr` or `na3`):** the required library was not compiled or loaded.
- **Library not found:** check the compilation output and the path used by `nrn_load_dll`.
- **Mechanism already exists:** two libraries containing the same mechanism were loaded. Restart NEURON and load only the intended library.
- **Compilation fails:** retain the compiler error message and check compatibility with your NEURON version. Legacy MOD files may require changes for NEURON 9 or later.

Generated `.c`, `.cpp`, `.o` and library files do not need to be uploaded to reproduce the source code. Users should compile the provided `.mod` files locally.

These instructions describe the repository’s Windows workflow. Linux and macOS use `nrnivmodl`; the explicit Windows library paths in the CA1 scripts must also be adapted.

References: [NEURON: Using NMODL files](https://www.neuronsimulator.org/en/latest/courses/using_nmodl_files.html) and [NEURON documentation and version compatibility](https://nrn.readthedocs.io/en/latest/index.html).

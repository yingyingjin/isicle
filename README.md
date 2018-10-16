ISiCLE
======

Overview
--------
ISiCLE, or the _in silico_ chemical library engine, is a pipeline for high-accuracy chemical property calculation. ISiCLE takes an [InChI](https://en.wikipedia.org/wiki/International_Chemical_Identifier) (international chemical identifier) string as input, generates an initial 3D conformation, and subsequently optimizes this initial structure through molecular dynamics simulations and quantum chemistry optimizations. Finally, ISiCLE simulates desired properties (e.g. collision cross section, CCS) for each conformer yielded during molecular dynamics simulations to produce a single value, Boltzmann-weighted by Gibb's free energy, giving emphasis to properties from highly probable conformations.

ISiCLE is implemented using the [Snakemake](https://snakemake.readthedocs.io) workflow management system, enabling scalability, portability, provenance, fault tolerance, and automatic job restarting. Snakemake provides a readable Python-based workflow definition language and execution environment that scales, without modification, from single-core workstations to compute clusters through as-available job queuing based on a task dependency graph.

<p align="center">
  <img align="center" src="resources/schematic.svg" width="40%" height="40%">
</p>

Installation
------------
Simply clone ISiCLE to your workstation or cluster, ensuring the following packages are installed:
* [snakemake](https://snakemake.readthedocs.io)
* [openbabel](http://openbabel.org/wiki/Main_Page), [pybel](https://openbabel.org/docs/dev/UseTheLibrary/Python_Pybel.html)
* [rdkit](https://www.rdkit.org/)
* [numpy](http://www.numpy.org/)
* [pandas](https://pandas.pydata.org/)
* [yaml](https://pyyaml.org/wiki/LibYAML)
* [statsmodels](https://www.statsmodels.org)
* [ambertools](http://ambermd.org/)

If using [``conda``](https://www.anaconda.com/download/), this can be achieved by creating a new virtual environment:
```bash
conda create -n isicle -c bioconda -c openbabel -c rdkit -c ambermd python=3.6.1 openbabel rdkit ambertools snakemake numpy pandas yaml pathlib statsmodels
```

Additionally, ensure the following third-party software is installed and added to your ``PATH``:
* [cxcalc](https://chemaxon.com/marvin-archive/5_2_0/marvin/help/applications/calc.html)
* [NWChem](http://www.nwchem-sw.org/index.php/Download)

Getting Started
---------------
First, if using ``conda``, activate the virtual environment by executing:
```bash
source activate isicle
```

Install ISiCLE using [``pip``](https://pypi.org/project/pip/):
```bash
# local
pip install /path/to/isicle/

# git
pip install git+https://github.com/pnnl/isicle
```

ISiCLE assumes a user starts with a text file with a SMILES or InChI string on each line. Use ``isicle-input`` to prepare inputs for Snakemake, specifying an ISiCLE config file and operation mode (SMILES versus InChI). This ensures each input has a unique InChI key identifier. Detailed instructions can be accessed through the help flag (``--help`` or ``-h``).
```bash
# SMILES input
isicle-input /path/to/smi_list.txt --config /path/to/isicle_config.yaml --smi

# InChI input
isicle-input /path/to/inchi_list.txt --config /path/to/isicle_config.yaml --inchi
```

To begin simulations, simply execute ``isicle`` with desired configuration flags (``isicle --help`` or ``-h`` for help). Default [workflow](resources/example_config.yaml) and [cluster](resources/example_cluster.yaml) configurations are provided, but these are intended to be modified and supplied by the user to accomodate workflow-specific needs. 

An example dryrun for desktop use (CCS module, Lite mode):
```bash
isicle --config /path/to/config.yaml --cores 4 --ccs --lite --dryrun
```

An example dryrun for ``slurm`` cluster environments (CCS module, Standard mode):
```bash
isicle --config /path/to/config.yaml --cluster-config /path/to/cluster.yaml --jobs 999 --ccs --standard --dryrun
```

Citing ISiCLE
-------------
If you would like to reference ISiCLE in an academic paper, we ask you include the following references:

* Colby, S.M., Thomas, D.G., Nunez, J.R., Baxter, D.J., Glaesemann, K.R., Brown, J.M., Pirrung, M.A., Govind, N., Teeguarden, J.G., Metz, T.O., and Renslow, R.S., 2018. ISiCLE: A molecular collision cross section calculation pipeline for establishing large in silico reference libraries for compound identification. arXiv preprint arXiv:1809.08378.
* Yesiltepe, Y., Nunez, J.R., Colby, S.M., Thomas, D.G., Borkum, M.I., Reardon, P.N., Washton, N.M., Metz, T.O., Teeguarden, J.T., Govind, N., and Renslow, R.S., 2018. "An automated framework for NMR chemical shift calculations of small organic molecules." Journal of Cheminformatics. In press.
* ISiCLE, version 0.1.0 http://github.com/pnnl/isicle (accessed Oct 2018)

The first is a [preprint paper](https://arxiv.org/abs/1809.08378) describing ISiCLE for CCS, the second describes ISiCLE for NMR chemical shifts, and the third is to cite the software package (update version and access date appropriately).

Disclaimer
----------
This material was prepared as an account of work sponsored by an agency of the United States Government. Neither the United States Government nor the United States Department of Energy, nor Battelle, nor any of their employees, nor any jurisdiction or organization that has cooperated in the development of these materials, makes any warranty, express or implied, or assumes any legal liability or responsibility for the accuracy, completeness, or usefulness or any information, apparatus, product, software, or process disclosed, or represents that its use would not infringe privately owned rights.

Reference herein to any specific commercial product, process, or service by trade name, trademark, manufacturer, or otherwise does not necessarily constitute or imply its endorsement, recommendation, or favoring by the United States Government or any agency thereof, or Battelle Memorial Institute. The views and opinions of authors expressed herein do not necessarily state or reflect those of the United States Government or any agency thereof.

PACIFIC NORTHWEST NATIONAL LABORATORY operated by BATTELLE for the UNITED STATES DEPARTMENT OF ENERGY under Contract DE-AC05-76RL01830

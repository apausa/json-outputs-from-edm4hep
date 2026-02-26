# json-outputs-from-edm4hep

JSON outputs from all EDM4HEP releases, generated from (FCC Tutorials)[https://hep-fcc.github.io/fcc-tutorials/main/fast-sim-and-analysis/k4simdelphes/doc/starterkit/FccFastSimDelphes/Readme.html]:

```bash
# e+ e- -> ZH -> Z and H to anything
# e+ e- -> ZZ -> Z to anything
# e+ e- -> WW -> W to anything
```

1. Setup a FCC software stack release

```bash
# source /cvmfs/sw.hsf.org/key4hep/setup.sh -r 2023-11-23
# source /cvmfs/sw.hsf.org/key4hep/setup.sh -r 2024-03-10
# source /cvmfs/sw.hsf.org/key4hep/setup.sh -r 2024-04-12
# source /cvmfs/sw.hsf.org/key4hep/setup.sh -r 2024-10-03
# source /cvmfs/sw.hsf.org/key4hep/setup.sh -r 2024-10-28
# source /cvmfs/sw.hsf.org/key4hep/setup.sh -r 2024-11-28
# source /cvmfs/sw.hsf.org/key4hep/setup.sh -r 2025-01-28
# source /cvmfs/sw.hsf.org/key4hep/setup.sh -r 2025-05-29
source /cvmfs/sw.hsf.org/key4hep/setup.sh -r 2026-02-01
```

2. Download official pythia cards for the various processes

```bash
wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/winter2023/FCCee/Generator/Pythia8/p8_ee_ZH_ecm240.cmd
wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/winter2023/FCCee/Generator/Pythia8/p8_ee_ZZ_ecm240.cmd
wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/winter2023/FCCee/Generator/Pythia8/p8_ee_WW_ecm240.cmd

wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/winter2023/FCCee/Delphes/card_IDEA.tcl
wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/winter2023/FCCee/Delphes/edm4hep_IDEA.tcl
```

3. Simulate events with DelphesEDM4Hep

```bash
DelphesPythia8_EDM4HEP card_IDEA.tcl edm4hep_IDEA.tcl p8_ee_ZH_ecm240.cmd p8_ee_ZH_ecm240_edm4hep.root
DelphesPythia8_EDM4HEP card_IDEA.tcl edm4hep_IDEA.tcl p8_ee_ZZ_ecm240.cmd p8_ee_ZZ_ecm240_edm4hep.root
DelphesPythia8_EDM4HEP card_IDEA.tcl edm4hep_IDEA.tcl p8_ee_WW_ecm240.cmd p8_ee_WW_ecm240_edm4hep.root
```

4. Convert the first 20 events from root files to json

```bash
edm4hep2json -n 20 p8_ee_ZH_ecm240_edm4hep.root
edm4hep2json -n 20 p8_ee_ZZ_ecm240_edm4hep.root
edm4hep2json -n 20 p8_ee_WW_ecm240_edm4hep.root
```

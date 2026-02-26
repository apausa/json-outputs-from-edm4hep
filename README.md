# json-outputs-from-edm4hep

JSON outputs from all EDM4HEP releases, generated from (FCC Tutorials)[https://hep-fcc.github.io/fcc-tutorials/main/fast-sim-and-analysis/k4simdelphes/doc/starterkit/FccFastSimDelphes/Readme.html]:

## Table of EDM4HEP releases and their respective schema

| Release        | edm4hepVersion | schemaVersion |
| -------------- | -------------- | ------------- |
| **2023-11-23** | 0.10.2         | 1             |
| **2024-03-10** | 0.10.5         | 1             |
| **2024-04-12** | 0.10.5         | 1             |
| **2024-10-03** | 0.99.1         | 2             |
| **2024-10-28** | 0.99.1         | 2             |
| **2024-11-28** | 0.99.1         | 2             |
| **2025-01-28** | 0.99.1         | 2             |
| **2025-05-29** | 0.99.2         | 3             |
| **2026-02-01** | 1.0.0          | 6             |

## Table of EDM4HEP versions and their respective schema

| edm4hepVersion | schemaVersion |
| -------------- | ------------- |
| 0.9.0          | 1             |
| 0.10.0         | 1             |
| 0.10.1         | 1             |
| 0.10.2         | 1             |
| 0.10.3         | 1             |
| 0.10.4         | 1             |
| 0.10.5         | 1             |
| 0.10.99        | 1             |
| 0.99.0         | 2             |
| 0.99.1         | 2             |
| 0.99.2         | 3             |
| ?              | 4             |
| ?              | 5             |
| 1.0.0          | 6             |

## File generation

Setup a FCC software stack release

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

### 2. Event simulation

From (FCC: Getting started with simulating events in Delphes)[https://hep-fcc.github.io/fcc-tutorials/main/fast-sim-and-analysis/k4simdelphes/doc/starterkit/FccFastSimDelphes/Readme.html].

```bash
# e+ e- -> ZH -> Z and H to anything
# e+ e- -> ZZ -> Z to anything
# e+ e- -> WW -> W to anything
```

2.1 Download official pythia cards for the various processes

```bash
wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/winter2023/FCCee/Generator/Pythia8/p8_ee_ZH_ecm240.cmd
wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/winter2023/FCCee/Generator/Pythia8/p8_ee_ZZ_ecm240.cmd
wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/winter2023/FCCee/Generator/Pythia8/p8_ee_WW_ecm240.cmd

wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/winter2023/FCCee/Delphes/card_IDEA.tcl
wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/winter2023/FCCee/Delphes/edm4hep_IDEA.tcl
```

2.2. Simulate events with DelphesEDM4Hep

```bash
DelphesPythia8_EDM4HEP card_IDEA.tcl edm4hep_IDEA.tcl p8_ee_ZH_ecm240.cmd p8_ee_ZH_ecm240_edm4hep.root
DelphesPythia8_EDM4HEP card_IDEA.tcl edm4hep_IDEA.tcl p8_ee_ZZ_ecm240.cmd p8_ee_ZZ_ecm240_edm4hep.root
DelphesPythia8_EDM4HEP card_IDEA.tcl edm4hep_IDEA.tcl p8_ee_WW_ecm240.cmd p8_ee_WW_ecm240_edm4hep.root
```

2.3. Convert the first 20 events from root files to json

```bash
edm4hep2json -n 20 p8_ee_ZH_ecm240_edm4hep.root
edm4hep2json -n 20 p8_ee_ZZ_ecm240_edm4hep.root
edm4hep2json -n 20 p8_ee_WW_ecm240_edm4hep.root
```

### Full simulation

From (FCC-ee Full Sim General Overview)[https://hep-fcc.github.io/fcc-tutorials/main/full-detector-simulations/FCCeeGeneralOverview/FCCeeGeneralOverview.html#towards-full-sim-physics-analyses-with-cld].

# e⁺e⁻ → Z(μμ)H(inclusive)

3.1. Clone repo

```bash
git clone https://github.com/HEP-FCC/fcc-tutorials

git clone https://github.com/key4hep/CLDConfig.git
cd CLDConfig/CLDConfig
```

3.2  Retrieve Z(mumu)H(X) MC generator events

```bash
wget https://fccsw.web.cern.ch/fccsw/tutorials/MIT2024/wzp6_ee_mumuH_ecm240_GEN.stdhep.gz
gunzip wzp6_ee_mumuH_ecm240_GEN.stdhep.gz
```

3.3. Run the Geant4 simulation (1 event)

```bash
ddsim -I wzp6_ee_mumuH_ecm240_GEN.stdhep -N 1 -O wzp6_ee_mumuH_ecm240_CLD_SIM.root --compactFile $K4GEO/FCCee/CLD/compact/CLD_o2_v07/CLD_o2_v07.xml --steeringFile cld_steer.py
```

3.4 Running CLD reconstruction (1 event)

```bash
sed -i "s/DEBUG/INFO/" CLDReconstruction.py
k4run CLDReconstruction.py --inputFiles wzp6_ee_mumuH_ecm240_CLD_SIM.root --outputBasename wzp6_ee_mumuH_ecm240_CLD --num-events -1

```

3.5. Convert the first 20 events from root files to json

```bash
edm4hep2json wzp6_ee_mumuH_ecm240_CLD_REC.edm4hep.root
edm4hep2json wzp6_ee_mumuH_ecm240_CLD_SIM.root
```

# Supplementary materials for paper Adapting Multi-objectivization for Configuration Tuning
This repository contains the data and source code for the paper "Adapting Multi-objectivization for Configuration Tuning"




## Data Result

The dataset of this work can be accessed via the Zenodo link [here](https://zenodo.org/record/6522292). The zip file contains all the raw data as reported in the paper; most of the structures are self-explained but we wish to highlight the following:

* The data under the folder `1.0-0.0` and `0.0-1.0` are for the single-objective optimizers, including IRACE, GA, RS, FLASH, and BOCA. The former uses O1 as the target performance objective while the latter uses O2 as the target.

* The data in the folder `none` under the folders named by the subject systems are for PMO.

* The data in the folder `linear` which is under the folders named by the subject systems are for the AdMMO and MMO. In particular:

  * The data under the folder `0.0` are for AdMMO under the budget for measurement-based optimizer (30% proportion).
  * The data under the folder `0.0-flash` are for AdMMO under the budget for model-based optimizer (30% proportion).
  * The data under the folder `1.0` are for MMO.
  * The result under the folder `0.0-x`, where x is a certain number are for AdMMO under x% of the proportion.
  * The data under the folder `0.0-wotrigger` are for the variant of AdMMO that does not use a progressive trigger.
  * The data under the folder `0.0-wodup` are for variant of AdMMO that does not have duplicate handling.

* For those data of AdMMO, MMO, and PMO, the folder `0` and `1` denote using uses O1 and O2 as the target performance objective, respectively.

* In the lowest-level folder where the data is stored (i.e., the `sas` folder), `SolutionSet.rtf` contains the results over all repeated runs; `SolutionSetWithMeasurement.rtf` records the results over different numbers of measurements.

## Souce Code

The [`code`](https://github.com/3c23/admmo/tree/main/code) folder contains all the information about the source code, as well as a library jar file in the [`library`](https://github.com/3c23/admmo/tree/main/library) folder. The core of AdMMO is implemented in the `AdMMONSGAII.java` class, but due to it using some libraries that may violate the double-blind requirement, we temporarily hide some of the code import those packages.

## Running Code

Running the code requires a few steps depending on the RQs. For all those steps, both the jars files in the `library` folders need to be imported and compiled together. There are a few variables need to change in the `AutoRun.java` class (which is the main class to run the experiments):

* The variable `weights` indicates which is the target performance objective for the single objective optimizer, 1.0-0.0 means O1 while 0.0-1.0 means O2.
* The variable `single_algs` controls which single objective optimizer to run: irace, ga, or rs.
* The variable `benchmark` describes which system to run, currently, it can take the following values (where SS-K, Coffee, and SS-N means the system storm, keras, and x264, respectively):
  * mariadb
  * SS-K
  * vp9
  * Coffee
  * mongodb
  * SS-N
  * llvm
* The variable `index` indicates which is the target objective, 0 means O1 while 1 means O2.
* The variable `w_a` controls whether it is AdMMO or MMO: 0.0 is for AdMMO while 1.0 is for MMO.
* The variable `flash` controls whether we run different variants of AdMMO.

The `prefix_path` variable in the `Parser.java` class would also need to be changed to correctly reflect which system to run, using the path to the data source in the [`measured-data`](https://github.com/3c23/admmo/tree/main/measured-data) folder. 

### RQ1

* Running function `so` would produce results for single-objective optimizer.
* Running function `pmo` would produce results for PMO.
* Running function `mo` would produce results for AdMMO or MMO (controlled by the `w_a` variable).

### RQ2

* Changing the measurement budget and population size in EAConfigure to reflect this.
* Running function `flash_so` would produce results for FLASH
* Running function `boca_so` would produce results for BOCA.
* Running function `mo` would produce results for AdMMO.

### RQ3

* Running funtion `mo` would produe results for the variants of AdMMO (controlled by the `flash` variable) and by setting `SASAlgorithmAdaptor.isAdaptConstantly = false` or `SASAlgorithmAdaptor.isToFilterRedundantSolution = false`.

### RQ4

* Running function `mo` would produce results for the variants of AdMMO (controlled by the `flash` variable) and by setting the variable `p` or in the `AdMMONSGAII.java` class.

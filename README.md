# OPS

<p align="left">
  <img src="res/images/OPS-v2.png" width="200"/>
</p>


The Opportunistic Protocol Simulator (OPS, pronounced as oops!!!) is a set of
simulation models for OMNeT++ to simulate opportunistic networks. It has a
modular architecture where different protocols relevant to opportunistic networks
can be developed and plugged in. The details of prerequisites, installing OPS, 
configuring OPS, simulating with OPS and much more are given in the following 
sections.

Here are the most important links to get OPS running quickly.

1. Install and build components required by OPS - [Prerequisites](#prerequisites)

2. Install and compile OPS - [Install and Build OPS](#install-and-build-ops)

3. Run simulations - [Running Simulations](#running-simulations)



## Prerequisites

The models of OPS require OMNeT++, the INET framework, the KeetchiLib and some mobility models
that are not part of the INET framework. Each of the sections below describe how these are
installed and configured.

These prerequisites **MUST** be installed before proceeding to install OPS.

### OMNeT++ (Version 5.6.2)

OPS requires OMNeT++. Install OMNeT++ version 5.6.2 using the following resources.

- [OMNeT++ Installation Guide](https://omnetpp.org/documentation/)
- [OMNeT++ Installation Binaries](https://omnetpp.org/download/)

Once OMNeT++ is installed, run the OMNeT++ IDE. The IDE provides options to install all the 
additional components (OMNeT++ frameworks, libraries, etc.) required by OPS.


### INET Framework (Version 4.1.0)

OPS requires the use of [INET Framework](https://inet.omnetpp.org), version 4.1.0, of OMNeT++ to simulate
mobility. There are a number of ways of installing the INET Framework in the IDE. OPS require the exact 
version 4.1.0. So, do not install the default version prompted by the IDE when run for the first time.

There are many ways of installing INET 4.1.0. We recommend the following way.

1. Run the OMNeT++ IDE

2. Select `File->Import->Git->Projects from Git->Clone URI->` and provide the following parameters and 
install INET
  - URI `https://github.com/inet-framework/inet.git`
  - Branch `v4.1.x`
  - Wizard for project import `Import existing Eclipse projects`

3. Select the installed project in `Project Explorer` pane and set the active configuration to `release` using
`Project->Properties->C/C++ Build->Manage Configurations...`

4. Select the installed project in `Project Explorer` pane and build INET using `Project->Build Project`

**Note:** `Project->Build Project` should result in an error free build.


### KeetchiLib

An external library called `KeetchiLib` is used by the `KKeetchiLayer` model to handle all the functionality
related to the [Organic Data Dissemination](https://www.mdpi.com/1999-5903/11/2/29/htm)
forwarding model. The code for this library is available at Github. 

There are many ways of installing KeetchiLib. We recommend the following way.

1. Run the OMNeT++ IDE

2. Select `File->Import->Git->Projects from Git->Clone URI->` and provide the following parameters 
and install KeetchiLib
  - URI `https://github.com/ComNets-Bremen/KeetchiLib.git`
  - Branch `master`
  - Wizard for project import `Import existing Eclipse projects`

3. Select the installed project in `Project Explorer` pane and set the active configuration to `release` using
`Project->Properties->C/C++ Build->Manage Configurations...`

4. Select the installed project in `Project Explorer` pane and build KeetchiLib using `Project->Build Project`

**Note:** `Project->Build Project` should result in an error free build.


###  SWIM Mobility Model

[Small Worlds in Motion](https://arxiv.org/pdf/0809.2730.pdf) (SWIM) is a mobility model
developed by A. Mei and J. Stefa. The INET code for the SWIM mobility model was developed 
by our research group. SWIM is used by many of the OPS scenarios for mobility. 

There are many ways of installing SWIM. We recommend the following way.

1. Run the OMNeT++ IDE

2. Select `File->Import->Git->Projects from Git->Clone URI->` and provide the following parameters 
and install SWIMMobility
  - URI `https://github.com/ComNets-Bremen/SWIMMobility.git`
  - Branch `master`
  - Wizard for project import `Import as general project`

3. Copy the following 3 files into the INET project folder `src/inet/mobility/single`
  - `SWIMMobility.ned`
  - `SWIMMobility.h`
  - `SWIMMobility.cc`

4. Select the INET project in `Project Explorer` pane and rebuild INET using `Project->Build Project`


## Install and Build OPS

Once the prerequisites are installed and built, to install and build OPS, follow the procedure below.

1. Run the OMNeT++ IDE

2. Select `File->Import->Git->Projects from Git->Clone URI->` and provide the following parameters 
and install OPS
  - URI `https://github.com/ComNets-Bremen/OPS.git`
  - Branch `master`
  - Wizard for project import `Import existing Eclipse projects`

3. Copy the following 2 files, located in `res/inet-models/ExtendedSWIMMobility` into the INET 
project folder `src/inet/mobility/contract`
  - `IReactiveMobility.ned`
  - `IReactiveMobility.h`

4. Copy the following 3 files, located in `res/inet-models/ExtendedSWIMMobility` into the INET 
project folder `src/inet/mobility/single`
  - `ExtendedSWIMMobility.ned`
  - `ExtendedSWIMMobility.h`
  - `ExtendedSWIMMobility.cc`

5. Select the installed project in `Project Explorer` pane and set the active configuration to `release` using
`Project->Properties->C/C++ Build->Manage Configurations...`

6. Select the INET project in `Project Explorer` pane and rebuild INET using `Project->Build Project`

7. Select the OPS project in `Project Explorer` pane and select `File->Properties->Project References`

8. Tick the installed INET and KeetchiLib, and click `Apply and Close`.

9. Select the OPS project again in `Project Explorer` pane and build using `Project->Build Project`

**Note:** `Project->Build Project` should result in an error free build.



## Running Simulations

To run simulations, an OMNeT++ `.ini` file has to be created. A sample `.ini` file with a number of 
different scenarios (configurations) is provided in `simulations` folder. To run a simulation, follow 
the procedure given below.

1. Run the OMNeT++ IDE

2. Select the sample `ops-omnetpp.ini` file from the folder `simulations/`

3. Select `Run->Run As->OMNeT++ Simulation`. After clicking through other popups, the following window should appear. Select the sample configuration using the pull-down. 

<p align="center">
  <img src="res/images/ops-config-select.png" width="300"/>
</p>

4. Based on the selected configuration, the following window should appear.

<p align="center">
  <img src="res/images/ops-simulator-window.png" width="700"/>
</p>

5. Start simulating the network by clicking on the simulation controls shown below.

<p align="center">
  <img src="res/images/simulator-controls.png" width="300"/>
</p>


As indicated, there are many sample configurations in `ops-omnetpp.ini` file. Details of these
configurations are given in the [SAMPLEINI File](./res/info/SAMPLEINI.md).


## Creating Graphs of Results

Based on the standard configuration, the raw results (vector and scalar) collected after a simulation run 
are located in the `simulation/results` folder. There are 2 ways of creating charts and tables of results. 
Using the OMNeT++ IDE or using external tools/scripts. Here are examples of the 2 ways.

### Using the OMNeT++ IDE

Below is a brief (high-level) procedure to create your charts using the OMNeT++ IDE. 

1. Run the OMNeT++ IDE

2. In the `simulation/results` folder, create an `Analysis File` by selecting `New -> Analysis File (anf)`

3. Add the created results files (`*.vec` or `*.sca`) to the created file

4. Create datasets with the data you want plotted in every graph

5. Plot them using an appropriate chart type (Line, Bar, etc.)

For more information, check Chapter 10 of the [IDE User Guide](https://www.omnetpp.org/doc/omnetpp/UserGuide.pdf)

An example results chart generated using 2 simulations (Epidemic and RRS) is shown below.

<p align="center">
  <img src="res/images/delv-ratio-epi-rrs.png" width="700"/>
</p>



### Using Python and Matplotlib

Another way is to create tables and graphs using the [Python](https://www.python.org) scripting 
laguage and the [Matplotlib](https://matplotlib.org) library of Python. Have a look and adapt 
the following files (in `res/python-results` folder) to plot graphs and dump a table of the results.

  1. [`plot-sca-results.py`](./res/python-results/plot-sca-results.py) - Plot a bar chart for each of the selected scalar results
  2. [`show-sca-results.py`](./res/python-results/show-sca-results.py) - Show a single table of all the selected scalar results
  3. [`plot-vec-results.py`](./res/python-results/plot-vec-results.py) - Plot a line chart of the selected vector results
  4. [`datalist.csv`](./res/python-results/datalist.csv) - List of all the paths to the `.vec` and `.sca` files of simulations (given as a parameter to `1.`, `2.` and `3.`)
  5. [`statlist.csv`](./res/python-results/statlist.csv) - List of the results to plot by the above scripts (given as a parameter to `1.`, `2.` and `3.`)



## Available Statistics

There are a set of network-level as well as node-level statistics collected by OPS in every 
simulation run. Check the [STATS file](./res/info/STATS.md) to know about all the available statistics.



## Creating New Scenarios

To run simulations for different scenarios, duplicate an existing `.ini` file and modify the model
parameters as required. To know about all the model parameters, check the `.ned` files in the 
`src/` folder.


## Node and Network Architectures

OPS has a **network model**, a **node model** and a number of **protocol layer models** to simulate 
opportunistic networks. A description of all the models is given in the 
[MODELINFO File](./res/info/MODELINFO.md).



## Model Parameters

All parameters are configurable using the `.ini` file used to run simulations. Some of these parameters
have default values but all can be reconfigured in the `.ini` file. Here are some examples.

### Network-level Parameters (e.g., in `OPSMessengerNetwork.ned`)

1. `numNodes` - The total number of nodes in the network
2. `num-rngs` - Number of random number generators

### Application Layer Parameters (e.g., in 'KHeraldApp.ned`)

1. `dataGenerationInterval` - How often are data messages injected into the network
2. `dataSizeInBytes` - Payload size of a data message

### Forwarding Layer Parameters (e.g., in `KEpidemicRoutingLayer.ned`)

1. `maximumCacheSize` - The size of the cache maintained by each node in bytes
2. `maximumHopCount` - The maximum hops that a data packet is allowed to travel (be forwarded) before being discarded


### Link Layer Parameters (e.g., in `KWirelessLayer.ned`)

1. `wirelessRange` - The wireless range of each node's wireless interface
2. `bandwidthBitRate` - Communication bit rate of the wireless interface


Please check the respective model's `.ned` file to see all the parameters. Details about models are 
found in [MODELINFO File](./res/info/MODELINFO.md). Details of the sample `.ini` file with multiple 
sample configurations is provided in [SAMPLEINI File](./res/info/SAMPLEINI.md). 



## Help

If you have any question or clarifications related to OMNeT++, please check the documentation provided at the [OMNeT++ site](https://www.omnetpp.org) and
[INET](https://inet.omnetpp.org) sites. Here are the important documents.

1. OMNeT++ - [Install Guide](https://www.omnetpp.org/doc/omnetpp/InstallGuide.pdf), 
   [Simulation Manual](https://www.omnetpp.org/doc/omnetpp/SimulationManual.pdf), 
   [IDE User Guide](https://www.omnetpp.org/doc/omnetpp/UserGuide.pdf)

2. INET - [Documentation](https://inet.omnetpp.org/Introduction.html)

3. Results processing in OMNeT++ IDE - Chapter 10 of the [IDE User Guide](https://www.omnetpp.org/doc/omnetpp/UserGuide.pdf)

4. INET Mobility Models - [Node Mobility](https://inet.omnetpp.org/docs/users-guide/ch-mobility.html)



## Frequently Asked Questions and Known Problems

We have compiled a list of frequently asked questions and solutions to some known 
problems. Check the [FAQ file](./res/info/FAQ.md) for these questions and answers.



## Questions or Comments

Firstly, if you have any questions, please check the [FAQ file](./res/info/FAQ.md) 
and if you cannot find answers there, then write to us. Secondly, if you have any 
comments or suggestions, we will be very glad to hear them. In both cases, please 
write to anyone of us listed blow using ops@comnets.uni-bremen.de.

  - Asanga Udugama
  - Jens Dede
  - Anna Förster
  - Vishnupriya Parimalam



## Model Contributors

There are many individuals who have used and extended OPS, either by extending 
existing models or by introducing new models. This section lists the individuals 
who have done such contributions.

  - Anas bin Muslim
  - Jibin P. John
  - Karima Khandaker 
  - Kirishanth Chethuraja
  - Zeynep Vatandas 



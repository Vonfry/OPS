# OPS
The Opportunistic Protocol Simulator (OPS, pronounced as oops!!!) is a set of
simulation models in OMNeT++ to simulate opportunistic networks. It has a
modular architecture where different protocols relevant to opportunistic networks
can be developed and plugged in. The details of current models available
are given in the following sections. The immediately following section is a
`tl;dr` for anyone who wishes to get it up and running without going through
all the explanations.

tl;dr
-----

For Linux or MacOS:

- Install and setup OMNeT++ v5.4.1 (use OMNeT++ installation Guide)
- Install (or update) dependent software (automake, libtool and others - check `Prerequisites I` section)
- Clone this repository: `git https://github.com/ComNets-Bremen/OPS`
- Checkout and build dependencies: `./bootstrap.sh`. This will also build SWIM and ExtendedSWIM with
  INET (see section `SWIM Mobility Model` and `ExtendedSWIM Mobility Model` below) and further, KeetchiLib
- Create makefiles: run `./ops-makefile-setup.sh`
- Build simulation: run `make`
- Create the input data files to UBM and ExtendedSWIM: run `python ./tools/ubm-data-gen.py ./tools/sample-ubm-data-gen-params.txt`. This will create `locations.txt`, `events.txt` and `properties.txt` files. For more information check [FORMATS](./FORMATS.md)
- Modify the `simulations/omnetpp-ubm-epidemic.ini` according to your preferences
- Simulate: run `./ops-simu-run.sh -m cmdenv -c simulations/omnetpp-ubm-epidemic.ini -p parsers.txt`
- Check data files generated by parsers to obtain graphs (check `Parsers` section)
- See how you can improve !!!


Introduction
------------

The number of computing devices of the Internet of Things (IoT) are expected
to grow exponentially. To address the communication needs of the IoT, research
is being done to develop new networking architectures and to extend existing
architectures.

Opportunistic Networking is an architecture that is currently being considered for
communications in the IoT. The Sustainable Communication Networks group (ComNets)
at the University of Bremen has developed a simulator called the Opportunistic
Protocol Simulator (OPS) to evaluate protocols and mechanisms for opportunistic
networks in the context of the IoT.

This repository contains the OMNeT++ based models of OPS. OPS consist of nodes,
each of which implements a protocol stack with the functionality required to
perform opportunistic networking. This release of the models consist of
models relevant to each protocol layer.


Prerequisites I (MUST for All Users)
------------------------------------

OPS requires the following software to be installed and operational in the systems
on which it is run.



### OMNeT++ 5.4.1

OMNeT++ 5.4.1 version must be installed and the PATH variable must be
set to run all the binaries of OMNeT++.

e.g.,

`export PATH=/Users/adu/Development/omnet/omnetpp-5.4.1/bin:$PATH (on a MacOS)`

Use the Installation Guide of OMNeT++.


###  Python

The log files of the simulations are parsed using Python to obtain the statistics. Once the statistics are
obtained, another set of Python scripts are used to plot the graphs. Therefore, for parsing logs and plotting
graphs, the following are required.

- Python version - `3.0 or above`
- Python packages - `numpy` and `matplotlib`


###  Automake Tools

KeetchiLib require automake tools. Therefore, before pulling the OPS code
from Github, install `autoconf`, `automake` and `libtool`. Check the
[README](https://github.com/ComNets-Bremen/KeetchiLib/blob/master/README.md) of `KeetchiLib`



Prerequisites II (Only for Advanced Users)
------------------------------------------

The software listed below are pulled automatically by OPS when setting up. Read this section
if different versions of the following software needs to be installed and configured.


### INET 4.0.0

OPS requires the use of [INET Framework](https://inet.omnetpp.org) of OMNeT++ to simulate
mobility and checks out an own version of INET where running `./bootstrap.sh`.
As downloading and compiling INET takes a while, you might prefer using your
own checkout of INET. To do so, please change the following values in the file
`settings.default`:

- `INET_BUILD=false`
- `INET_PATH=<your path to INET>`

When building the `INET Framework`, make sure to build and use the `release` version (not the `debug` version).

Please be aware that not all INET versions are compatible with all versions of
OMNeT++. Refer to the INET documentation for further information.

###  KeetchiLib

An external library called `KeetchiLib` is used by the `KKeetchiLayer` model to handle all the functionality
related to the [Organic Data Dissemination](https://link.springer.com/chapter/10.1007%2F978-3-319-26925-2_18)
forwarding model. The code for this library is available at Github and pulled for building by OPS. OPS builds
this library automatically, but if the build environment is not setup properly, the build will fail. Check the
[README](https://github.com/ComNets-Bremen/KeetchiLib/blob/master/README.md) of `KeetchiLib` to see what is
required to be done to setup the build environment.


Caveats
-------

Though OMNeT++ models are possible to be executed in Microsoft Windows, the scripts
provided here are only meant for Unix based systems (Mac OS X, Linux, etc.).


Adapt the default Settings
--------------------------

The OPS repository should work out of the box. The configuration
parameters are read from certain files:

- `settings.default` is located in the root directory of the simulation and
  contains the default parameters for building and running the simulation

- `~/.opsSettings` is the user defined version of the settings

We recommend to copy `settings.default` to your home directory and rename it to
`.opsSettings`. So your settings won't get lost in case of updates. If you
are later on facing problems (especially after an update), please check your
local settings. Maybe we have added additional parameters which you now should
add to your settings.

At the moment, the simulation uses the following parameters:

- `OPS_MODEL_NAME` - Name of the model executable generated my OMNeT++

- `INET_BUILD` - Boolean value to determine if a local INET checkout should be
  build. You can set this to false if you would like to use your own INET
  checkout

- `INET_PATCH` - Boolean value. If set to `true`, missing modules will be added
  to inet automatically

- `INET_PATH` - Path to where INET is located. Change this if you prefer using
  an own INET checkout

- `INET_LIB` - Path to where the dynamic library of INET is located (.so or .dylib).

- `INET_NED` - Path to where all the INET source files (i.e., NED files) are located.

- `INET_VERSION` - The git tag for the INET version to be used

- `OMNET_INI_FILE` - Path to your OMNeT++ simulation setup file. Change this
  according to the simulation you would like to run.

- `OMNET_OUTPUT_DIR` - Path where the binaries will be located

- `SIM_OUTPUT_DIR` - (Optional) Define a directory for the simulation output. If
  not set, a generic directory is used


Building the Model
------------------

1. Run `bootstrap.sh` to build the INET if you are not using your own version.

2. Run the `ops-makefile-setup.sh` script. This will result in the creation of
the Makefiles in all the relevant folders, pulling the dependents linked in the
`./modules/` folder and building these dependents.

3. Run make from the root folder of OPS.

When successfully compiled and linked, an executable by the name given
in `OPS_MODEL_NAME` must be present in the root folder of OPS.



Running the Model
-----------------

1. Create the node model, the network, an appropriate OMNeT++ parameter file and other required
   parameter files to run the required simulations. Currently there are many samples of these 3 components.

  - Node Model - Currently implemented node model is `KUBMNode.ned`
  in the `src/` folder. IMPORTANT: If a new node model is created, remember to include
  the name of this model in the `expectedNodeTypes` parameter of the `KWirelessInterface`
  module
  - Network - Currently created simulation network is `OpsNetE.ned` in the `simulations/` folder
  - OMNeT++ Parameter File - Currently available parameter file is `omnetpp-ubm.ini` in the `simulations/` folder
  - Other Files - The 3 additional files required by the models are `locations.txt`, `events.txt` 
    and `properties.txt`. Run `python ./tools/ubm-data-gen.py ./tools/sample-ubm-data-gen-params.txt` to
    create these files. Check `./tools/sample-ubm-data-gen-params.txt` file to know about the different 
    parameters used in creating these files. Information about the formats of these files are available
    in [FORMATS](./FORMATS.md).

2. Modify the settings to point to the created OMNeT++ parameters in the
previous step.

3. Run the `ops-simu-run` script file to run simulations. Simulation can be run using the GUI
(Qtenv) mode or the command line mode. To enable the required mode, the  `-m` switch must be used.

  - `ops-simu-run -m qtenv` - The `qtenv` mode brings up the GUI version of the simulation
  where the buttons have to be pressed to start simulating and further, this mode also shows
  the animations. __WARNING:__ this mode results in longer simulation times (VERY LONG)

  - `ops-simu-run -m cmdenv` - The `cmdenv` mode directly starts the simulation and the
  command line is blocked until the simulation is completed.


The results files and the log files are created in the `simulations/out/` folder.
For a detailed description of the `ops-simu-run.sh` script, please refer 
to [README_ops-simu-run.md](./README_ops-simu-run.md)


Creating Your Own Scenarios
---------------------------

The network file (i.e., scenario) created to simulate nodes deployed with the different
protocol layers are located in the `simulations/` folder. As indicated in the Running
the Model section, check the given example networks (e.g., `OpsNetE.ned` file) and the
parameter file (e.g., `omnetpp-ubm.ini` file) to make your own networks.


Node Architecture
-----------------

The architecture of a node uses a number of protocol layers which can be configured
based on the scenario considered. Generally, every node has the following layers.

```
                           +------------------------+
                           |      OppNets Node      |
  +--------------+         |  +------------------+  |
  | Notification |         |  |Application Layer |  |
  |  Generator   +------------+     with UBM     |  |
  +--------------+         |  +--------+---------+  |
                           |           |            |
                           |  +--------+---------+  |
                           |  |   Opportunistic  |  |
                           |  | Networking Layer |  |
                           |  +--------+---------+  |
                           |           |            |
                           |  +--------+---------+  |
                           |  | Link Adaptation  |  |
                           |  |     Layer        |  |
                           |  +--------+---------+  |
                           |           |            |
                           |  +--------+---------+  |
                           |  |    Link Layer    |  |
                           |  | +--------------+ |  |
                           |  | |   Mobility   | |  |
                           |  | +--------------+ |  |
                           |  +--------+---------+  |
                           +-----------|------------+
                                       |
```

The Notification Generator is a network-wide model that holds a set of messages and disseminates to the
user behaviour models of nodes. The models associated with notification generation are,

   - `KBasicNotificationGenerator` - Notifications (i.e., messages) are held and disseminated to the user behaviour 
models of each node to inject them into the network.


The layers of a node can be configured to use different implementations relevant to the specific layer as 
listed below.


1. Application Layer with UBM - Applications generate data and feedback based on the preferences
   of the users. The details of the application and the user behaviour model (UBM) are described in 
   the publication [Reactive User Behaviour and Mobility Models](https://arxiv.org/pdf/1709.06395.pdf). 
   Current models are,

   - `KBasicUBM` - Performs the enforcement of user preferences on the notifications (messages)
   - `KBasicUBMApp` - Injects and receives to/from network

2. Opportunistic Networking Layer - Performs the forwarding of data and feedback according
   to the forwarding strategy employed. Current implementations are,

   - `KRRSLayer` - Implements a simple forwarding strategy based on the Randomised
     Rumor Spreading (RRS) algorithm which randomly selects a data item to broadcast
     to a node's neighbourhood
   - `KKeetchiLayer` - Implements the Organic Data Dissemination algorithm as described
     in the publication [A Novel Data Dissemination Model for Organic Data Flows](https://link.springer.com/chapter/10.1007%2F978-3-319-26925-2_18) by
     A. Foerster et al
   - `KEpidemicRoutingLayer` - Implements the epidemic routing algorithm as described
     in the publication [Epidemic Routing for Partially-Connected Ad Hoc Networks](http://issg.cs.duke.edu/epidemic/epidemic.pdf)
     by A. Vahdat and D. Becker

3. Link Adaptation Layer - Tasked with converting packets sent by the Opportunistic
   Networking Layer to the specific link technology used (at Link Layer). Currently
   implemented has a simple pass-through layer.

   - `KLinkAdaptLayer` - Pass-through layer

4. Link Layer - Implements the operations of a link technology used. Currently has
   the following implementation.

   - `KWirelessInterface` - A simple wireless interface implementation (without the `INET
     framework`)

5. Mobility - Implements the mobility modelling for the node. Currently uses the interfaces
   provided by the `INET framework` and therefore, is able to use any of the mobility models
   supported by the `INET framework`.



Simulation Parameters
---------------------

Before running simulations, the OMNeT++ parameter file (.ini file) must be configured properly
to setup the node models and the network. Following are some of the important parameters to
consider.

- `numNodes` - The number of nodes in the network
- `wirelessRange` - The wireless coverage range. The default is 100 meters
- `forwardingLayer` - The forwarding layer to use
- `mobilityType` - The mobility model used to move the nodes

There are many more parameters that need to be set according to the scenario to be simulated. Most
of these parameters have default values. Check the sample .ini files in `simulations/` folder to
see what parameters are usually set to run a simulation.



Logging
-------

Simulation runs result in the creation of logs that  dump information about the activities of a
model. These logs are used by the parsers described below. The contents of these logs are coded
to reduce the sizes of the logs. The codes used are listed in [ENCODINGS](./LOG_ENCODINGS.md). When
developing new models, please check these encodings to see what can be reused.





Parsers
-------

The `parsers/` folder has sub folders that include Python scripts to parse the 
logs created to generate statistics. These scripts are automatically
called by `ops-simu-run.sh` script once a simulation is run. To indicate what parsers
are required to be run, they must be listed in `parsers.txt` file.

Once the statistics are computed and placed in the output files, use Python 
(`numpy` and `matplotlib` packages) or any other tool (e.g., `Matlab)`
to plot graphs of the statistics.

In the following is a description of the currently available parsers.

- `s04-contact-times` folder - This folder contains a script to compute statistics 
related to contacts made by nodes during the simulation.

- `s05-keetchi-evaluation-parsers` folder - This folder contains a set of scripts used
in evaluating Keetchi and Epidemic Routing behaviour with UBM



Use of Mobility Models with OPS
-------------------------------

OPS can be configured to use any INET based mobility model. Here are 2 special mobility
models that we have used in our work.



###  SWIM Mobility Model

[Small Worlds in Motion](https://arxiv.org/pdf/0809.2730.pdf) (SWIM) is a mobility model
developed by A. Mei and J. Stefa. The INET code for the SWIM was developed by our research
group. Swim is automatically checked out using the `bootstrap.sh` script. In
the default settings, all files are copied automatically to the corresponding
directories of INET. SWIM part of OPS and it will be built automatically by the setup scripts.
Once installed, set parameters in `.ini` file in `simulations/` folder to use the SWIM mobility model.


###  ExtendedSWIM Mobility Model

The ExtendedSWIMMobility model is an extended version of the SWIM model that is used by the 
user behaviour model in OPS where in addition to the SWIM capabilities, it can make mobility decisions 
on requests made by the user behaviour model (i.e. Reactive Mobility). It is automatically built by 
the setup scripts of OPS.


###  BonnMotion Mobility Model

Another mobility model that we have used extensively is the Bonn Motion model. This model
(which is available in INET) requires a trace to move nodes in a network. The trace files
are generated using a tool provided by the authors of the Bonn Motion model. The tool can
be requested to generate traces based on synthetic mobility models (e.g., Random Way Point
mobility model) or from an actual mobility trace (e.g., San Fransisco Taxi trace at
www.crawdad.org).

If you look at some of the `.ini` files in `./simulations/` folder, you see that we refer
to a `50_traces.gpx.movements` file. This is a 50 node version of the San Fransisco Taxi trace
we created to use with the INET BonnMotion Mobility Model. We have not made this file available
here due to the size of this file (49MB). If you require that file, please send us an email.



FAQ
---

We have compiled a list of common questions and answers [here](./FAQ.md)



Questions or Comments
---------------------

Firstly, if you have any questions, please check the FAQ (linked above) and if you cannot find answers there, then
write to us. Secondly, if you have any comments or suggestions, we are very glad to hear them. In both cases, please
write to us using any of the e-mail adresses below.

  - Asanga Udugama (adu@comnets.uni-bremen.de)
  - Jens Dede (jd@comnets.uni-bremen.de)
  - Anna Förster (anna.foerster@comnets.uni-bremen.de)
  - Vishnupriya Parimalam (vp@fb1.uni-bremen.de)



Model Contributors
------------------

There are many individuals who have used and extended OPS, either by extending existing models or by introducing new models. This section lists the individuals who have done such contributions.

  - Anas bin Muslim
  - Jibin P. John
  - Karima Khandaker 




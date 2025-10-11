# Generate Timing Graphs with OpenSTA 

## Parallax Static Timing Analyzer

## Build from source

OpenSTA is built with CMake.

**Prerequisites**
The build dependency versions are shown below. Other versions may work, but these are the versions used for development.

```bash
         Ubuntu   Macos
        22.04.2   14.5
cmake    3.24.2    3.29.2
clang             15.0.0
gcc      11.4.0
tcl       8.6      8.6.16
swig      4.1.0    4.1.1
bison     3.8.2    3.8.2
flex      2.6.4    2.6.4
```

External library dependencies:

```bash
           Ubuntu   Darwin  License
eigen       3.4.0   3.4.0   MPL2  required
cudd        3.0.0   3.0.0   BSD   required
tclreadline 2.3.8   2.3.8   BSD   optional
zLib        1.2.5   1.2.8   zlib  optional
```

<img width="958" height="806" alt="Screenshot 2025-10-09 124931" src="https://github.com/user-attachments/assets/c9ec862e-9d7f-4a0c-a409-a3d06485c7e6" />

The TCL readline library links the GNU readline library to the TCL interpreter for command line editing To enable TCL readline support use the following Cmake option: See (https://tclreadline.sourceforge.net/) for TCL readline documentation. To change the overly verbose default prompt, add something this to your ~/.sta init file:

```bash
if { ![catch {package require tclreadline}] } {
  proc tclreadline::prompt1 {} {
    return "> "
  }
}
```
The Zlib library is an optional. If CMake finds libz, OpenSTA can read Liberty, Verilog, SDF, SPF, and SPEF files compressed with gzip.

CUDD is a binary decision diageram (BDD) package that is used to improve conditional timing arc handling, constant propagation, power activity propagation and spice netlist generation.

CUDD is available **https://github.com/davidkebo/cudd/blob/main/cudd_versions/cudd-3.0.0.tar.gz**

* Unpack and build CUDD.

```bash
tar xvfz cudd-3.0.0.tar.gz
cd cudd-3.0.0
./configure
make
```

You can use the "configure --prefix" option and "make install" to install CUDD in a different directory

<img width="1260" height="801" alt="Screenshot 2025-10-09 125555" src="https://github.com/user-attachments/assets/d094ac73-2f14-4b7c-92a5-b9319e74e77c" />
<img width="1255" height="795" alt="Screenshot 2025-10-09 125632" src="https://github.com/user-attachments/assets/080d2a98-34bf-42c9-88d3-0579a2ae329a" />
<img width="1258" height="801" alt="Screenshot 2025-10-09 125715" src="https://github.com/user-attachments/assets/8cb20209-f21f-4027-b609-1bca11e90b48" />

## Building with CMake
Use the following commands to checkout the git repository and build the OpenSTA library and excutable.

```bash
git clone https://github.com/parallaxsw/OpenSTA.git
cd OpenSTA
mkdir build
cd build
cmake -DCUDD_DIR=<CUDD_INSTALL_DIR> ..
make
```

## Build with Docker
An alternative way to build and run OpenSTA is with Docker. After installing Docker, the following command builds a Docker image.

```bash
cd OpenSTA
docker build --file Dockerfile.ubuntu22.04 --tag opensta .
```
<img width="1276" height="640" alt="Screenshot 2025-10-09 132209" src="https://github.com/user-attachments/assets/ee5085b0-8701-4508-94ee-e2fd36c2ac68" />

To run a docker container using the OpenSTA image, use the -v option to docker to mount direcories with data to use and -i to run interactively.

```bash
docker run -i -v $HOME:/data opensta
```
<img width="973" height="434" alt="Screenshot 2025-10-09 175904" src="https://github.com/user-attachments/assets/1d8907e5-0a3b-47c2-b068-bb7e1c1c0ac3" />

## Install using a package manager
Guix

OpenSTA is available in the https://hpc.guix.info/package/opensta

```bash
  guix install opensta
```
# Timing Analysis using SDF

 A sample command file that reads a library and a Verilog netlist and reports timing checks:
 
```bash
#To open the STA
docker run -i -v $HOME:/data opensta

#Read the liberty file [ for timing info]
read_liberty /data/OpenSTA/examples/nangate45_typ.lib.gz

#Read the verilog file
read_verilog /data/OpenSTA/examples/example1.v

#Link the top module
Format :link_design <module name>   [module name is top]
link_design top

#Standard delay format to describe the timing delays
read_sdf /data/OpenSTA/example/example1.v

#Creates a clock definition for timing analysis
create_clock -name clk -period 10 {clk1 clk2 clk3}

#Specifies the input delay of external signals
set_input_delay -clock clk 0 {in1 in2}

#report timing check results
report_checks
```
<img width="1260" height="808" alt="Screenshot 2025-10-10 094213" src="https://github.com/user-attachments/assets/116f5dc6-8db0-4f8e-bcc0-4c95d7f86bfd" />
<img width="1262" height="809" alt="Screenshot 2025-10-10 094226" src="https://github.com/user-attachments/assets/b65b39e7-7de4-4699-ac89-d8e61af232f9" />
<img width="1255" height="807" alt="Screenshot 2025-10-10 094233" src="https://github.com/user-attachments/assets/d86230a7-5911-4703-a499-7ba4739f23e5" />

<img width="666" height="678" alt="Screenshot 2025-10-10 095523" src="https://github.com/user-attachments/assets/2c969c51-f4a8-4e98-9b84-329a591e343e" />

## Timing Analysis with Multiple Process Corners

An example command script using three process corners and +/-10% min/max derating:

```bash
#Defines multiple analysis corners
define_corners wc typ bc

#Reads the timing library file (.lib)
read_liberty -corner typ /data/OpenSTA/examples/nangate45-typ.lib.gz
read_liberty -corner wc /data/OpenSTA/examples/nangate45-slow.lib.gz
read_liberty -corner bc /data/OpenSTA/examples/nangate45-fast.lib.gz

#read the gate-level Verilog netlist
read_verilog /data/OpenSTA/examples/example1.v

#Links the design hierarchy using the libraries just read
link_design top

#Apply timing derates to model On-Chip Variation (OCV)
set_timing_derate -early 0.9
set_timing_derate -late 1.1

#Creates a clock definition for timing analysis
create_clock -name clk -period 10 {clk1 clk2 clk3}

#Specifies the input delay of external signals
set_input_delay -clock clk 0 {in1 in2}
```
<img width="1116" height="806" alt="Screenshot 2025-10-10 104630" src="https://github.com/user-attachments/assets/980d2393-c218-43b3-85ee-7afee23f1cdc" />

```bash
#Generates a combined timing report for both setup (max delay) and hold (min delay) checks
report_checks -path_delay min_max
```

<img width="1118" height="807" alt="Screenshot 2025-10-10 104642" src="https://github.com/user-attachments/assets/af3419a4-b22a-4e11-bcc4-dc2f87cc9457" />
<img width="1117" height="803" alt="Screenshot 2025-10-10 104652" src="https://github.com/user-attachments/assets/6668cca3-faba-44a5-922a-e64467a4eb14" />


#Reports timing analysis specifically for the typical corner
report_checks

# Power Analysis

OpenSTA also supports static power analysis with the report_power command. Probabalistic switching 
activities are propagated from the input ports to determine switching activities for internal pins.

```bash
#Read the liberty file [ for timing info]
read_liberty /data/OpenSTA/examples/sky130hd_tt.lib

#Read the verilog file
read_verilog /data/OpenSTA/examples/gcd_sky130hd.v

#Link the top module
Format :link_design <module name>   [module name is top]
link_design GCD

#Reads the Synopsys Design Constraints (SDC) file
read_sdc /data/OpenSTA/examples/gcd_sky130hd.sdc

#Reads the Standard Parasitic Exchange Format (SPEF) file that contains the RC parasitic extraction data for the design
read_spef /data/OpenSTA/examples/gcd_sky130hd.spef

#Defines switching activity (α) for all input ports in the design
set_power_activity -input -activity 0.1

#Overrides the switching activity specifically for the reset input port, setting it to zero
set_power_activity -input_port reset -activity 0
```

<img width="1148" height="808" alt="Screenshot 2025-10-10 113424" src="https://github.com/user-attachments/assets/752da1d6-5573-462b-8f3a-d380450589bf" />

```bash
#Generates a power report for the design
report_power
```

<img width="1147" height="804" alt="Screenshot 2025-10-10 113742" src="https://github.com/user-attachments/assets/2fa96d37-08d7-43a4-b685-a4c2453e94b6" />

<img width="1186" height="796" alt="Screenshot 2025-10-10 160939" src="https://github.com/user-attachments/assets/84c8deab-f486-4479-a96e-43f3e02c93cc" />



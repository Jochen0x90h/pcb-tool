# PCB Tool
Tool for modifying KiCad schematic (.kicad_sch) and PCB (.kicad_pcb) files.
Can be used to hide some or all references or remove nets from pads prior to autorouting.

## Features
* Support for KiCad 8
* Hide references using regular expression match
* Remove nets from pads using regular expression match

## Usage
Parameter                                      | Description
-----------------------------------------------|-----------------
-sfp \<reference regex> \<footprint>           | Set footprint (only schematic)
-xfp \<old footprint regex> \<new footprint>   | Exchange footprint (only schematic)
-ijb \<bom file>                               | Import downloaded JLCPCB BOM Excel file (only schematic)
-as                                            | Annotate schematic using sheet number as first number (only schematic)
-rp \<property regex>                          | Remove property (schematic and pcb)
-xsw \<old segment width> \<new segment width> | Exchange segment width (only pcb)
-xsl \<old layer> \<new layer>                 | Exchange segment layer (only pcb)
-xvs \<old via size> \<new via size>           | Exchange via size (only pcb)
-hr \<reference>                               | Hide reference, supports regular expressions, e.g. R.* (only pcb)
-rn \<net>                                     | Remove net, supports regular expressions, e.g. GND (only pcb)
-mfp \<footprint>                              | Move footprint to border (only pcb)


## Examples
Command                                                          | Description
-----------------------------------------------------------------|-----------------
pcb_tool -xfp R_0603_1608Metric R_0402_1005Metric test.kicad_sch | Exchange resistor footprint
pcb_tool -hr .* test.kicad_pcb                                   | Hide all references
pcb_tool -hr R.* test.kicad_pcb                                  | Hide all resistors
pcb_tool -rn GND -rn \\+3V3 test.kicad_pcb                       | Remove GND and 3.3V before autorouting

## Build

* Make sure conan 2.x is installed (see below)
* Run $ python cinstall.py in this project to install dependencies to build/ and create CMakeUserPresets.json
* Run $ python install.py in this project to install to ~/.local/bin

## Develop

* Follow build steps until you have CMakeUserPresets.json
* Open an IDE that supports CMakeUserPresets.json (e.g. Visual Studio Code)
* Select configure and build preset for CMake
* Select build target for CMake
* Build

## Conan 2.x

If you use conan for the first time, run

```console
$ conan profile detect
```

It creates a default profile in ~/.conan2/profiles

### Windows
Open default profile (~/.conan2/profiles/default) with text editor, check that the compiler is msvc and set cppstd to 23:

```
[settings]
...
compiler=msvc
compiler.cppstd=23
...
```

If the compiler is missing, install Visual Studio Community Edition (in addition to VSCode!) with C++ for desktop option and run conan profile detect again.

### Linux
Open default profile (~/.conan2/profiles/default) with text editor, check that the compiler is gcc and set cppstd to 23:

```
[settings]
...
compiler=gcc
compiler.cppstd=23
...
```

If you want to let conan install missing packages, add these lines at the end of the profile:

```
[conf]
tools.system.package_manager:mode=install
tools.system.package_manager:sudo=True
```

### Debug Profile

Create debug profile ~/.conan2/profiles/debug by copying the default profile and set

```
build_type=Debug
```

### CMake Presets

Run

```console
$ python cinstall.py
```

It installs dependencies to build/ generates CMakeUserPresets.json which can be used by IDEs such as VSCode

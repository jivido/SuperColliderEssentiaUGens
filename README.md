# SuperColliderEssentiaUGens
Running Essentia algorithms in SuperCollider

## Requirements
Tested on Linux Mint with SuperCollider 3.14 and Essentia 2.1 beta 5.

Install [Essentia](https://github.com/MTG/essentia).
When compiling from source, make sure to avoid `--build-static`, otherwise the dynamic library (.so) won't get installed. 

## Installation
Currently you've got to build each UGen seperately. Go into the directory of the plugin, and do this:
Make sure to change the path of the SuperCollider source, and the path for your extensions (home/user/.local/share/SuperCollider/Extensions) 
~~~
mkdir build && cd build
cmake -DSC_PATH=~/Downloads/supercollider -DCMAKE_INSTALL_PREFIX=/path/to/extensions ..
make
make install
~~~

## Issues
### Library not found
Check where (and if) the libessentia.so is installed.
Then check the CMakeLists.txt line 89, if the paths correspond.

Sometimes the .so library isn't found, this results in a message like this (and a server crash) when booting the server.
`ERROR: dlopen '/home/user/.local/share/SuperCollider/Extensions/EssentiaHFC/EssentiaHFC/EssentiaHFC_scsynth.so' err '/home/user/.local/share/SuperCollider/Extensions/EssentiaHFC/EssentiaHFC/EssentiaHFC_scsynth.so: undefined symbol: _ZN8essentia15EssentiaFactoryINS_8standard9AlgorithmEE9_instanceE'`

# Example
~~~
(
{
	var hfc, sin;
	sin = SinOsc.ar(LFNoise1.kr(1, 400, 1000), mul: 1);
	hfc = EssentiaHFC.ar(sin).poll;
	sin!2; // Only play the sine oscillator, show the HFC value with .poll
}.play;
)
~~~

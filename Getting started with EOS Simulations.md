## Basics

Always remember to `source setup.sh` first.  
Then, run `eos MACROFILENAME.mac` to run an Eos simulation.  
The macro files (`.mac`) tell EosSims what you want to run.  

## Examples

Here's a simple macro that you can try copying and running:
https://github.com/EosDemonstrator/EosSimulations/blob/main/macros/eos_electrons.mac  
This simulates some 2 MeV electrons pointing down from the center of the detector.  

Here's another:  
https://github.com/EosDemonstrator/EosSimulations/blob/main/macros/calibration/eos_y90_dirsource.mac  
This simulated our directional source pointing down from the center of the detector, and the Y90 beta decay is actually simulated, too.  
Note: Because this actually simulates the beta decay from inside the source, surrounded in all the shielding, there will be a very small percentage of betas that actually escape the source and emit any light in the detector. Most of them never make it out.

I think a nice first step would be to just try running these two example macros, and learning how to look at the output files they produce. There are three things you can try doing:
1. opening the root files directly
2. parsing through them to try plotting interesting stuff
3. visualizing the geometry files themselves

## Macro files

The macro files are pretty easy to read, but it's not always straightforward to find the commands you want to add. (Documnetation exists, but it's not complete. Best to just ask)  
Order matters in these macro files, so it's best to follow the examples as closely as possible when adding commands.  

To highlight some stuff:

### Loading the geometry
```
/rat/db/set DETECTOR geo_file "Eos/Eos.geo"
```
`.geo` are geometry files. `Eos/Eos.geo` is the geometry file for the detector itself.
This path is always relative to `/project/snoplus/EosSimulations/install/share/eos/ratdb/`.

Note, new files in `/project/snoplus/EosSimulations/install/` can potentially be overwritten when re-compiling EosSimulations, so people should back these files up before compiling it - this does not always happen, so keep a backup copy for yourself if needed.

You can also load more geo files in addition, like this:
```
/rat/db/load cal/dir_source.geo
```
which is done in `eos_y90_dirsource.mac` to load our directional source geometry in addition to the Eos detector.

The geo files specify what material and shape and size and position everything is,
but you can modify any of these in the macro file.
That's what's being done in these lines:

```
/rat/db/set GEO[eos_inner] material "wbls_1pct"

/rat/db/set GEO[source_mother] position [0.0,0.0,0.0] 
/rat/db/set GEO[source_mother] rotation [0.0,0.0,0.0]
```

### Generators
```
/generator/add combo gun:point:uniform
# Downward going 2.0 MeV electrons at the center of the detector
/generator/vtx/set e- 0.0 0.0 -1.0 2.0
/generator/pos/set 0.0 0.0 0.0

##### RUN ###########
# Run ten events
/run/beamOn 10
```

Generators are relatively well documented here, if you need anything specific: https://ratpac.readthedocs.io/en/latest/users_guide/generators.html

The ones that we'll most likely use are the gun, gun2 and decaychain generators.

### Storing trajectories

ratpac, by default, does not store tracking information of all particles, but we can ask it to do so:
```
/rat/proclast eosntuple
/rat/procset include_tracking 1
```
The second line will store trajectories for all particles in the output root file (this has to go after the `eosntuple` line). But we don't need the tracking information for optical photons, which is the vast majority of the particles simulated. You can prune these by:

```
/rat/proc prune
/rat/procset prune "mc.track:opticalphoton"
```

Or prune any type of particle that you don't want to store in a similar manner.

More information here: https://ratpac.readthedocs.io/en/latest/users_guide/processors.html#prune

### Triggered or untriggered events

The simulation will, by default, only store events that "triggered" the detector. I can't remember what the default trigger nhit threshold is, but you can change it:
```
/rat/proc splitevdaq
/rat/procset trigger_threshold 2.0
```
Or just tell it to store all events, regardless of trigger:
```
/rat/procset include_untriggered_events 1
```
(this has to go after `/rat/proclast eosntuple`)

This is normally not necessary though.

### Output Files
By default, running rat will produce ntuple files with the name `output.ntuple.root`.  
You can change this when running the simulation by doing:
```
eos example.mac -o newOutputName.root
```
Or, alternatively, specify it in the macro by adding the line:
```
/rat/procset file "/path/to/output.root"
```
This line has to go after the line
```
/rat/proclast eosntuple
```
which specifies the type of output file (`eosntuple`) we're asking ratpac to produce, and that this type of output file should have the name that we've specified.  
There are other types of output files like `outroot` that you don't have to care too much about at the moment.  


## Opening ROOT files
```
root ROOTFILENAME.root
```

will open the root file in ROOT as `_file0`  
You can look at what the root files contain by doing this:

```
_file0->ls()
```

You should see that there are two trees called `meta` and `output`.  
You can look at what branches the trees contain by doing:

```
output->Print()
```

or the actual contents of the branches for some event:

```
output->Show(0) # or Show(1) or Show(999)
```

and see how many entries it has:

```
output->GetEntries()
```

These commands are nice to know so that you can do some sanity checks or quickly check what the root files contain.  

Of course, ROOT also lets you view its file contents through TBrowser, a GUI where you can navigate through trees and look at the histograms.  
(If you're on VScode, the ROOT viewer extension is nice; basically helps you load the TBrowser in the VScode window.)  
But if you're just trying to see how many events were collected, there's no need to use this which will take a bit to load.  

When you're actually having to do some analysis, it's probably most convenient to just use pyROOT or uproot (both in python) to read the data, sort/do things, and plot in matplotlib.  

## Using pyROOT

### Event Display
One thing you can check is of course the event display of the simulation.  
[EventDisplay.py](https://github.com/EosDemonstrator/Waveform_Analysis/blob/master/EventDisplay.py) should work directly on the simulation files.

### Checking nhit, charge, and tracks
Here's a script that should plot the tracks of simulated betas:
```
/project/snoplus/SoYoung/eos/plot_tracks.py
``` 
(you will have had to save tracks to the output ntuple for this to work)

One thing you should try is to plot the energy, nhit and charge distribution for the events you have simulated. The script above should be a good starting point to do that.  
It's good to look at these plots and always check that the results are what we expect.  

There's currently no good documentation of the outntuple content, so if you want to know more details about the information a branch contains, you'll just have to ask. (or look at the source code)  

## Geometry Visualization

`/project/snoplus/SoYoung/eos/simulation/viz/visualize.mac` is a macro script that creates a `.wrl` file.  
Run it like you would run any other eos simulation.   
Then, you can view the `.wrl` file using Paraview to see the 3D rendering of the geometry that you've loaded, and also trajectories of events if you've simulated any.  

This is useful for sanity checks when messing with the geometry -- either modifying the geo files or trying to rotate our source.  

### Running Paraview on SCC

Paraview is installed on SCC, but you'll have to install a VNC program and remote desktop into SCC to see things: https://www.bu.edu/tech/support/research/system-usage/connect-scc/remote-desktop-vnc/  
(just realizing this webpage recommends using SCC ondemand's desktop session; I've only ever used VNC but you can try it out if you want)

On the remote desktop, open a terminal window, type
```
module load paraview
```
to load the paraview module, then run the program by
```
paraview
```
Open the `.wrl` file you just created to look at it.
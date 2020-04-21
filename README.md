# star-fwd-sim

Generates simulation for STAR Forward detectors

### Usage
Clone the repo and `cd` into it.
```sh
git clone https://github.com/jdbrice/star-fwd-sim.git
cd star-fwd-sim
```

Now you cen generate simulation with:
```
./run.sh
```
This runs the `genfzd` script, which runs `testg.kumac`, inside a docker container capable of running `starsim`.
It will run starsim and produce a file `testg.fzd` in your current directory.  


### Details
genfzd sets up the environment and then runs:

```
starsim -w 0 -b testg.kumac nevents=1 ntrack=100 etamn=2.4 etamx=4.1 ptmn=4.5 ptmx=5.0
```

This runs `starsim` with the `testg.kumac` script:

```
MACRO testg tag=dev2021 nevents=100 ntrack=10 g3id=5 ptmn=0.2 ptmx=5.0 etamn=2.5 etamx=4.0 
DETP GEOM [tag]
GEXE $STAR_LIB/libStarAgmlUtil.so
GEXE $STAR_LIB/xgeometry.so
AGUSER/GKINE [ntrack] [g3id] [ptmn] [ptmx] [etamn] [etamx] 
GFILE o testg.fzd
TRIG [nevents]
EXIT
RETURN
```

The options include:  
`tag` - the geometry tag to use (default is `dev2021`)  
`nevents` - the number of events to generate  
`ntrack` - the number of tracks/event  
`g3id` - the geant3 particle id  
`ptmn` - minimum pT  
`ptmx` - maximum pT  
`etamn` - minimum eta  
`etamx` - maximum eta  


### Modifying Geometry
The `FstGeom.xml` file can be modified. In order for the effect to be acknowledged:

1. Launch an interactive container
```sh
./dev.sh
```

2. Build the new geometry
```sh
./build.sh
```

3. Generate the simulation file with the new geometry
```sh
./genfzd
```


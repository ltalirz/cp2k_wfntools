# wfnTools
### Tools to parse and combine CP2K's .wfn wavefunction files

Consider:
* a sistem with two fragments, we want to compute the interaction
* the wavefunction of the fragmentA, a
* the wavefunction of the fragmentB, b 
* the geometry of the fragmentA, A
* the geometry of the fragmentB, B
* by convention, A is the framework (or surface, bigger molecule) and B is the adsorbate
 
### Examples:
#### 1) Print the formatted wavefunction:
```
wfntools parse -a fragment.wfn -o outputname
```
Prints the file `outputname.fwfn`

#### 2) Combine two wavefunctions:
```
wfntools combine -a fragmentA.wfn -b fragmentB.wfn -o outputname
```
Prints the combined `outputname.wfn` of the two fragments

#### 3) Combine two wavefunctions and geometries:
```
wfntools combine -a fragmentA.wfn -b fragmentB.wfn -A fragmentA.xyz -B fragmentB.xyz -o outputname
```
* Prints the combined `outputname.wfn` of the two fragments.
* Prints the combined `outputname_nolabel.xyz` of the two fragments, for visualization.
* Prints the combined `outputname_label.xyz` of the two fragments, to be used for CP2K with "_A" and "_B" labels for the atoms.
* Prints the combined `outputname.kind` h the CP2K's &KIND section. Use `-bs` and `-pot` options to specify the basis set and the pseudopotential choice (TODO: read it from a CP2K input or output file)
TODO: use the `-check` keyword to check if the number of atoms are consistent in the geometry and wfn files. 

#### 4) TODO: Prepare the inputs for a Counterpoise Corrected calculation (recycling the wavefunctions)
```
wfntools cp -AB bothfragments_label.xyz -o outputname
```

### Utilities:

#### 5) TODO: Insert the fragment B in a pore of fragment A:
```
wfntools makeAB -A fragment1.xyz -B fragment2.xyz -cell unitcell1.cell -o outputname -nout 10
```
This is an usefull utility to generate a number of `-nout` starting configurations of combined geometries, possibly to later adjust in Avogadro. Fragment B is inserted in a random not-overlapping position, conisdering the atoms as rigid spheres with a defined radius (from UFF) possibly scaled by a factor `-scalerad`.
Prints `outputname_AB_1.xyz` `outputname_B_1.xyz` `outputname_AB_2.xyz` `outputname_B_2.xyz` ... `outputname_AB_nout.xyz` `outputname_B_nout.xyz` ... 

#### 6) TODO: Label A and B in the AB geometry file:
```
wfntools labelAB -AB both_fragments.xyz -nA 156 -o outputname 
```
or
```
wfntools labelAB -AB both_fragments.xyz -nB 3 -o outputname 
```
 Print a new `outputname_label.xyz` file where A and B are labelled according to the number of `-nA` atoms in A (or specifying `-nB`). Use the `-swapAB` option to read first B and then A in the .xyz file. 

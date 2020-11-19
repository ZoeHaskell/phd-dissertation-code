# phd-dissertation-code
Repository for all the code referenced in my thesis:  Haskell, Z. A. (2020) Mapping between tree metric spaces and time series: new constructions with applications to limit theorems for branch counts in partial Galton-Watson trees and signal processing. Dissertation at the University of Nevada, Reno.

Programs for transforming data between function-space and tree-space: 

trees to functions is performed by the Harris Path (harrispath), and functions to trees by Level Set Tree (level_set_tree).

Pruning programs remove a set of small leaves from trees, and denoise the corresponding time series. Options are included for paths with long flat portions, i.e. plateaus (pruning).

Comparisons in terms of mean-squared error are performed relative to the FFT and to wavelets. (Compare_Prune_to_FFT and CompareWavelets)
The CompareWavelets program requires installation of Wavelab850.

Edge detection is performed using pruning (EdgeDetectionWithPruning)

Auxillary functions needed to run the code described above include CompareError, KidsArrayFromTree, PickTrimTypeWOdeg2Nodes, PickTrimType, TopoDelete, RenameNodes, Fn2Excursion, tokunagap, ewing, skeleton and treerank. 

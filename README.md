# phd-dissertation-code
Repository for all the code referenced in my thesis:  Haskell, Z. A. (2020) Mapping between tree metric spaces and time series: new constructions with applications to limit theorems for branch counts in partial Galton-Watson trees and signal processing. Dissertation at the University of Nevada, Reno.

Main functions:

harrispath: Graphs a Harris path from the tree and lengths given.
[X,V,t]=harrispath(T,L,t) 

       Inputs:
       T: parent pointer vector of a tree (T(i) = parent of node i)
       L: vector of edge lengths (ones if unspecified)
       t: time steps specified (ones if unspecified)
       
       Outputs:
       X: vector of the heights of the harris path for T
       t: time steps for X
       V: tracks how T has been embedded; f uses input for edge lengths L or generates them if unspecified.


level_set_tree: Creates the level-set tree from a given continuous function with a finite number of local extrema. 
[T,L,IMax,IMin,Xs,Fs,Ms]=level_set_tree(f,X,plateaus = 0)

       Inputs: 
       f: a real function with a finite number of local extrema.
       X: timestamps for f
       plateaus: option to take plateaus into account if set equal to 1.
       
       Outputs: 
       T: parent pointer vector of a tree
       L: vector of edge lengths
       IMax: indices of local maxima
       IMin: indices of local minima
       Xs: time stamps of local extrema
       Fs: time-sorted extrema values/heights
       Ms: time-sorted vector tracking types of extrema (1 for max, 0 for min)
       
pruning: removes a set of small leaves from trees, and denoises the corresponding time series. Options are included for paths with long flat portions, i.e. plateaus.
[tst2,Dcheck] = pruning(tst1,Delta,type) 

       Inputs: 
       tst1: a time_series_tree object (see time_series_tree for details)
       Delta: the cutoff length for leaves
       type: selection of which pruning algorithm to use 
       
       Outputs:
       tst2: resulting (pruned) time_series_tree object
       Dcheck: 

Compare_Prune_to_FFT: script for comparison of signal processing methods. Generates data, approximates with the FFT and with tree pruning; compares MSE for similar numbers
of parameters.

Compare_Wavelets: script which generates data, and approximates it using wavelets and tree pruning. Compares MSE of each approximation for similar numbers of parameters. 
Requres Wavelab 850.

EdgeDectectionWithPruning: [M,BWm] = PruneImage(data,map,process,Delta0,bwthres)
Uses tree pruning to detect edges in an image.

    Inputs: data: the image
    map: auxillary to help image be read correctly; optional.
    process: option to pre-process image
    Delta0: threshold for pruning leaves
    bwthres: threshold for black/white conversion
    
    Outputs: M: resulting image
    BWm: black and white version of resulting image
    
 
Get_H: Script solving the problem of finding values for the function h(x) which meets the Gordin/Lifsic criteria.  Note that this is written for R.

Auxillary functions needed to run the code described above:
------------------------------------------------------------

CompareError: compares two input time series (t, X) and (t2, X2), outputs the MSE between the two and W is an optional weights vector. 

KidsArrayFromTree: [Kids2] = KidsArrayFromTree(T1,T2)

      Inputs: 
      T1: is a "direct" tree vector with -1 in place of any degree two nodes.
      T2: includes all the degree two nodes' parent-child links
      
      Output: 
      parent-pointer type array of kids for each node

PickTrimTypeWOdeg2Nodes: Picks whether or not to keep a degree two node in a trim based on the error level but doesn't consider parantage of degree-two nodes.

PickTrimType: Picks whether or not to keep a degree two node in a trim based on the error level.

TopoDelete: Takes a list of nodes that are to be deleted, and the current trees, removes them, outputs the resulting trees. IMPORTANT: the tree outputs won't be plottable! They have not yet been renamed. This must be used alongside RenameNodes.

RenameNodes: Takes care of the node renaming process after the to-be-deleted nodes are selected by TopoDelete.

Fn2Excursion: 

        Inputs:
        X(t) is the function to be analysed;
        Outputs:
        X1(t1) is the longest excursion to be found in X(t), shifted to
        begin at the origin.

time_series_tree: script which defines a class of time_series_tree objects, which store data about trees
 and their corresponding Harris paths in one place, and which have methods defined on them for visualizing, as well as for pruning, resulting in modified time_series_tree objects.

tokunagap: generates a Tokunaga self-similar tree with parameters a,c and order omg.

ewing: Finds local extrema of the function f(t) and their exceedance values.

skeleton: auxilliary to tokunagap which generates the skeleton (no side branching) of a tree with order omg.

treerank: auxilliary to tokunagap.

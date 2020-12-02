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
[tst2] = pruning(tst1,Delta,type) 

       Inputs: 
       tst1: a time_series_tree object (see time_series_tree for details)
       Delta: the cutoff length for leaves
       type: selection of which pruning algorithm to use 
       
       Outputs:
       tst2: resulting (pruned) time_series_tree object
       
Compare_Prune_to_FFT: script for comparison of signal processing methods. Generates data, approximates with the FFT and with tree pruning; compares MSE for similar numbers
of parameters.

Compare_Wavelets: script which generates data, and approximates it using wavelets and tree pruning. Compares MSE of each approximation for similar numbers of parameters. 
Requres Wavelab 850.

EdgeDectectionWithPruning: Uses tree pruning to detect edges in an image.
[M,BWm] = PruneImage(data,map,process,Delta0,bwthres)

    Inputs: 
    data: the image
    map: auxillary to help image be read correctly; optional.
    process: option to pre-process image
    Delta0: threshold for pruning leaves
    bwthres: threshold for black/white conversion
    
    Outputs: 
    M: resulting image
    BWm: black and white version of resulting image
    
 
Get_H: Script solving the problem of finding values for the function h(x) which meets the Gordin/Lifsic criteria.  Note that this is written for R.

Auxillary functions needed to run the code described above:
------------------------------------------------------------

CompareError: compares two input time series and outputs the MSE between the two. 
[MSE,Dev,TE,WSE] = CompareError(t, X, t2, X2, W)

       Inputs:
       t: time vector for first series
       X: values of first series
       t2: time vector for second series
       X2: values of second series
       W: optional weight vector
       
       Outputs:
       MSE: mean squared error between the two time series
       Dev: vector giving the error at each point
       TE: total error, sum of Dev
       WSE: weighted MSE if W was provided

KidsArrayFromTree: creates parent-pointer using vectors of degree-two and non-degree-two parantage
[Kids2] = KidsArrayFromTree(T1,T2)

      Inputs: 
      T1: is a "direct" tree vector with -1 in place of any degree two nodes.
      T2: includes all the degree two nodes' parent-child links
      
      Outputs: 
      Kids2: parent-pointer type array of kids for each node

PickTrimTypeWOdeg2Nodes: Picks whether or not to keep a degree two node in a trim based on the error level but doesn't consider parantage of degree-two nodes.
[ Direct, AddDelete ] = PickTrimTypeWOdeg2Nodes(MinLeaf,Parent,Orig,tOrig,V,T1,Deg2,ErrChoice,Direct)

       Inputs:
       MinLeaf: index of the smallest leaf
       Parent: index of parent of the smallest leaf
       Orig: most recent version of the approximation values
       tOrig: time vector for most recent version of approximation
       V: node tracking vector (links time series indices to tree indices)
       T1: parent pointer vector of tree
       Deg2: logical vector indicating whether a node is degree two (yes 1, no 0)
       ErrChoice: logical (adjust based on local error 1, otherwise 0)
       Direct: logical (removal of degree-two node in previous trim 1, no such removal 0)
       
       Outputs:
       Direct: logical (a degree two node will be removed in this trim 1, otherwise 0)
       AddDelete: list of indices of nodes to be removed

PickTrimType: Picks whether or not to keep a degree two node in a trim based on the error level.
function [ Direct ] = PickTrimType(MinLeaf,Parent,Orig,tOrig,V)

       Inputs:
       MinLeaf: index of the smallest leaf
       Parent: index of parent of the smallest leaf
       Orig: most recent version of the approximation values
       tOrig: time vector for most recent version of approximation
       V: node tracking vector (links time series indices to tree indices)
       
       Outputs:
       Direct: indicates whether to delete the degree two node (yes 1, no 0)

TopoDelete: Takes a list of nodes that are to be deleted, and the current trees, removes them, outputs the resulting trees. IMPORTANT: the tree outputs won't be plottable! They have not yet been renamed. This must be used alongside RenameNodes.
[ T1B, T2B, Delete ] = TopoDelete( Delete,T1,T2,Parent,Sibling)

       Inputs:
       Delete: list of nodes to be deleted
       T1: parent pointer without degree two nodes
       T2: parent pointer with degree two nodes
       Parent: parents of nodes to be deleted
       Sibling: siblings of nodes to be deleted
       
       Outputs:
       T1B: new parentage without degree two nodes
       T2B: new parentage with degree two nodes
       Delete: passes Delete out to RenameNodes (unchanged)

RenameNodes: Takes care of the node renaming process after the to-be-deleted nodes are selected by TopoDelete.
[ NewT1,NewT2,VSort ] = RenameNodes( T1,T2,Lt,Delete,Deg2,VSort,root)

       Inputs:
       T1: parent pointer vector without degree two nodes
       T2: parent pointer vector with degree two nodes
       Lt: lengths of edges
       Delete: vector of nodes to be deleted
       Deg2: vector indicating whether nodes are degree two
       VSort: list of node names
       root: index of root node
       
       Outputs:
       NewT1: new parent pointer vector without degree two nodes
       NewT2: new parent pointer vector with degree two nodes
       VSort: list of node names

Fn2Excursion: generates the minimal excursion of a function
[t1,X1] = Fn2Excursion(t,X)

        Inputs:
        t: time vector of series
        X: values of series
        
        Outputs:
        t1: time vector of minimal excursion (shifted to begin at the origin)
        X1: values of minimal excursion

time_series_tree: script which defines a class of time_series_tree objects, which store data about trees and their corresponding Harris paths in one place, and which have methods defined on them for visualizing, as well as for pruning, resulting in modified time_series_tree objects.

tokunagap: generates a Tokunaga self-similar tree with parameters a,c and order omg.
[SK]=tokunagap(a,c,omg)

       Inputs:
       a: tree parameter
       c: tree parameter
       omg: tree order
       
       Outputs:
       SK: a Tokunaga self similar tree (parent-pointer vector)

ewing: Finds local extrema of the function f(t) and their exceedance values.
[w,wl,wr,time,ext,s,ind]=ewing(f,t,n)

       Inputs:
       f: values of function to analyze
       t: time vector of f
       n: width of plateau to consider
       
       Outputs:
       w: sum of wl and wr
       wl: value of left exceedance
       wr: value of right exceedance
       time: times of extrema
       ext: values of extrema
       s: signs of extrema (max 1, min -1)
       ind: indices of extrema

skeleton: auxilliary to tokunagap which generates the skeleton (no side branching) of a tree with order r.
[TR]= skeleton(r)

       Inputs:
       r: order of tree
       
       Outputs:
       TR: skeleton (principal branching structure)

treerank: auxilliary to tokunagap.
[RNK]= treerank(r)

       Inputs:
       r: order of tree
       
       Outputs:
       RNK: array giving the numbers of edges of various orders
       

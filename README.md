# phd-dissertation-code
Repository for all the code referenced in my thesis. To be defended in November 2020.

Programs for transforming data between function-space and tree-space: 

trees to functions is performed by the Harris Path (harrispath), and functions to trees by Level Set Tree (level_set_tree).

Pruning programs remove a set of small leaves from trees, and denoise the corresponding time series. Options are included for paths with long flat portions, i.e. plateaus.
This is done by ThreeOptionTrim

Comparisons in terms of mean-squared error are performed relative to the FFT and to wavelets. ()
The wavelet program requires installation of Wavelab850.

Edge detection is performed using pruning ()

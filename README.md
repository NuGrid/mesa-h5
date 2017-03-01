The HDF5 framework for MESA
===========================

This is the new implementation of HDF5 support for MESA based on the former SE format.
It provides mppnp compatibilty.
Created by Jean-Claude Passy.
Tested by Falk Herwig, Umberto Battino, and Pavel Denissenkov.
(c) 2016
For questions/comments, please contact Jean-Claude Passy (jean-claude.passy [at] tuebingen.mpg.de) or Falk Herwig (fherwig [at] uvic.ca).


INSTALLATION:
Theses have been tested with MESA v9331, so please use that one. The different steps you must follow are:

1) download MESA v9331;
2) copy the new makefile_header in $MESA_DIR/utils;
3) install MESA;
4) copy the run_star_extras.f and mesa_hdf5_*.inc files into the src folder in your work directory. You can use the inlist and history/profile lists provided here, but yours should work as well;
5) clean, compile, and enjoy!


NOTES:
In order to keep the run_star_extras clean and more easily readable, I used include files:
- mesa_hdf5_routines.inc: these are all the routines. You should not need to modify them (except in one case that I will describe below);
- mesa_hdf5_params.inc: the parameters that were initially in the SE namelist, part of the inlist. You can modify them as you wish.

The module takes care of extra history and profiles columns created by the user (see example in run_star_extras). Also it allows you to change the name of a few variables in case some of them are different between SE and MESA. If you want to remove/add some of them, you must modify the routines change_history_names and change_profile_names in mesa_hdf5_routines.inc. 


WARNING:
Keep in mind that I used include files, so if you modify them you need to clean AND make for the compiler to see the changes. Doing './mk’ only will NOT work.


DISCUSSION:
       - isomeric_state: the data is currently hard-coded (with only values 1). What should we do with these?
       - abundances and ISO_MASSF:  the abundances are now written both in the SE_DATASET and in ISO_MASSF datasets. Should we keep them in both?
       - dynamically changing net (CRITICAL): there should be a correspondance between A,Z, and ISO_MASSF. The problem is that there is one ISO_MASSF per cycle, but only one (A,Z) per HDF5 data. So in case the net changes dynamically, the code might crash or, at best, the post-processing will be wrong. My suggestion is to have an (A,Z) dataset per cycle, which might require some changes in mppnp. But I think that it is the most viable solution.
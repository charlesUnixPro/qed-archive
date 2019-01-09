# Multics 12.5 QED

## Introduction

These files were supplied to me by Charles Anthony
(charles.unix.pro AT gmail.com).

Charles Anthony:

The files (segments, actually) were copied from a running Multics instance. The
system was running Multics R12.6f; which was derived from R12.5, the last
"official" release of Multics.

The 12.5 release is available on bitsavers
(http://bitsavers.trailing-edge.com/bits/Honeywell/multics/tape/). The 12.6f
qedx is unchanged from 12.5.


## Contents

multics/documentation/info\_segments/qedx.errors.info  
multics/documentation/info\_segments/qedx.info  
multics/documentation/info\_segments/qedx\_.info  
&nbsp;&nbsp;&nbsp;&nbsp;  The Multics help system documentation on qedx.

documentation/subsystem/bce/qedx.info  
&nbsp;&nbsp;&nbsp;&nbsp;  Documentation about using
qedx in the Boot Command Environment, an obsolete Multics boot subsystem.

documentation/subsystem/linus/qedx.info   
&nbsp;&nbsp;&nbsp;&nbsp;    Documentation about using
qedx in the Multics Logical Inquiry and Update System, a database utility.

library\_dir\_dir/include/qedx\_buffer\_io\_info.incl.pl1  
library\_dir\_dir/include/qedx\_info.incl.pl1  
library\_dir\_dir/include/qedx\_internal\_data.incl.pl1  
&nbsp;&nbsp;&nbsp;&nbsp;   the "include" files for qedx.

library\_dir\_dir/system\_library\_1/maps.archive/bound\_qedx\_.bindmap   
&nbsp;&nbsp;&nbsp;&nbsp;   memory map file produced by the object file
binder, showing the size and offsets of the various compoents of the executable
image.

library\_dir\_dir/system\_library\_1/object/bound\_qedx\_.archive/bound\_qedx\_.bind  
&nbsp;&nbsp;&nbsp;&nbsp;    the binder control file; it controls the layout and
entry point specifications of the qedx executable.

library\_dir\_dir/system\_library\_1/source/bound\_bce\_paged.s.archive/bootload\_qedx.pl1     
&nbsp;&nbsp;&nbsp;&nbsp;    the source code for the minimal qedx version used
in the bootload environment.

library\_dir\_dir/system\_library\_1/source/bound\_qedx\_.s.archive/  
&nbsp;&nbsp;&nbsp;&nbsp;     contains the source code for the qedx editor.   
&nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp;     check\_entryname\_.pl1  
&nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp;     get\_addr\_.pl1    
&nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp;     qedx.pl1  
&nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp;     edx\_util\_.pl1         
&nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp;     qedx\_.pl1      
&nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp;     search\_file\_.pl1  


## Building

To build and use them, you  need a running Multics. (A Public Access Multics can be found at https://ban.ai/multics/. Information for running your own instance can be found at http://swenson.org/multics\_wiki/index.php?title=Main\_Page.

To build, logon to Multics and

    cd qedx
    cwd qedx
    lf -lb h bound_qedx_.s.archive
    lf -lb h bound_qedx_.archive
    ac x bound_qedx_.s.archive
    pl1 ([segs *.pl1]) -ot
    ac ud bound_qedx_ [segs *]
    ac x bound_qedx_ bound_qedx_.bind
    bind bound_qedx_
    [pwd]>qedx


_cd_ is create directory.

_cwd_ is change working directory.

_lf_ is "library fetch"; _-lb h_ specifies the "hardcore" (roughly equivalent to kernel plus "/bin"). The two commands copy the archive files containing the source (.s) and object archives from the system library to the working directory.  The directory now  has

    r w   15  bound_qedx_.archive
    r w   43  bound_qedx_.s.archive

The object archive is needed as it contains the "bind" file which controls how the objects are bound together and which entry points are exposed.

_ac_ is the archive command; this extracts all of the segments in the bound\_qedx\_.s archive; more of less equivalent to "tar x bound\_qedx\_.archive", The directory now has

    r      6  search_file_.pl1
    rew   22  qedx_.pl1
    r w    4  qedx.pl1
    rew    4  get_addr_.pl1
    rew    8  edx_util_.pl1
    r      1  check_entryname_.pl1
    r w   15  bound_qedx_.archive
    r w   43  bound_qedx_.s.archive


_pl1 ([segs \*.pl1]) -ot_ compiles the source; ("cc -o \*.c").

    re     1  check_entryname_
    re     3  edx_util_
    re     1  get_addr_
    re     2  qedx
    re     8  qedx_
    re     2  search_file_
    r      6  search_file_.pl1
    rew   22  qedx_.pl1
    r w    4  qedx.pl1
    rew    4  get_addr_.pl1
    rew    8  edx_util_.pl1
    r      1  check_entryname_.pl1
    r w   15  bound_qedx_.archive
    r w   43  bound_qedx_.s.archive

_ac ud bound_\__qedx_\_ _[segs \*]_ updates ("u") the contents of the object archive with the newly compiled objects and deletes them ("d").

_bind bound_\__qedx_\_ binds together the object segments in bound\_qedx\_.archive and creates a bound\_qedx\_ segment.

    rew   14  bound\_qedx\_
              search\_file\_
              qedx\_
              check\_entryname\_
              qedx
              qx
    r      6  search\_file\_.pl1
    rew   22  qedx\_.pl1
    r w    4  qedx.pl1
    rew    4  get\_addr\_.pl1
    rew    8  edx\_util\_.pl1
    r      1  check\_entryname\_.pl1
    r w   15  bound\_qedx\_.archive
    r w   43  bound\_qedx\_.s.archive
    
The segment _bound_\__qedx_\_" has been created, with the entry points _search_\__file_\_, _qedx_\_, _check_\__entryname_\_, _qedx_ and _qx_. By convention, function names have a trailing underscore, command names do not.

_[pwd]_\>_qedx_ ("./qedx") runs the executes the _bound_\__qedx_\_ segment using the _qedx_ entrypoint.


#### Last Updated

Wed Jan  9 22:06:12 UTC 2018

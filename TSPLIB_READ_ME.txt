
========================================================================
|                                                                      |
|                           TSPLIB  1.0                                |
|                                                                      |
|                     Version of May 23, 1990                          |
|                                                                      |
|                         Gerhard Reinelt                              |
|                     Institut fuer Mathematik                         |
|                      Universitaet Augsburg                           |
|                                                                      |
========================================================================


========================================================================
|                                                                      |
| 1. INTRODUCTION                                                      |
|                                                                      |
========================================================================

This directory contains TSPLIB, a library of traveling salesman and
related problem instances. All of the data is publicly available.

The purpose of this documentation is to define the format of the data 
files and to specify the computation of distances if they are not given 
explicitly by a matrix. All distances are required to be integral.

At present, the following problem classes are available.

1. Symmetric traveling salesman problems (TSP)

   Given a set of n nodes and distances for each pair of nodes find a
   roundtrip visiting each node exactly once whose total length is
   minimum.

2. Capacitated vehicle routing problems (CVRP)

   We are given n-1 nodes, one depot and distances between nodes and to
   the depot. All nodes have demands which can be satisfied by the depot.
   For delivery to the nodes there are trucks with identical capacities
   available. The task is to find tours for the trucks to satisfy the
   demands at the nodes such that no truck exceeds its capacity and the
   sum of the tour lengths is minimum. The number of trucks is not
   specified. Each tour visits a subset of the nodes and starts and
   terminates at the depot. 
   Remark: In some data files we have a collection of alternate depots.
   A CVRP is then given by selecting one of these depots.

In the current version all problems are defined on a complete graph. 
Asymmetric TSPs as well as problems on sparse graphs can also be specified
using our format.

==========================================================================
|                                                                        |
| 2. THE FILE FORMAT                                                     |
|                                                                        |
==========================================================================

It is our intention to not only distribute problem data but also 
solutions, Lagrangean multipliers or special subgraphs which might be of 
interest. To this end a special file format was chosen which also improves
readability of the data files. In this section we describe the basic 
version. Future enhancements (especially for the definition of other 
problem classes) are possible.

Basically, each file consists of two parts: a specification part and a 
data part. The specification part contains information on the file format 
and on its contents. The data part contains explicit data.

==========================================================================
| 2.1. The specification part                                            |
==========================================================================

All entries in this section consist of lines of the form

-----------------------------------------------
<keyword> : <value>
-----------------------------------------------

where <keyword> denotes an alphanumerical keyword and <value> denotes 
alphanumerical or numerical data. The terms <string>, <integer> and <real>
denote character string, integer or real data, respectively. Integer and
real numbers are given in free format. The order of specification of the 
keywords in the data file is arbitrary (in principle), but has to be 
logical, i. e. whenever a keyword is specified then all necessary 
information for the correct handling of the keyword has to be known. 

Below we give a list of all available keywords not in alphabetical but in 
a (more or less) logical order.

-----------------------------------------------
NAME : <string>
-----------------------------------------------

Used as an identification of the data file.

-----------------------------------------------
TYPE : <string>
-----------------------------------------------

Specifies the type of the data file. This is sometimes necessary to allow
for a correct interpretation of the data. Possible types are

    TSP         : Traveling salesman problem data

    CVRP        : Capacitated vehicle routing problem data

    TOUR        : A collection of tours

    GRAPH       : The file specifies a graph

-----------------------------------------------
COMMENT : <string>
-----------------------------------------------

An additional comment on the data. 

-----------------------------------------------
DIMENSION : <integer>
-----------------------------------------------

Specifies the number of nodes of a TSP, resp. the number of nodes and
depots for a CVRP.

-----------------------------------------------
CAPACITY : <integer>
-----------------------------------------------

Specifies the capacity of a truck in a capacitated vehicle routing
problem of type CVRP.

-----------------------------------------------
GRAPH_TYPE : <string>
-----------------------------------------------

Has to be used to give more information if the file contains a graph.
Valid values are

    COMPLETE_GRAPH : The underlying graph is complete

    SPARSE_GRAPH   : The file contains a sparse subgraph

The default value of this entry is COMPLETE_GRAPH. 

-----------------------------------------------
EDGE_TYPE : <string>
-----------------------------------------------

Specifies whether the edges are directed or undirected. The corresponding 
values are

    UNDIRECTED : The edges are not directed

    DIRECTED   : The edges(arcs) are directed

The default value of this entry is UNDIRECTED.

-----------------------------------------------
EDGE_WEIGHT_TYPE : <string>
-----------------------------------------------

Specifies how the edge weights are given (if the edges are weighted).
Values are

    EXPLICIT : Weights are listed explicitly in the corresponding section

    EUD_2D   : Weights are the 2-dimensional Euclidean distance

    EUD_3D   : Weights are the 3-dimensional Euclidean distance

    MAX_2D   : Weights are the 2-dimensional maximum distance

    MAX_3D   : Weights are the 3-dimensional maximum distance

    MAN_2D   : Weights are the 2-dimensional Manhattan distance

    MAN_3D   : Weights are the 3-dimensional Manhattan distance
 
    GEO      : Weights are the geographical distance

    ATT      : Special distance function for att48 and att532

    SPECIAL  : There is a special distance function documented elsewhere

-----------------------------------------------
EDGE_WEIGHT_FORMAT : <string>
-----------------------------------------------

Describes the format of the edge weights if they are given explicitly.
Possible values are

    FULL_MATRIX    : Weights are given by a full matrix

    UPPER_ROW      : Weights are given by an upper triangular matrix
                     (rowwise without diagonal entries)

    LOWER_ROW      : Weights are given by a lower triangular matrix
                     (rowwise without diagonal entries)

    UPPER_DIAG_ROW : Weights are given by an upper triangular matrix
                     (rowwise including diagonal entries)

    LOWER_DIAG_ROW : Weights are given by a lower triangular matrix
                     (rowwise including diagonal entries)

    UPPER_COL      : Weights are given by an upper triangular matrix
                     (columnwise without diagonal entries)

    LOWER_COL      : Weights are given by a lower triangular matrix
                     (columnwise without diagonal entries)

    UPPER_DIAG_COL : Weights are given by an upper triangular matrix
                     (columnwise including diagonal entries)

    LOWER_DIAG_COL : Weights are given by a lower triangular matrix
                     (columnwise including diagonal entries)

    WEIGHT_LIST    : Weights are specified in a list

-----------------------------------------------
EDGE_DATA_FORMAT : <string>
-----------------------------------------------

Specifies how edges are given if the file contains a subgraph. Values are

    ADJ_LIST       : The graph is given as an adjacency list

    EDGE_LIST      : The graph is given as an edge list

-----------------------------------------------
NODE_TYPE : <string>
-----------------------------------------------

Specifies whether the nodes have weights associated with them. Values are

    WEIGHTED_NODES   : The nodes have associated weights

    UNWEIGHTED_NODES : The nodes are unweighted

The default value is UNWEIGHTED_NODES.

-----------------------------------------------
NODE_COORD_TYPE : <string>
-----------------------------------------------

Specifies whether coordinates are associated with each node (which
may e. g. be used for graphical display or distance computations).
Values are

    TWOD_COORDS   : The nodes have 2-dimensional coordinates

    THREED_COORDS : The nodes have 3-dimensional coordinates

    NO_COORDS     : The nodes do not have associated coordinates

The default value is NO_COORDS.

Coordinates may also receive offsets and scalings. These can be used
e. g. for modelling different speed of a machine in x - and y -directions.
The following six keywords specify offset and scaling for the 
coordinates. 

-----------------------------------------------
COORD1_OFFSET : <real>
-----------------------------------------------

-----------------------------------------------
COORD1_SCALE : <real>
-----------------------------------------------

-----------------------------------------------
COORD2_OFFSET : <real>
-----------------------------------------------

-----------------------------------------------
COORD2_SCALE : <real>
-----------------------------------------------

-----------------------------------------------
COORD3_OFFSET : <real>
-----------------------------------------------

-----------------------------------------------
COORD3_SCALE : <real>
-----------------------------------------------

The offsets (default values 0.0) specify numbers that are added to the 
respective coordinates. After addition of the offsets the coordinates 
are multiplied by the respective scaling factors (default values 1.0).

-----------------------------------------------
DISPLAY_DATA_TYPE : <string>
-----------------------------------------------

Specifies how a graphical display of the nodes can be obtained. Values 
are

    COORD_DISPLAY : Display is generated from the node coordinates

    TWOD_DISPLAY  : Explicit 2-dimensional coordinates are given

    NO_DISPLAY    : No graphical display is possible

The default value is NO_DISPLAY.

-----------------------------------------------
EOF 
-----------------------------------------------

Terminates the input data. This entry is optional.

==========================================================================
| 2.2. The data part                                                     |
==========================================================================

Depending on the choice of specifications some additional data (in 
specified format) may be required. These data are given in corresponding
data sections which follow the specification part. Each data section is
started with a corresponding keyword. The length of the section is either 
implicitly known from the format specification or the section is 
terminated by special end-of-section terminators.

-----------------------------------------------
NODE_COORD_SECTION 
-----------------------------------------------

Node coordinates are given in this section. Each line is of the form

<integer> <real> <real>

if NODE_COORD_TYPE is TWOD_COORDS, or

<integer> <real> <real> <real>

if NODE_COORD_TYPE is THREED_COORDS. The integers give the number of the
respective nodes. The real numbers give the associated coordinates.

-----------------------------------------------
DEPOT_SECTION 
-----------------------------------------------

Contains a list of possible alternate depot nodes. This list is terminated
by a -1.

-----------------------------------------------
DEMAND_SECTION 
-----------------------------------------------

The demands of all nodes of a CVRP are given in the form (per line)

<integer> <integer>

The first integer specifies a node number, the second integer gives its
demand. The depot nodes must also occur in this section. 
Their demand is 0.

-----------------------------------------------
DISPLAY_DATA_SECTION 
-----------------------------------------------

If DISPLAY_DATA_TYPE is TWOD_DISPLAY then the 2-dimensional coordinates 
from which a display can be generated are given in the form (per line)

<integer> <real> <real>

The integers give the number of the respective nodes. The real numbers
give the associated coordinates.

-----------------------------------------------
NODE_WEIGHT_SECTION 
-----------------------------------------------

The non-zero node weights are given in the form (per line)

<integer> <real>

The integers give the number of the respective nodes. The real numbers
give the associated node weight. This section is terminated by a negative
node number.

-----------------------------------------------
TOUR_SECTION 
-----------------------------------------------

A collection of tours is specified in this section. Each tour is given by
a list of integers giving the sequence in which the nodes are visited in
this tour. Every such tour is terminated by a -1.

-----------------------------------------------
EDGE_DATA_SECTION 
-----------------------------------------------

Edges of a graph are specified in either of the two formats allowed in 
the EDGE_DATA_TYPE entry. If the type is EDGE_LIST then the edges are 
given as a sequence of lines of the form

<integer> <integer>

giving the terminal nodes of every edge. The list is terminated by a 
negative number.

If the type is ADJ_LIST then the section consists of a list of adjacency
lists for nodes. The adjacency list of a node is specified as

<integer> <integer> .... <integer> -1 

where the first integer gives the respective node and the following 
integers (terminated by -1 ) give adjacent nodes. The list of adjacency 
lists is terminated by an additional -1 .

-----------------------------------------------
EDGE_WEIGHT_SECTION 
-----------------------------------------------

The edge weights are given in the format specified by the
EDGE_WEIGHT_FORMAT entry. The matrix formats are self-explaining and  
their length is implicitly given. If the format WEIGHT_LIST is 
specified then a sequence of lines of the form

<integer> <integer> <integer>

has to be given where the first two integers give the terminal nodes of
the edge and the third integer is the edge weight. The list is 
terminated by a -1 .

==========================================================================
|                                                                        |
| 3. DISTANCE COMPUTATIONS                                               |
|                                                                        |
==========================================================================

In this section we desribe the distance functions that have to be 
available for the respective entries of EGDE_WEIGHT_TYPE. We always list a
(simplified) C-implementation for computing the distances from the input 
coordinates. All computations involving floating-point numbers are carried
out in double precision arithmetic. The integers are assumed to be 
represented in 32-bit words. Note that all distances are required to be 
integral. Therefore we have to apply rounding when necessary. All 
explicitly given distances have to be already integral.

--------------------------------------------------------------------------
| 3.1 Euclidean distance                                                 |
--------------------------------------------------------------------------

If we have the edge weight type EUC_2D of EUC_3D then the nodes must have 
two, three resp., coordinates associated with them. These  coordinates 
are floating point numbers. Let x[i], y[i], and z[i] be the coordinates of
a node i .

In the 2-dimensional case the distance between two points i and j is 
computed as follows:

    xd = x[i] - x[j];
    yd = y[i] - y[j];
    dij = aint( sqrt( xd*xd + yd*yd) + 0.5 );

In the 3-dimensional case we have:

    xd = x[i] - x[j];
    yd = y[i] - y[j];
    zd = z[i] - z[j];
    dij = aint( sqrt( xd*xd + yd*yd + zd*zd) + 0.5 );

--------------------------------------------------------------------------
| 3.2 Manhattan distance                                                 |
--------------------------------------------------------------------------

Distances are given as Manhattan distances if the edge weight type is
MAN_2D of MAN_3D. They are computed as follows.

2-dimensional case:

    xd = abs( x[i] - x[j] );
    yd = abs( y[i] - y[j] );
    dij = aint( xd + yd + 0.5 );

3-dimensional case:

    xd = abs( x[i] - x[j] );
    yd = abs( y[i] - y[j] );
    zd = abs( z[i] - z[j] );
    dij = aint( xd + yd + zd + 0.5 );

Note that a scaling factor for the coordinates may be given in the input
data file. This factor has to be applied to the coordinates before doing
the computations.

--------------------------------------------------------------------------
| 3.3 Maximum distance                                                   |
--------------------------------------------------------------------------

Maximum distances have to be computed if the edge weight type is MAX_2D 
or MAX_3D. 

2-dimensional case:

    xd = abs( x[i] - x[j] );
    yd = abs( y[i] - y[j] );
    dij = max( aint( xd + 0.5 ), aint( yd + 0.5) ) );

3-dimensional case:

    xd = abs( x[i] - x[j] );
    yd = abs( y[i] - y[j] );
    zd = abs( z[i] - z[j] );
    dij = max( aint( xd + 0.5 ), aint( yd + 0.5), aint( zd + 0.5) );

Again we may have scaling factors which have to be applied to the 
coordinates before doing the computations.

--------------------------------------------------------------------------
| 3.4 Geographical distance                                              |
--------------------------------------------------------------------------

If the traveling salesman problem is a geographical problem then the nodes
correspond to points on the earth and the distance between two points is 
their distance on the idealized circle with radius 6378.388 kilometers. 
The node coordinates give the geographical latitude and longitude of the 
corresponding point on the earth. Latitude and longitude are given in the
form  DDD.MM where DDD are the degrees and MM the minutes. A positive 
latitude is assumed to be 'North', negative latitude means 'South'. 
Positive longitude means 'East', negative latitude is assumed to be 
'West'. For example, the input coordinates for Augsburg would be 48.23 
and 10.53, meaning 48.23 North and 10.53 East.

Let  x[i] and  y[i] be the input for city i in the above format. In a 
first step this input is converted to geographical latitude and
longitude given in radian.

    PI = 3.141592;

    deg = aint( x[i] );
    min = x[i] - deg;
    latitude[i] = PI * (deg + 5.0 * min) / 3.0 ) / 180.0;
    deg = aint( y[i] );
    min = y[i] - deg;
    longitude[i] = PI * (deg + 5.0 * min) / 3.0 ) / 180.0;

The distance between two nodes i and j in kilometers is then computed as
follows:

    RRR = 6378.388;

    q1 = cos( longitude[i] - longitude[j] );
    q2 = cos( latitude[i] - latitude[j] );
    q3 = cos( latitude[i] + latitude[j] );
    dd = trunc( (RRR * acos( 0.5*((1.0+q1)*q2 - (1.0-q1)*q3) ) + 1.0);

--------------------------------------------------------------------------
| 3.5 Pseudo-Euclidean distance                                          |
--------------------------------------------------------------------------

The edge weight type ATT corresponds to a special 'pseudoeuclidean' 
distance function. Let x[i] and y[i] be the coordinates of a node i. The 
distance between two points i and j is computed as follows:

    xd = x[i] - x[j];
    yd = y[i] - y[j];
    rij = sqrt( (xd*xd + yd*yd)/10.0 );
    tij = aint( rij );
    if (tij<rij) dij = tij + 1;
    else dij = tij;

--------------------------------------------------------------------------
| 3.6 Verification                                                       |
--------------------------------------------------------------------------

To verify correctness of implementations of these distance functions we
give the length of the 'canonical' tour 1,2,3,....,n for three problems:

  Euclidean distance
  ------------------

  The canonical tour for the problem pcb442 has length 221440.

  Geographical distance
  ---------------------

  The canonical tour for the problem gr666 has length 423710.

  ATT distance
  ---------------------

  The canonical tour for the problem att532 has length 309636.

==========================================================================
|                                                                        |
| 4. LIST OF FILES                                                       |
|                                                                        |
==========================================================================

We give a list of the data files that are available in TSPLIB in the 
above format. The number before the '.' in the file name always gives the
dimension of a problem, e. g. the file gr666.tsp  contains data of a TSP
on 666 cities.

The following Traveling Salesman problem data files are available:

   ali535.tsp      gil262.tsp      kroE100.tsp     rat783.tsp 
   att48.tsp       gr120.tsp       lin105.tsp      rat99.tsp 
   att532.tsp      gr137.tsp       lin318.tsp      rd100.tsp 
   bayg29.tsp      gr17.tsp        p654.tsp        rd400.tsp 
   bays29.tsp      gr202.tsp       pcb1173.tsp     rl11849.tsp 
   bier127.tsp     gr21.tsp        pcb3038.tsp     rl1304.tsp 
   d1291.tsp       gr229.tsp       pcb442.tsp      rl1323.tsp 
   d1655.tsp       gr24.tsp        pr1002.tsp      rl1889.tsp 
   d198.tsp        gr431.tsp       pr107.tsp       rl5915.tsp 
   d2103.tsp       gr48.tsp        pr124.tsp       rl5934.tsp 
   d493.tsp        gr666.tsp       pr136.tsp       st70.tsp 
   d657.tsp        gr96.tsp        pr144.tsp       u1060.tsp 
   dantzig42.tsp   hk48.tsp        pr152.tsp       u1432.tsp 
   dsj1000.tsp     kroA100.tsp     pr226.tsp       u159.tsp 
   eil101.tsp      kroA150.tsp     pr2392.tsp      u1817.tsp 
   eil51.tsp       kroA200.tsp     pr264.tsp       u2152.tsp 
   eil76.tsp       kroB100.tsp     pr299.tsp       u2319.tsp 
   fl1400.tsp      kroB150.tsp     pr439.tsp       u574.tsp 
   fl1577.tsp      kroB200.tsp     pr76.tsp        u724.tsp 
   fl3795.tsp      kroC100.tsp     rat195.tsp      vm1084.tsp 
   fl417.tsp       kroD100.tsp     rat575.tsp      vm1748.tsp 

Some optimum tours are also provided. The corresponding files have names
of the form <probname>.opt.tour.

Capacitated vehicle routing problem data is contained in the files:

   att48.vrp       eil30.vrp       eil7.vrp        eilB76.vrp
   eil13.vrp       eil31.vrp       eilA101.vrp     eilC76.vrp
   eil22.vrp       eil33.vrp       eilA76.vrp      eilD76.vrp
   eil23.vrp       eil51.vrp       eilB101.vrp     gil262.vrp

Further special files contained in the library are:

   TSPLIB_VERSION:  Gives the current version of the library

   TSPLIB_VALUES:   Provides information on optimum solution values.

   TSPLIB_CHANGES:  Changes from one version to the next one are 
                    documented here.

   READ_ME:         The documentation you are currently reading


==========================================================================
|                                                                        |
| 5. FURTHER REMARKS                                                     |
|                                                                        |
==========================================================================

(1) The problem lin318 is originally a Hamiltonian path problem. You
    obtain this problem if you add the additional requirement that
    the edge from 1 to 214 is contained in the tour.

(2) Some data sets are referred to by different names in the literature.
    Below we give the corresponding names used in:

    M.Groetschel/O.Holland: Solution of large-scale symmetric travelling 
    salesman problems, Report No. 73 Schwerpunktprogramm der Deutschen 
    Forschungsgemeinschaft, Universitaet Augsburg, Augsburg, 1988, 
    to appear in Mathematical Programming

    and

    M.W.Padberg/G.Rinaldi: A branch & cut algorithm for the resolution of
    large-scale symmetric traveling salesman problems, IASI Research 
    Report 247, 1988

    TSPLIB              P/R             G/H
    ------------------------------------------

    att48              ATT048              -
    att532             ATT532              -
    dantzig42            -                42
    eil101             EIL10               -
    eil51              EIL08               -
    eil76              EIL09               -
    gil262             GIL249              -
    gr120                -               120
    gr137              GH137             137
    gr202              GH202             202
    gr229              GH229             229
    gr431              GH431             431
    gr666              GH666             666
    gr96               GH096              96
    hk48                 -                48H
    kroA100            KRO124            100A
    kroB100            KRO125            100B
    kroC100            KRO126            100C
    kroD100            KRO127            100D
    kroE100            KRO128            100E
    kroA150            KRO30               -
    kroB150            KRO31               -
    kroA200            KRO32               -
    kroB200            KRO33               -
    lin105             LK105               -
    lin318             LK318             318
    pr1002             TK1002              -
    pr107              TK107               -
    pr124              TK124               -
    pr136              TK136               -
    pr144              TK144               -
    pr152              TK152               -
    pr226              TK226               -
    pr2392             TK2392              -
    pr264              TK264               -
    pr299              TK299               -
    pr439              TK439               -
    pr76               TK076               -
    st70               KRO070             70

(3) The vehicle routing problems are also available in a TSP version. Here
    the depots are just treated as normal nodes.
    The problem gil262 originally contained two identical nodes. So one
    was eliminated.

(4) Contributions to this library are strongly encouraged. Potential 
    contributors should provide their data files in the above format and 
    contact

     Gerhard Reinelt
     Institut fuer Mathematik
     Universitaet Augsburg
     Universitaetsstrasse 8
     D-8900 Augsburg
     West Germany

     Tel.   (821) 598 2212
     Fax    (821) 598 2200

    or

     CRPC
     Center for Research on Parallel Computation
     Rice University
     P.O.Box 1892
     Houston, Texas 77251-1892
     USA

     Tel.    (713) 527 6077
     Fax     (713) 285 5136
     E-Mail  CRPC@RICE.EDU

    Comments or error messages should also be sent to these addresses.
    
(5) The library is available via BITNET as follows.


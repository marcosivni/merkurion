===========================================================
                 Merkurion Demo Tool. 
               Last built on 2016-04-23.
                     README.txt
===========================================================

... Beep, beep...
... Here are the command line examples...

Launch Windows PowerShell or execute cmd.exe
Launch Merkurion.exe on PowerShell

//Load the dataset
> LOAD dataset/cities.csv ELEMENTS 5507 DIMENSIONALITY 2 INTO citycoord METRIC EUCLIDEAN

//Create synopses
> CREATE SYNOPSIS dp FOR citycoord TYPE DISTANCE_PLOT USING DEFAULTS
> CREATE SYNOPSIS nnh FOR citycoord TYPE NEAREST_NEIGHBOR_HISTOGRAM USING DEFAULTS
> CREATE SYNOPSIS cdh FOR citycoord TYPE COMPACT_DISTANCE_HISTOGRAM USING DEFAULTS
> CREATE SYNOPSIS se FOR citycoord TYPE SEARCH_EVENT USING DEFAULTS

//Visualize synopses
> VIEW SYNOPSIS dp
> VIEW SYNOPSIS nnh
> VIEW SYNOPSIS cdh
> VIEW SYNOPSIS se

//For each query element in the test_range.csv file estimates
//the number of elements in a Range Query with radius 0.3 by using a synopsis 
ESTIMATE RANGE_SELECTIVITY RADIUS 0.3 FOR citycoord USING dp QUERY_ELEMENTS test/test_range.csv ELEMENTS 10 DIMENSIONALITY 2
ESTIMATE RANGE_SELECTIVITY RADIUS 0.3 FOR citycoord USING nnh QUERY_ELEMENTS test/test_range.csv ELEMENTS 10 DIMENSIONALITY 2
ESTIMATE RANGE_SELECTIVITY RADIUS 0.3 FOR citycoord USING cdh QUERY_ELEMENTS test/test_range.csv ELEMENTS 10 DIMENSIONALITY 2
ESTIMATE RANGE_SELECTIVITY RADIUS 0.3 FOR citycoord USING se QUERY_ELEMENTS test/test_range.csv ELEMENTS 10 DIMENSIONALITY 2
//For each query element in the test_knn.csv file estimates
//the initial radius of in a kNN Query with k = 15 by using a synopsis 
ESTIMATE KNN_RADIUS K 15 FOR citycoord USING dp QUERY_ELEMENTS test/test_knn.csv ELEMENTS 10 DIMENSIONALITY 2
ESTIMATE KNN_RADIUS K 15 FOR citycoord USING nnh QUERY_ELEMENTS test/test_knn.csv ELEMENTS 10 DIMENSIONALITY 2
ESTIMATE KNN_RADIUS K 15 FOR citycoord USING cdh QUERY_ELEMENTS test/test_knn.csv ELEMENTS 10 DIMENSIONALITY 2
ESTIMATE KNN_RADIUS K 15 FOR citycoord USING se QUERY_ELEMENTS test/test_knn.csv ELEMENTS 10 DIMENSIONALITY 2

//Executes a Range Query
EXECUTE RANGE_SELECT RADIUS 0.3 FOR citycoord USING dp QUERY_ELEMENTS test/test_range.csv ELEMENTS 10 DIMENSIONALITY 2
EXECUTE RANGE_SELECT RADIUS 0.3 FOR citycoord USING nnh QUERY_ELEMENTS test/test_range.csv ELEMENTS 10 DIMENSIONALITY 2
EXECUTE RANGE_SELECT RADIUS 0.3 FOR citycoord USING cdh QUERY_ELEMENTS test/test_range.csv ELEMENTS 10 DIMENSIONALITY 2
EXECUTE RANGE_SELECT RADIUS 0.3 FOR citycoord USING se QUERY_ELEMENTS test/test_range.csv ELEMENTS 10 DIMENSIONALITY 2

//Executes a kNN Query
EXECUTE KNN_SELECT K 15 FOR citycoord USING dp QUERY_ELEMENTS test/test_knn.csv ELEMENTS 10 DIMENSIONALITY 2
EXECUTE KNN_SELECT K 15 FOR citycoord USING nnh QUERY_ELEMENTS test/test_knn.csv ELEMENTS 10 DIMENSIONALITY 2
EXECUTE KNN_SELECT K 15 FOR citycoord USING cdh QUERY_ELEMENTS test/test_knn.csv ELEMENTS 10 DIMENSIONALITY 2
EXECUTE KNN_SELECT K 15 FOR citycoord USING se QUERY_ELEMENTS test/test_knn.csv ELEMENTS 10 DIMENSIONALITY 2

... Beep, beep...

___________________________________________________________
1. How to install

Launch the binary Install.exe, choose the installation folder 
and  accept the license (see LICENSE.txt file). The extracted 
Binary file Merkurion.exe is the demo main application.

__________________________________________________________
2. Folder/Directory Structure

Folder /license contains the license specification.
Folder /dataset contains the datasets employed in the system.
You may add as many datasets (as space-separated .csv files) into this directory.
Folder /test contains the query elements employed as examples in this demo tool.
You also may add as many test files with query elements (as space-separated .csv files) into this directory.
The remaining folders contain the dynamic libraries required by Qt applications.

___________________________________________________________
3. Getting Started

First, you must load the dataset. This procedure is carried out by the LOAD command.
Available metric distance function are: CITY_BLOCK, EUCLIDEAN, CHEBYSHEV, CANBERRA or BRAY_CURTIS.

___________________________________________________________
4. Create and view a synopsis

A synopsis only can be created after the loading of a dataset.
To create a synopsis type the command CREATE SYNOPSIS.
The available synopses are: NEAREST_NEIGHBOR_HISTOGRAM, COMPACT_DISTANCE_HISTOGRAM, DISTANCE_PLOT and SEARCH_EVENT.
Each synopsis has a proper setting.
By typing DEFAULTS, the system selects a default setting for the synopsis construction.
On the other hand, you may specifically set the synopsis parameters.

The command syntax is the following:

CREATE SYNOPSIS synopsis_name FOR merkurion_attribute TYPE <synopsis_type>

<synopsis_type> ::= NEAREST_NEIGHBOR_HISTOGRAM USING <param_nnh> |
                    COMPACT_DISTANCE_HISTOGRAM USING <param_cdh> |
                    DISTANCE_PLOT USING <param_dp> |
                    SEARCH_EVENT USING <param_se>
<param_nnh> ::= <pivot_type> num_pivots, MAX_K max_k | DEFAULTS
<pivot_type> ::= OMNI_PIVOTS | K-MEDOIDS_PIVOTS | BRID_PIVOTS
<param_cdh> ::= <pivot_type> num_pivots, POLICY <policy_type>,
                CONSTRAINT <constr_type> | DEFAULTS
<policy_type> ::= TIGHT | AVERAGE | RELAXED
<constr_type> ::= <bucket_const_type> | ERROR error_value |
                  ERROR_BUCKET error_value, num_buckets
<bucket_const_type> ::= BUCKETS num_buckets
<param_dp> ::= <fit_type> WITH num_points POINTS | DEFAULTS
<fit_type> ::= LEAST_SQUARES | REGION_BASED
<param_se> ::= <hist_type>, CONSTRAINT <bucket_const_type> | DEFAULTS
<hist_type> ::= EQUI_WIDTH | EQUI_DEPTH | V-OPTIMAL | CURVE-FITTING

To visualize a synopsis after its construction use the next command:

VIEW SYNOPSIS synopsis_name

where synopsis_name is a valid synopsis.

___________________________________________________________
5. How to obtain estimates based on synopses

First, you must define the query elements for a Range or kNN query.
The query elements must be saved into a space-separated .csv file.

The syntax of the ESTIMATE command is as follows:
ESTIMATE <estimation_type> FOR merkurion_attribute
   USING synopsis_name, QUERY_ELEMENTS <data_source>
<estimation_type> ::= <range_selectivity_estimation> | <knn_radius_estimation>
<range_selectivity_estimation> ::= RANGE_SELECTIVITY RADIUS r
<knn_radius_estimation> ::= KNN_RADIUS K k

___________________________________________________________
6. How to execute a similarity query

Using the query elements, the commands EXECUTE performs a similarity query by resorting to the estimates of the user-defined synopsis whenever necessary.

The syntax of the EXECUTE commands is as follows:
EXECUTE <query_type> FOR merkurion_attribute
   USING synopsis_name, QUERY_ELEMENTS <data_source>
<query_type> ::= <range_select> | <rnn_select>
<range_select> ::= RANGE_SELECT RADIUS r
<knn_select> ::= KNN_SELECT K k

# calico
## Iterative clustering assisted by coarse graining

### Installation

Calico is designed to be run as a standalone script. It does however require some third-party libraries from CRAN. To install them, just run calico by typing in the command prompt:

`calico.R`

or

`calico.R -h`

or

`Rscript calico.R`

In Windows you may need to specify the path of Rscript. For example:

`"c:\Program Files\R\R-3.4.1\bin\Rscript.exe" calico.R`

### Example usage

(You may need to directly call Rscript directly as described above. See "Installation" for an example.)

1. Create an empirical model using a positive control:
`./calico_raster.R -m -i 01_data/20160923_MP_BRAFV600E_DNA_StdCurve//20160923_MP_BRAFV600E_DNA_StdCurve_D03_Amplitude.csv -o 02_results/20160923_MP_BRAFV600E//model_20160923_MP_BRAFV600E_DNA_StdCurve_D03_Amplitude.csv`

2. Cluster an unknown dataset using the empirical model:
`./calico_raster.R -c -r 02_results/20160923_MP_BRAFV600E/model_20160923_MP_BRAFV600E_DNA_StdCurve_D03_Amplitude.csv -i 01_data/20160923_MP_BRAFV600E_DNA_StdCurve//20160923_MP_BRAFV600E_DNA_StdCurve_D03_Amplitude.csv -o 02_results/20160923_MP_BRAFV600E//20160923_MP_BRAFV600E_DNA_StdCurve_D03_Amplitude.clustered.csv >> 02_results/20160923_MP_BRAFV600E//log.txt`

3. (Optional) Draw a figure with the clustered dataset:
`./calico_raster.R -p -r 02_results/20160923_MP_BRAFV600E/model_20160923_MP_BRAFV600E_DNA_StdCurve_D03_Amplitude.csv -i 02_results/20160923_MP_BRAFV600E//20160923_MP_BRAFV600E_DNA_StdCurve_D03_Amplitude.clustered.csv -o 02_results/20160923_MP_BRAFV600E//20160923_MP_BRAFV600E_DNA_StdCurve_D03_Amplitude.clustered.png`

### Output

The model generation step (`-m`) creates an output file that must be used for the clustering (`-c`) and plotting (`-p`) steps.

During the clustering (`-c`) stage, it creates an output csv file (each row is a droplet partition) with appended columns corresponding to the Mahalanobis score against each reference cluster, the final q-score, and the cluster call. It also outputs to stdout the filename, total number of droplets, and the number of droplets in each cluster. This line is useful when looping over many ddPCR assays, as it can be piped into a summary log file.

The plotting (`-p`) stage creates an output PNG with estimated boundaries from the reference model and associated cluster calls for the unknown dataset.

### Calico parameters

There are a number of parameters that you can set:

Number of clusters `-n`: this sets the expected number of clusters from a postive control dataset. Default is 3, and has been tested for 4 with a Taqman ddPCR dataset.

Tiles `-t`: this sets the tile size for the sliding window average. It creates a t x t sliding window to average over the gridded dataset.


# BAP_Genetics
Scripts and Instructions for the analyses in !["A Genomic Resource for the Development, Improvement, and Exploitation of Sorghum for Bioenergy"](http://doi.org/10.1534/genetics.115.183947)

###### Please Cite:
Brenton, Z. W., Cooper, E. A., Myers, M. T., Boyles, R. E., Shakoor, N., Zielinski, K. J., … Kresovich, S. (2016). A Genomic Resource for the Development, Improvement, and Exploitation of Sorghum for Bioenergy. Genetics, 204(1), 21–33. 

## Analyses Performed in the Paper

### SNP Calling Pipeline with TASSEL 5.0
See our complete command lines for running the pipeline in the "TASSEL_BAP.md" file.

### Data Filtering
Refer to the Instructions and Scripts in the HapMap_Helpers respository to filter hapmap files for missing data and minor allele frequency.

### Imputation
Performed with ![fastPhase](http://stephenslab.uchicago.edu/software.html#fastphase)

Refer to the instructions and scripts in the formatConversion repository to find the Perl scripts to convert between hapmap format and fastPHASE input/output format.

### STRUCTURE
To run the program ![STRUCTURE](https://web.stanford.edu/group/pritchardlab/structure.html), you can also refer to programs within the formatConversion repository to convert the hapmap format to the STRUCTURE input format.

### GWAS
Instructions and code for the GAPIT pipeline can be found in the GWAS_Pipelines repository.

### Linkage Disequilibrium
Instructions and code for using the R 'genetics' package to calculate linkage disequilibrium can be found in the LDanalysis repository.  This includes a script for converting a hapmap file to the necessary input format, running the R package analysis automatically, and binning and plotting results.

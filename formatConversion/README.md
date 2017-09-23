# File Format Conversion Utilities
A collection of Perl scripts that can convert between various SNP file
formats.

## Details on Formats Used in these Scripts
1.  **catalog**:  A .tsv format file created by the program
![Stacks](http://catchenlab.life.illinois.edu/stacks/).  Use with
RADseq or GBS data.

2.  **diploid**: The diploid genotype format is the type of file used
    by the
    ![R genetics package](https://cran.r-project.org/web/packages/genetics/genetics.pdf).
    This package has many useful analytical functions for SNP data.

3.  **fasta**: A standard format used for DNA sequences.  Utilized by
    many programs.  Details about this format can be found
    ![here](https://en.wikipedia.org/wiki/FASTA_format).

4.  **fastq**: The raw data format of most next-generation
    sequencers.  Details can be found ![here](https://en.wikipedia.org/wiki/FASTQ_format).

5.  **fastPHASE**: A program that can phase diploid genotype data as
    well as impute missing genotypes.  It requires its own special
    input format, and outputs a special format as well.  Details
    ![here](http://stephenslab.uchicago.edu/software.html#fastphase).

6.  **hmp** : The HapMap format commonly used by the program
    ![TASSEL](http://www.maizegenetics.net/tassel). This program is
	typically used for working with GBS data.

7.  **im**: The input format required for the Isolation-Migration
    program: ![IM](https://bio.cst.temple.edu/~hey/software).

8.  **linkage** : A file format used as input for the program
![HaploView](https://www.broadinstitute.org/haploview/haploview).

9.  **migrate**: The input file format for ![MIGRATE-N](http://popgen.sc.fsu.edu/Migrate/Migrate-n.html)

10.  **numeric**: The same as the hapmap format, except with numeric
    genotypes instead of IUPAC codes.  0=homozygous reference;
    1=heterozygous; 2=homozygous alternate.  Especially useful for
    GWAS applications.

11.  **pileup**: A file format created by
![Samtools](samtools.sourceforge.net) mpileup command.

12.  **plink**: The format used by the software package
![Plink](http://zzz.bwh.harvard.edu/plink/).

13.  **sam**: The common SAM alignment format generated by many
     mapping programs.  Details ![here](https://samtools.github.io/hts-specs/SAMv1.pdf).

14.  **structure**: The input format required by the program ![STRUCTURE](https://web.stanford.edu/group/pritchardlab/structure.html).


## Command line arguments for each script

###### catalog2fasta.pl
Converts the tags file created by Stacks into a fasta file (where each
tag is a separate sequence or "chromosome.")  This can be used as a
reference sequence or to call SNPs with external programs.

```bash
./catalog2fasta.pl <input.tsv> <output.fasta>
	input.tsv = A tsv format file create by Stacks
	output.fasta = The name of the fasta format file to create
```

###### fastPhase2hmp.pl
Converts the imputed genotypes output by fastPHASE (in the
_genotypes.out file) back into a HapMap format file.

```bash
./fastPhase2hmp.pl <phase.inp> <genotypes.out> <chr> <Remove Low Confidence? (Y/N)> <new.hmp.txt>

	phase.inp = The  original fastPHASE input file (need this to get the positions),
	genotypes.out = The genotypes out file from fastPHASE,
	chr = Chromosome Number (only 1 chromosome at a time)
	Remove Low Confidence = Enter Y if you want to replace bracketed (low confidence) genotypes with missing values; enter N to keep all imputed genotype calls regardless of confidence.
	new.hmp.txt = A name for the new hapmap file to create
```

###### fastq2fasta.pl
Coverts Fastq format sequences to Fasta (essentially just removes
lines with the quality scores).

```bash
./fastq2fasta.pl input.fastq
	input.fastq = The name of the input .fastq file.  The output .fasta file will automatically have the same prefix as this input file
```

###### hmp2diploid.pl
Converts a hapmap format SNP file into a diploid genotypes format that
can be read into the R package 'genetics.'  This script allows you to
specify regions to extract from a hapmap file (rather than converting
the whole thing), which is especially useful for running the Linkage
Disequilibrium analysis in R.

```bash
./hmp2diploid.pl <input.hmp.txt> <output.diploid.txt> <chr> <start> <end>
		input.hmp.txt = The input file in hapmap format
		output.diploid.txt = The output file in R genetics format to be created
		chr = The chromosome to extract genotypes from
		start = The start position of the region to extract genotypes from
		end = The end position of the region to extract genotypes from
```

###### hmp2fastPHASE.pl
Converts the HapMap format SNPs into an input format that can be read
by the program fastPHASE.

```bash
./hmp2fastPHASE.pl <input.hmp.txt> <output.phase.inp>

	input.hmp.txt = An input file in the TASSEL hapmap format
	output.phase.inp = The name of the fastPHASE input file to be
	created
```

###### hmp2linkage.pl
Uses a hapmap file to create an input file that can be read by the
program HaploView.

```bash
./hmp2linkage.pl <input.hmp.txt> <output.linkage> <output.markerInfo>
	input.hmp.txt = An input file in the TASSEL hapmap format
	output.linkage.txt = The name of the linkage format file to be created
	output.markerInfo = The name of a locus info. file to be created
```
###### hmp2numeric.pl
Recodes the genotypes in a Hapmap file to be numeric instead of IUPAC
codes.  0=homozygous REF; 1=heterozygous; 2=homozygous ALT.

```bash
./hmp2numeric.pl <input.hmp.txt> <output.txt>
	input.hmp.txt = The name of the input file in hapmap format
	output.txt = The name of the output file to create with numeric SNP codes
```

###### hmp2structure.pl
Creates an file in the input format required by STRUCTURE from a
HapMap file.  Includes options for filtering, and specifying the
desired model to run in STRUCTURE.

```bash
./hmp2structure.pl <input.hmp.txt> <output.structure> <Min. coverage> <Min. MAF> <Linkage Model?> <popData.txt> <locData.txt>

	input.hmp.txt = An input file in the TASSEL hapmap format
	output.structure = The name of the structure input file to be created
	Min. coverage = The minimum number of individuals required at a locus to include it (Integer)
	Min.MAF = The minimum minor allele frequency at a locus to include it (Number between 0 and 1)
	Linkage Model = Type <0> if not desired; <1> if desired
		Note that the linkage model is discouraged without phased haplotype information!
	popData = (Optional) A file indicating population assignments for each individual; Type <NA> if not desired
		If used, the file should be tab delimited, with individual names in column 1 and integers indicating population assignments in the second column
	locData = (Optional) A file indicating location (or phenotype) information for each individual; Type <NA> if not desired
		Same format as the popData file
```

###### im2migrate.pl
Convets the input file for the IM (Isolation-with-Migration) program
into an input file for the MIGRATE-N program.  Since it is often the
case that you may want to test estimates from both programs, this is a
useful way to convert data quickly into a new input file.

```bash
./im2migrate.pl <input.ima2> <output.migrate>
	input.ima2 = The input file in IMa2 format
	output.migrate = The output file to create, in MIGRATE required input format
```
###### pileup2fasta.pl
Converts a file in pileup format (created by Samtools) to a fasta
format.  This scripts looks for continuous segments in the reference
sequence that have mapped reads, and outputs each segment as a single
sequence in fasta format.

```bash
./pileup2fasta.pl <input.pileup> <output.fasta>
	input.pileup = An input file in the pileup format created by Samtools
	output.fasta = The name of the output file in .fasta format to create
```
###### plink2hmp_numeric.pl
Converts the Plink .ped and .map files into a single HapMap file.

```bash
./plink2hmp_numeric.pl <input.ped> <input.map> <output.hmp.txt>
	input.ped = The input .ped file from Plink
	input.map = The input .map file from Plink
	output.hmp.txt = The name of the hapmap file to create
```
	
###### sam2fastq.pl
Extracts the reads and quality strings from a SAM file and outputs
back into Fastq format.  Useful if you want to extract only mapped
(or unmapped) reads from a SAM file, and convert them to fastq to use
in another pipeline.

```bash
./sam2fastq.pl <input.sam> <output.fastq>
	input.sam = The input file in .sam format
	output.fastq = The name of the output file to create
```
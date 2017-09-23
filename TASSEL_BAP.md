# Command Line History from TASSEL 5 on BAP
## Tag Counts

```diff
+ Start: 12:37pm
- End: 4:05pm
```
```bash
lizcooper$ mac2unix.sh BAP_2014_key.txt 
lizcooper$ mkdir TagCounts
lizcooper$ ~/tassel5.0_standalone/run_pipeline.pl -Xmx50G -fork1 -FastqToTagCountPlugin -i /Volumes/Kresovich/DataArchives/DNA/GBS/Sbicolor_RawData/ -k ../BAP_2014_key.txt -e ApeKI -s 700000000 -c 1 -o TagCounts/ -endPlugin -runfork1 >TagCounts/FastqToTagCount.log 2>TagCounts/FastqToTagCount.err
```

## Merge Multiple Tag Counts

```diff
+ Start: 8:39am
- End: 8:41am
```
```bash
lizcooper$ mkdir mergedTagCounts
lizcooper$ ~/tassel5.0_standalone/run_pipeline.pl -fork1 -MergeMultipleTagCountPlugin -Xmx32g -i TagCounts/ -o mergedTagCounts/MasterBAPtags.cnt -c 10 -endPlugin -runfork1 >mergedTagCounts/MergeMultipleTags.log 2>mergedTagCounts/MergeMultipleTags.err
izcooper$ ~/tassel5.0_standalone/run_pipeline.pl -fork1 -TagCountToFastqPlugin -Xmx32g -i mergedTagCounts/MasterBAPtags.cnt -o mergedTagCounts/MasterBAPtags.fq -c 10 -endPlugin -runfork1 >mergedTagCounts/TagsToFastq.log 2>mergedTagCounts/TagsToFastq.err
```

## Tag Alignment
```diff
+ Start: 10:53am
- End: 10:58am
```

```bash
lizcooper$ mkdir bwa_alignment
lizcooper$ bwa aln -t 2 ~/Sorghum_Genome/Sbicolor_v2.1_255.fa mergedTagCounts/MasterBAPtags.fq >bwa_alignment/mergedBAPtags.sai
lizcooper$ bwa samse ~/Sorghum_Genome/Sbicolor_v2.1_255.fa bwa_alignment/mergedBAPtags.sai mergedTagCounts/MasterBAPtags.fq >bwa_alignment/mergedBAPtags.sam
lizcooper$ sed 's/Chr0//g' bwa_alignment/mergedBAPtags.sam | sed 's/Chr//g' | sed 's/super_/1/g' >bwa_alignment/mergedBAPtags_rename.sam
```

## TOPM
```diff
+ Start: 11:02am
- End: 11:02am
```
```bash
lizcooper$ mkdir topm
lizcooper$ ~/tassel5.0_standalone/run_pipeline.pl -fork1 -SAMConverterPlugin -i bwa_alignment/mergedBAPtags_rename.sam -o topm/MasterBAPtags.topm -endPlugin -runfork1 >topm/SAMConverter.log 2>topm/SAMConverter.err
```

##TBT
```diff
+ Start: 11:04am
- End: 2:48pm
```

```bash
lizcooper$ mkdir tbt
lizcooper$ ~/tassel5.0_standalone/run_pipeline.pl -fork1 -FastqToTBTPlugin -i /Volumes/Kresovich/DataArchives/DNA/GBS/Sbicolor_RawData/ -k ../BAP_2014_key.txt -e ApeKI -o tbt/ -y -t mergedTagCounts/MasterBAPtags.cnt -endPlugin -runfork1 >tbt/FastqToTBT.log 2>tbt/FastqToTBT.err
lizcooper$ ~/tassel5.0_standalone/run_pipeline.pl -fork1 -MergeTagsByTaxaFilesPlugin -Xmx32g -i tbt/ -o tbt/mergedBAP.tbt.byte -endPlugin -runfork1 >tbt/MergeTagsByTaxa.log 2>tbt/MergeTagsByTaxa.err
```

## SNP Calling
###### need to run on palmetto cluster ; just compress and upload, then download hapmap and vcf files

```bash
lizcooper$ tar zcf TASSEL_BAP.tar.gz TASSEL_102014/*
lizcooper$ ~/tassel5.0_standalone/run_pipeline.pl -fork1 -MergeTagsByTaxaFilesPlugin -Xmx32g -i tbt/ -o tbt/mergedBAP_mergeRep.tbt.byte -x -runfork1

perl ~/tassel4.0_standalone/run_pipeline.pl -fork1 -DiscoverySNPCallerPlugin -Xmx32g -i ~/tbt/BAP_mergeRep.tbt.byte -y -m ~/topm/BAP_merged.topm -mUpd ~/topm/BAP_merged_wVar.topm -o ~/hapmap/BAP_chr+.hmp.txt -mnMAF 0.05 -ref Sbicolor_v2.1_255.renamed.fa -sC 1 -eC 10 -endPlugin -runfork1 >~/hapmap/SNPCaller_c1.log 2>~/hapmap/SNPCaller_c1.err

lizcooper$ ~/GBS/Scripts/mergeHMP.pl BAP_merge_allChr.hmp.txt 1 10
```

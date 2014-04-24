BreaKmer
========

A method to identify genomic structural variation in target regions/genes from reference-aligned high-throughput sequence data.

Installation
----------

Download the python scripts and run the command:
python setup.py install

Use appropriate commands for installing locally:
python setup.py install --user

The following are required python modules that will need to be installed:
- Biopython (http://biopython.org/wiki/Main_Page)
- Pysam (https://code.google.com/p/pysam/)

Once installed, BreaKmer can be run with the following command:
python breakmer.py <options> <path to config file>

Note that BreaKmer requires the following programs to operate properly:
- BLAT standalone and server binaries ([blat, gfServer, gfClient](http://hgdownload.cse.ucsc.edu/admin/exe/)).
  - Re-alignment to reference sequence.
- [Cutadapt](https://code.google.com/p/cutadapt/)
  - Trims adapter sequence from aligned reads.
- [Jellyfish](http://www.cbcb.umd.edu/software/jellyfish/) 
  - Generating kmers.

Paths to these binaries need to be specified in the configuration file input to breakmer.py

Configuration file
------------

- A configuration file with all the options is available, breakmer.cfg.
- The options in the configuration file determine where the output files are directed and stored as well as where key input files are located:
```
analysis_name=<sample_id>
targets_bed_file=<path to bed file containing locations of target regions>
sample_bam_file=<path to sample bam file>
normal_bam_file=<path to normal bam file>
analysis_dir=<path to analysis directory>
reference_data_dir=<path to where reference files will/are stored> 
other_regions_file=<path to bed file containing targeted unannotated cluster regions> 
cutadapt=<path to cutadapt binary> 
cutadapt_config_file=<path to cutadapt configuration file> 
jellyfish=<path to Jellyfish binary> 
blat=<path to BLAT binaries, blat, gfServer, gfClient>
reference_fasta=<path to whole genome reference fasta file>
gene_annotation_file=<path to ucsc_hg19_refgene.txt>
repeat_mask_file=<path to ucsc_hg19_rmsk.bed>
kmer_size=15
```

Input file formats
-----------

- targets_bed_file = tab-delimited file with columns <chr,start,end,region_name,feature_name>
   - example row = 1       2489165 2489273 TNFRSF14        exon
- other_regions_file = tab-delimited file with columns <chr,start,end,region_name>
   - This file is intended to cover regions that are not annotated in the annotation file. These are useful for cluster regions that are not well annotated in the annotation files.
- cutadapt_config_file = each row corresponds to a parameter for cutadapt (see cutadapt.cfg example file or cutadapt documentation)
  - The file provided is intended for data generated using the paired-end Illumina TruSeq library.
  - Many of the Illumina library sequences have been annotated here - https://wikis.utexas.edu/display/GSAF/Illumina+-+all+flavors


BreaKmer parameters
-------------
| Parameter | Description | Default |
|---------- | ----------- | ------- |
| -l, --log_level    | Logging level | Debug |
| -a, --keep_repeat_regions | Keep indels in repeat regions. Requires a repeat mask bed file. | False |
| -p, --preset_ref_data | Preset all the reference dta for all the target regions before running analysis. | False |
| -s, --indel_size | Indel size filter | 15 |
| -c, --trl_sr_thresh | Assembled read support threshold for translocations | 2 |
| -d, --indel_sr_thresh | Assembled read support threshold for indels | 5 | 
| -r, --rearr_sr_thresh | Assembled read support threshold for inversions and tandem duplications | 3 |
| -g, --gene_list | File containing a list of target region names to consider for analysis. Names must match targets_bed_file region names. | None |
| -k, --keep_intron_vars | Keep indels or rearrangements with breakpoints in intron regions | False |
| -v, --var_filter | Variant types to report (all, indel, trl, rearrangement) | all |
| -m, --rearr_min_seg_len | Threshold for minimum segment length to be rearranged | 30 |
| -n, --trl_min_seg_len | Threshold for minimum length of a translocation segment | 25 |
| -t, --align_thresh | Threshold for minimum read alignment for assembly | .90 |
| -z, --no_output_header | Suppress headers on output files. | False |

- kmer_size = option to change the length of the kmer size used (default = 15).

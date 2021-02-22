


This metagenome analysis workflow is in the process of being updated. For specific queries please contact martin.ostrowski@uts.edu.au

The standardised assembly and annotation pipeline used to characterise shotgun metagenomes produced by the Marine Microbes and Australian Microbiome Project. 

# The updated workflow consists of:

1. trimmomatic
1. bbnorm (bbnorm.sh)
3. SPAdes meta (spades.sh)
4. rename reformat (500) (reformat.sh)
5. prodigal (prodigal.sh)
6. bbmap gene-level and contig-level (bbmap.sh)
7. basta (~/bln.sh then basta loop)
8. eggnogmapper
9. cd-hit 98 (optional)
10. SEED mapping
11. concatatentaion (compile_basta.r, bbmap.r)
12. Overview statistics and visualisation
13. Hypothesis testing
14. Visualisation of results using pathways, modules and other functional hierarchies

Each process is described in detail in a series of three R markdown notebooks. The main processing steps are coded for the UTS HPCC

# The standardised outputs include

a gene abundance table aggreagated on Taxonomy (Genus-level) and a. eggnog orthologous gene IDs
                                                                 b. SEED figIDs
                                                                 c. a contig abundance table

The relative abundances of genes and/or contigs can be estimated in a number of ways (gene-wise, contig-wise and with or wi-thout taxonomic classification using raw data from the readmapping  carried out with bbmap)

# Description:

Steps 01 -08: to be imported

8. eggNOG mapper

split -l 1000000 -a 3 -d input_file.faa input_file.chunk_

fasta format one sequence per line, split in to chunks

# utility scripts
# all in bin

remove_smalls.pl

# unwrap
perl -ne 'chomp; if ( /^>/ ) { print "\n" if $n; print "$_\n"; $n++; } else { s/\s+//g; print; } END { print "\n"; }' MAI_all500.nt > MAI_all500unwrap.nt

rename 1
perl -ane 'if(/^>/){$a++;print ">139751_$a\n"}else{print;}'  /shared/c3/bio_db/BPA/assemblies/contigs/500/MAI/139751_contigs.nt >/shared/c3/bio_db/BPA/assemblies/contigs/500/MAI/139751_contigs.fasta ;
rename 2


for i in 35*vnt.out; do CODE=$(basename $i _contigs.vnt.out); basta sequence -p 51 -b 1 -d  /shared/c3/bio_db/BPA/nt202008/ /shared/c3/bio_db/BPA/assemblies/contigs/500/MAI/${CODE}_contigs.vnt.out ${CODE}_contigs.vnt$



10. SEED

SEED annotations were acquired by a diamond search of the predicted proteins against the SEED database with the hierarchy of functional categories were mapped wioth the aid of the subsystems R package <https://github$

11. Initial ordination and clustering to check the data sanity was carried out with vegan::metaMDS on sqrt-transformed normalised read count data for the top 100,000+ gene profiles (by total abundance) followed by km$


# old workflow below
***

# metagenome
metagenome analysis workflow

# AMP
AMP Metagenomic Workflow Description

## Metagenome Processing Products Description and Purpose



### 1. Australian Reference Gene Catalogue (ARGC)

- A. non-redundant catalogue of predicted gene sequences (> 200 nt, clustered at 95% nt) identity[clustered alongside the marRef <https://mmp.sfb.uit.no> Database?]

- B. gene abundance table (c.f. a zOTU table)

- C. The ARGC can be annotated against any source database

- D. contigs assembled for each discrete sample, with Taxonomic assignment

- E. Predicted gene sequences and protein sequences (fasta files)

#### Tools
- Trimmomatic
- Fastqc
- Spades/megahit
- metagenemark
- cd-hit
- basta

see AMRGC_Readme.txt for a starting point.
see metagenome workflow for details on the assembly and read mapping

#### Discussion: The RGC structure mirrors the structure of the zOTU table and can be delivered to functional interfaces in .biom format and potentially utilise the same suite of ecological tools.

The size of the database approaches 56,000,000 rows, but could be cut significantly by omitting singletons or short sequences (e.g. less than 500) if there was an apetite  or need

Ideally the gene abundances would be calculated with the exact same methods as the EBI profiles for inter comparison with global datasets, however I can’t find documentation on the process so cannot comment on the accuracy or applicability to the AMP data.

#### Format: ideally a subsettable .biom file served from the Bioplatforms Portal

Annotation sources include: best hit and LCA taxonomic assignment against nt, COG/NOG, GO-slim, nr, Interproscan. (Searches against Datbases in bold have been done). The others are not implemented.

***

### 2. SUPER-FOCUS functional characterisation -short term functional classification and enumeration against the SEED database

Discussion: can be implemented quickly. Hierarchal organisation of functional classes at 4 levels. Raw or Filtered reads (Fastq or Fasta) can be analysed

Can be used to produce rapid results. A curated RGC with defined read mapping criteria and inter comparison with global datasets is preferred.

- A. a table of results for all levels and all samples. See “output_all_levels_and_functions.xls”

#### Tools

- SUPER-FOCUS

### 3.  Metagenome Assembled Genomes (MAGs)

- A. Regional coassemblies with metadata file (length, LCA, coverage)



- B. Regional Assembled Genome bins with metadata (length, LCA, coverage, method of binning, contamination, completeness)

	i). Bacterial bins
	ii) Archaeal bins
	iii) Viral bins -complete viral genomes
	iv) eukaryote bins

#### Tools

- bowtie2
- samtools 
- MetaBat 
- GroopM
- Basta
- contig assembler (Geneious, CAP3 etc.)
- CheckM

#### Discussion: Some optimisation may be required to maximise the completeness and reduce contamination during the binning process.

#### Format: ideally a bioproject on one of the database sites

***

# Priorities

Test the complete workflow on datasets that are a priority for the community

- YON
- GAB
- EAC - Amaranta MQ
-  Entire dataset - assembled contigs

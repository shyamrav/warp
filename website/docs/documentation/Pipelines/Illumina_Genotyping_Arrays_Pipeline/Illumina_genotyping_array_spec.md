# VCF Overview: Illumina Genotyping Array 

The [Illumina Genotyping Array Pipeline](https://github.com/broadinstitute/warp/blob/develop/pipelines/broad/genotyping/illumina/IlluminaGenotypingArray.wdl) v1.11.0 pipeline produces a VCF (Variant Call Format) output with data processing and sample-specific genotype information. The VCF follows the format listed in the [VCF 4.2 specification](https://samtools.github.io/hts-specs/VCFv4.2.pdf), but additionally contains fields and attributes that are unique to the Arrays pipeline.

This document describes the Array pipeline’s unique VCF fields and attributes that are not listed in the standard VCF specification. To learn more about the pipeline, see the [Illumina Genotyping Array Pipeline Overview](./IlluminaGenotypingArray.documentation.md).


:::tip How do I view a VCF file?
A VCF is a tab delimited text file that you can open with either a text editor or with a spreadsheet editor (like Microsoft Excel). 
:::

## Meta information fields
Each VCF has meta information fields with attributes that generally describe the processing of the sample in the array. 


- arrayType - Name of the genotyping array that was processed.
- autocallDate - Date that the genotyping array was processed by ‘autocall’ (aka gencall), the Illumina genotype calling software
- autocallGender - Gender (sex) that autocall determined for the sample processed
- autocallVersion - Version of the autocall/gencall software used
- chipWellBarcode - Chip well barcode (a unique identifier for sample as processed on a specific location on the Illumina genotyping array)
- clusterFile - Cluster file used
- extendedIlluminaManifestVersion - Version of the ‘extended Illumina manifest’ used by the VCF - generation software.
- extendedManifestFile - File name of the ‘extended Illumina manifest’ used by the VCF generation software
- fingerprintGender - Gender (sex) determined using an orthogonal fingerprinting technology, populated by an optional parameter used by the VCF generation software
- gtcCallRate - GTC call rate of the sample processed that is generated by the autocall/gencall software and represents the fraction of callable loci that had valid calls
- imagingDate - Creation date for the chip well barcode IDATs (raw image scans)
- manifestFile - Name of the Illumina manifest (.bpm) file used by the VCF generation software
- sampleAlias - Sample name

:::tip Illumina-specific meta information
In addition to the above attributes, there are several attributes specific to Illumina control values that are not described here: Biotin, DNP, Extension, Hyb, NP, NSB, Restore, String, TargetRemoval. 
:::

## Header line columns 
Following the meta-information, there are 8 standard header line columns:
1. #CHROM 
2. POS 
3. ID 
4. REF 
5. ALT 
6. QUAL 
7. FILTER 
8. INFO

These columns are standard to each VCF, but the Illumina Genotyping Array Pipeline has an additional FORMAT column. Pipeline-specific column attributes are described below:

### FILTER 
- DUPE - Filter applied if there are multiple rows in the VCF for the same loci and alleles. For example, if there are two or more rows that share the same chromosome, position, ref allele and alternate alleles, all but one of them will have the ‘DUPE’ filter set
- TRIALLELIC - Filter applied if there is a site at which there are two alternate alleles and neither of them is the same as the reference allele
- ZEROED_OUT_ASSAY - Filter applied if the variant at the site was ‘zeroed out’ in the Illumina cluster file; typically applied when the calls at the site are intentionally marked as unusual. Genotypes called for sites that are ‘zeroed out’ will always be no-calls 

### FORMAT (genotype)  
- GT - GENOTYPE; standard field described in the [VCF specification](https://samtools.github.io/hts-specs/VCFv4.2.pdf)
- IGC - Illumina GenCall Confidence Score. Measure of the call confidence
- X - Raw X intensity as scanned from the original genotyping array
- Y - Raw Y intensity as scanned from the original genotyping array
- NORMX - Normalized X intensity
- NORMY - Normalized Y intensity
- R - Normalized R Value (one of the polar coordinates after the transformation of NORMX and NORMY)
- THETA - Normalized Theta value (one of the polar coordinates after the transformation of NORMX and NORMY)
- LRR - Log R Ratio
- BAF - B Allele Frequency

### INFO 
This column contains attributes specific to the array's probe. 

- AC - Allele Count in genotypes for each ALT allele; standard field in the VCF specification
- AF - Allele Frequency; standard field in the VCF specification
- AN - Allele Number; standard field in the VCF specification
- ALLELE_A - Allele A as annotated in the Illumina manifest (a *suffix indicates this is the reference allele)
- ALLELE_B - Allele B as annotated in the Illumina manifest (a *suffix indicates this is the reference allele)
- BEADSET_ID - BeadSet ID; an Illumina identifier used for normalization
- GC_SCORE - Illumina GenTrain Score; a quality score describing the probe design
- ILLUMINA_BUILD - Genome Build for the design probe sequence, as annotated in the Illumina manifest
- ILLUMINA_CHR - Chromosome of the design probe sequence as annotated in the Illumina manifest
- ILLUMINA_POS - Position of the design probe sequence (on ILLUMINA_CHR) as annotated in the Illumina manifest
- ILLUMINA_STRAND - Strand for the design probe sequence as annotated in the Illumina manifest
- PROBE_A - Allele A probe sequence as annotated in the Illumina manifest
- PROBE_B - Allele B probe sequence as annotated in the Illumina manifest; only present on strand ambiguous SNPs
- SOURCE - Probe source as annotated in the Illumina manifest
- refSNP - dbSNP rsId for this probe

The remaining attributes describe the cluster definitions provided in the cluster file used for calling the genotype for a SNP.

- N_AA - Number of AA calls in training set
- N_AB - Number of AB calls in training set
- N_BB - Number of BB calls in training set
- devR_AA - Standard deviation of normalized R for AA cluster
- devR_AB - Standard deviation of normalized R for AB cluster
- devR_BB - Standard deviation of normalized R for BB cluster
- devTHETA_AA - Standard deviation of normalized THETA for AA cluster
- devTHETA_AB - Standard deviation of normalized THETA for AB cluster
- devTHETA_BB - Standard deviation of normalized THETA for BB cluster
- devX_AA - Standard deviation of normalized X for AA cluster
- devX_AB - Standard deviation of normalized X for AB cluster
- devX_BB - Standard deviation of normalized X for BB cluster
- devY_AA - Standard deviation of normalized Y for AA cluster
- devY_AB - Standard deviation of normalized Y for AB cluster
- devY_BB - Standard deviation of normalized Y for BB cluster
- meanR_AA - Mean of normalized R for AA cluster
- meanR_AB - Mean of normalized R for AB cluster
- meanR_BB - Mean of normalized R for BB cluster
- meanTHETA_AA - Mean of normalized THETA for AA cluster
- meanTHETA_AB - Mean of normalized THETA for AB cluster
- meanTHETA_BB - Mean of normalized THETA for BB cluster
- meanX_AA - Mean of normalized X for AA cluster
- meanX_AB - Mean of normalized X for AB cluster
- meanX_BB - Mean of normalized X for BB cluster
- meanY_AA - Mean of normalized Y for AA cluster
- meanY_AB - Mean of normalized Y for AB cluster
- meanY_BB - Mean of normalized Y for BB cluster

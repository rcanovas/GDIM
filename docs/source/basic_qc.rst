Pre-Imputation Basic Quality Control 
====================================


Check NCBI build release 
-------------------------

In order to impute a plink file (bim/bed/fam) we first need to be sure that the information stored is using the NCBI buld 37. For that, you can follow these steps:

  * We can check the NCBI build by selecting some random rsids from the .bim file and checking their chromosome and position fields in https://www.ncbi.nlm.nih.gov/SNP/. 
    
  * Another option is to download the data bases of NCBI builds extracted from dbSNP (`GRCh37`_ or `GRCh36`_) and check the rsids positions and chromosome in these files (ex. using grep rsid data_base)

Finally, to update the rsids we followed two steps:

  * Note that on new genome assembly, previously different contig may align. So different rs clusters map to the same location. To fix this use the `RsMergeArch`_ table (https://www.ncbi.nlm.nih.gov/projects/SNP/snp_db_table_description.cgi?t=RsMergeArch) from NCBI and plink to update the rsids. 

  * Update the rsids positions to NCBI build 37 (if required) using the information from GRCh37 and plink (--update-map command)

.. _`GRCh37`:	http://hgdownload.cse.ucsc.edu/goldenPath/hg18/database/snp130.txt.gz
.. _`GRCh36`:	http://hgdownload.cse.ucsc.edu/goldenPath/hg19/database/snp150.txt.gz  
.. _`RsMergeArch`: ftp://ftp.ncbi.nlm.nih.gov/snp/organisms/human_9606/database/organism_data/RsMergeArch.bcp.gz



Fix Heterozygous Haploid genotypes warnings (if reported)
---------------------------------------------------------

This is usually caused by male heterozygous calls in the X chromosome pseudo-autosomal region. The recommended steps to follow are:

  * Check that the sex of the individuals are correct (check-sex command line on plink tool). 
    
  * If the waring is cause by SNPs that are in XY area, try to use split-x b37

  .. code-block:: console
  
    plink1.9 --bfile file --split-x b37 no-fail --make-bed --out output_file
  
  * If nothing change, warnings must be variant-calling errors (but genotype calls of those rsids are correct) and in this case 
    you can set those calls to missing if required

  .. code-block:: console

    plink1.9 --bfile file --make-bed --set-hh-missing --out output --allow-no-sex


Filter SNPs and Individuals
----------------------------

An important part of preprocessing out file considered exclude individuals and SNPs that do not carry 
sufficient information reliable. In our case we considered the following parameters:

  * Exclude individuals and SNPs with high missing genotype data (ex. 10%)

  * Filter SNPs with low minor allele frequency (MAF) (ex. MAF < 0.01)
    
  .. code-block:: console

    plink1.9 --bfile file --maf 0.01 --geno 0.1 --mind 0.1 --allow-no-sex --make-bed --out output


Principal Component Analysis (PCA)
-----------------------------------

Before imputing the data, if cases/control information are presented in the phenotype data, it is recommendable to do 
a PCA analysis of the set.


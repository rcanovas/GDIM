Pre-Imputation Basic Quality Control 
====================================


Check NCBI build release 
-------------------------

In order to impute a plink file (bim/bed/fam) we first need to be sure that the information stored is using the NCBI buld 37. For that, you can follow these steps:

  * We can check the NCBI build by selecting some random rsids from the .bim file and checking their chromosome and position fields in https://www.ncbi.nlm.nih.gov/SNP/. 
    
  * Another option is to download the data bases of NCBI builds extracted from dbSNP (`GRCh37`_ or `GRCh46`_) and check the rsids positions and chromosome in these files (ex. using grep rsid data_base)

If requiered, to update the rsids we followed two steps:

  * Update the rsids names 
.. if needed (update_rsids_names using rsMerge_id_to_id.txt from the assembly_build folder)
.. Ex: sbatch /home/rcanovas/scripts/plink/update_rsids_names.sh file /home/rcanovas/assembly_builds/rsMeger_id_to_id.txt  file_b3

  * Update the rsids positions to NCBI build 37 
.. (check /home/rcanovas/scripts/plink/update_rsids_pos.sh and use rsids_pos_b37.txt  from the assembly_build folder)


.. _`GRCh37`:	http://hgdownload.cse.ucsc.edu/goldenPath/hg18/database/snp130.txt.gz
.. _`GRCh36`:	http://hgdownload.cse.ucsc.edu/goldenPath/hg19/database/snp150.txt.gz  



Fix Heterozygous Haploid genotypes warnings (if reported)
---------------------------------------------------------

This is usually caused by male heterozygous calls in the X chromosome pseudo-autosomal region. The recommended steps to follow are:

  * Check that the sex of the individuals are correct (check-sex command line on plink tool). 
    
  * If the waring is cause by SNPs that are in XY area, try to use split-x b37: 
            
          plink1.9 --bfile prefix_plink_file --split-x b37 no-fail --make-bed --out prefix_output_file
 

.. * If nothing change, warnings must be variant-calling errors (but genotype calls of those rsids are correct)
.. * Use --make-bed --set-hh-missing to set those calls to missing if required (check 
.. /home/rcanovas/scripts/plink/ignore_hh.sh)

.. Ex: sbatch /home/rcanovas/scripts/plink/ignore_hh.sh file_b37 file_b37h

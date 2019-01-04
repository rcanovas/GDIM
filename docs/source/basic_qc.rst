Pre-Imputation Basic Quality Control 
====================================


Check NCBI build release 
-------------------------

In order to impute our plink file (bim/bed/fam) we first need to be sure that it was generated using the NCBI buld 37. For that you can follow these steps::

.. * We can check the build by selecting some random rsids from the .bim file and checking their chrs and positions
            in https://www.ncbi.nlm.nih.gov/SNP/ or using grep (only to check the positions) on the files in
..            /home/rcanovas/assembly_builds  (rsids_pos_37.txt for build 37 and rsids_pos_36.txt for build 36)
..   * To update the rsids we followed two steps
..        * Update the rsids names if needed (check /home/rcanovas/scripts/plink/update_rsids_names.sh and use
           rsMerge_id_to_id.txt from the assembly_build folder)
           Ex: sbatch /home/rcanovas/scripts/plink/update_rsids_names.sh file /home/rcanovas/assembly_builds/rsMeger_id_to_id.txt  file_b3
..
..        * Update the rsids positions to b37 (check /home/rcanovas/scripts/plink/update_rsids_pos.sh and use
..           rsids_pos_b37.txt  from the assembly_build folder)
..
..                        Ex:  sbatch /home/rcanovas/scripts/plink/update_rsids_pos.sh file_b3 /home/rcanovas/assembly_builds/rsids_pos_b37.txt file_b37


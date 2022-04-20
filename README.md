# manuscript2
 PASCAL
./Pascal --pval=/media/sf_shared_folder/testfolder/t2dfinalforldsc.txt --up=20000 --down=20000 --runpathway=on --outsuffix=t2dc2.txt --genesetfile/resources/genesets/msigdb/c2.cp.v7.4.entrez.gmt \
./Pascal --pval=/media/sf_shared_folder/testfolder/t2dfinalforldsc.txt --up=20000 --down=20000 --runpathway=on --outsuffix=t2dc5.txt --genesetfile/resources/genesets/msigdb/c5.go.v7.4.entrez.gmt \
./Pascal --pval=/media/sf_shared_folder/testfolder/covid19ihg19forldsc.txt --up=20000 --down=20000 --runpathway=on --outsuffix=c19c2.txt --genesetfile/resources/genesets/msigdb/c2.cp.v7.4.entrez.gmt \
./Pascal --pval=/media/sf_shared_folder/testfolder/covid19ihg19forldsc.txt --up=20000 --down=20000 --runpathway=on --outsuffix=c19c5.txt --genesetfile/resources/genesets/msigdb/c5.go.v7.4.entrez.gmt 
# LDSC 
singularity run adamldsc.sif
munge_stats.py \
--sumstats t2dfinalforldsc.txt \
--N 62892 \
--out tfinal \
--merge-alleles w_hm3.snplist

munge_stats.py \
--sumstats covid19ihg19forldsc.txt \
--N 38984 \
--out c19ihg19 \
--merge-alleles w_hm3.snplist

ldsc.py \
--rg tfinal.sumstats.gz,c19ihg19.sumstats.gz \
--ref-ld-chr eur_w_ld_chr/ \
--w-ld-chr eur_w_ld_chr/ \
--out t2d_c19
# cFDR analysis

singularity run adamldsc.sif

python sumstats.py csv --auto --sumstats traitfolder/t2dfinalforldsc.txt --n-val 62892 --out t2dfinalforldsc.csv --force \
python sumstats.py zscore --sumstats traitfolder/t2dfinalforldsc.csv --out traitfolder/t2dfinalforldscz.csv \
python sumstats.pt mat  --sumstats t2dfinalforldscz.csv --ref 9545380.ref --out traitfolder/t2dfinalforldscz.mat

python sumstats.py csv --auto --sumstats traitfolder/covid19ihg19forldsc.txt --n-val 38984 --out covid19ihg19forldsc.csv --force \
python sumstats.py zscore --sumstats traitfolder/covid19ihg19forldsc.csv --out traitfolder/covid19ihg19forldscz.csv \
python sumstats.pt mat  --sumstats covid19ihg19forldscz.csv --ref 9545380.ref --out traitfolder/covid19ihg19forldscz.mat

cd /cm/shared/apps/maths/matlab/R2020b/bin \
cd /chunhwu/pleiofdr/pleiofdr-master \
run('runme.m')

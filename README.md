# Overview  

Smart Variant Filtering (SVF) is and open-source Python framework for genomic variants filtering using machine learning.
Variant filtering process consists of selecting highly confident variants and removing the ones that are falsely called. Secondary genomic DNA analysis is mainly oriented toward alignment and variant calling assuming the accuracy of these two would provide major influence to the overall quality. Variant filtering step was mostly left out from deeper testing even though it can boost precision of variant calls significantly.

Smart Variant Filtering uses machine learning algorithms trained on features from the existing Genome In A Bottle (GIAB) variant-called samples (HG001-HG005) to perform variant filtering (classification). 

SVF is available on github and also as a Public tool on Seven Bridges Public App gallery. It is completely open sourced and free to use by any party or conditions. The comparison results obtained during deep, 3-stage testing has proven that it outperforms the solutions currently used within the most of the secondary DNA analysis. Smart Variant Filtering increases the precision of called SNVs (removes false positives) for up to 0.2% while keeping the overall f-score higher by 0.12-0.27% than in the existing solutions. Indel precision is increased up to 7.8%, while f-score increasement is in range 0.1-3.2%.

Detailed information about Smart Variant Filtering available in [white paper](https://www.sevenbridges.com/smart-variant-filtering).

## Getting Started

Before starting SVF make sure that the following modules are installed: 

```
pip install pandas
pip install sklearn
```

## Running the example
After cloning the repository several examples of SVF are available for training a model:

```
python svf_train.py --table_indel data/wes/6_features/HG001_NIST7035_dbsnp_indels.table --table_snv data/wes/6_features/HG001_NIST7035_dbsnp_SNVs.table --alg_param_indel MLP,250,logistic,sgd --alg_param_snv MLP,500,tanh,adam --vcf data/wes/6_features/HG005_oslo_exome_chr20.vcf
    
python svf_train.py --table_indel data/wes/7_features/HG002_oslo_exome_dbsnp_indels.table --table_snv data/wes/7_features/HG002_oslo_exome_dbsnp_SNVs.table --alg_param_indel MLP,10,logistic,sgd --alg_param_snv MLP,10,logistic,sgd --vcf data/wes/7_features/HG001_NIST7035_raw.dbsnp.vcf --features_snv QD,MQ,FS,MQRankSum,ReadPosRankSum,SOR,dbSNPBuildID --features_indel QD,MQ,FS,MQRankSum,ReadPosRankSum,SOR,dbSNPBuildID

```
and for performing a filtering:

```
python svf_apply.py --snv_model data/wes/6_features/HG001_NIST7035_dbsnp_SNVs.snv.sav --indel_model data/wes/6_features/HG001_NIST7035_dbsnp_indels.indel.sav --vcf data/wes/6_features/HG005_oslo_exome_chr20.vcf
    
```
## Models
This repository contains pre-trained models for SNVs and indels, for whole genome sequencing (WGS) and whole exome sequencing (WES) samples. 
7 features are used for training the models: QD,MQ,FS,MQRankSum,ReadPosRankSum,SOR and dbSNPBuildId.
6 features were used for training the WGS model (QD,MQ,FS,MQRankSum,ReadPosRankSum,SOR) with the following samples and library preps: HG001-NA12878-50x, HG003-60x, HG004-60x, HG005-60x, HG001-CEPH-30x, HG001-Robot-30x, HG001-ERR17432-150x.
7 features were used for training the WES model (QD,MQ,FS,MQRankSum,ReadPosRankSum,SOR,dbSNPBuildId) with the following samples and library preps: HG001-NA12878Rep1-S1-L00R-140x, HG003-oslo-190x, HG004-oslo-190x, HG005-oslo-190x.


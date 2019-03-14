[![Build Status](https://travis-ci.org/maxibor/microbiomeNormalization.svg?branch=master)](https://travis-ci.org/maxibor/microbiomeNormalization)
# **micronorn**: **Micro**biome **Norm**alization

Normalisation of metagenomics sequencing read count data.

## Quick start

```
micronorm data/read_count.csv
```

## Dependancies

- Pandas  
`conda install pandas`
- Numpy  
`conda install numpy`


## Assumption

- micronorm assumes that all the reads in sample are assigned to a feature, even if it's `unmapped_reads`.
- Samples in columns, OTUs in rows

## Example input:

Example file here:  [data/read_count.csv](data/read_count.csv)

|      | Sample1 | Sample2 | Sample3 |
|------|---------|---------|---------|
| OTU1 | 0       | 10      | 4       |
| OTU2 | 2       | 6       | 12      |
| unmapped_reads | 33      | 55      | 200     |

## Example output (with RLE method):

Example file here:  [data/read_count.RLE_normalized.csv](data/read_count.RLE_normalized.csv)

|      | Sample1 | Sample2 | Sample3 |
|------|---------|---------|---------|
| OTU1 | 0.0     | 11.0    | 2.0     |
| OTU2 | 5.0     | 6.0     | 5.0     |
| unmapped_reads | 79.0    | 59.0    | 79.0    |

## Help

```
$ micronorm -h
usage: micronorm [-h] [-m METHOD] [-p PROCESS] [-o OUTPUT] countFile

=======================================
Normalization of NGS microbiome samples
Homepage: https://github.com/maxibor/microbiomeNormalization
Methods:
    - RLE Normalization: Relative Log Expression (DESeq)
    - CLR Transformation: Centered Log Ratio Transform (Compositional)
=======================================

positional arguments:
  countFile   path to csv countFile

optional arguments:
  -h, --help  show this help message and exit
  -m METHOD   method (CLR | RLE | GMPR). Default = GMPR
  -p PROCESS  Number of process (only) for GMPR parallel computation.
  -o OUTPUT   Path to output file. Default =
              ./<file_basename>.<method>_micronorm.csv
```

## References

- [RLE - Love, M. I., Huber, W., & Anders, S. (2014). Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome biology, 1(12), 550.](https://doi.org/10.1186/s13059-014-0550-8)
- [CLR - Aitchison, J. (1982). The statistical analysis of compositional data. Journal of the Royal Statistical Society: Series B (Methodological), 44(2), 139-160.](https://doi.org/10.1111/j.2517-6161.1982.tb01195.x)
- [GMPR - Chen, L., Reeve, J., Zhang, L., Huang, S., Wang, X., & Chen, J. (2018). GMPR: A robust normalization method for zero-inflated count data with application to microbiome sequencing data. PeerJ, 6, e4600.](https://doi.org/10.7717/peerj.4600)
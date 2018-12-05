[![Build Status](https://travis-ci.org/maxibor/microbiomeNormalization.svg?branch=master)](https://travis-ci.org/maxibor/microbiomeNormalization)
# **micronorn**: **Micro**biome **Norm**alization

Normalisation of metagenomics sequencing read count data.

## Quick start

```
micronorm data/read_count.csv
```

## Assumption

micronorm assumes that all the reads in sample are assigned to a feature, even if it's `unmapped_reads`.

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
usage: micronorm [-h] [-m METHOD] [-o OUTPUT] countFile

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
  -m METHOD   method (RLE | CLR). Default = RLE
  -o OUTPUT   Path to output file. Default = ./<file_basename>.<method>_micronorm.csv
```

## Methods

### RLE Normalization Method (DESeq)

- **Step1:** Take the (natural) log of all values
- **Step2:** Average each row
- **Step3:** Filter out OTU with infinity
- **Step4:** Substract the average log value from the log of the counts to each sample
- **Step5:** Calculate the median for each sample
- **Step6:** Convert the medians to "normal" numbers to get scaling factors for each sample
- **Step7:** Divide the original read counts by the scaling factor

### CLR Transformation Method (Compositional)

- **Step1:** Replace all 0s by 1, and compute the geometric mean for each sample
- **Step2:** Divide each value in each sample by the sample geometric mean
- **Step3:** Take the natural log of each value

## References

- [Normalization and microbial differential abundance strategies depend upon data characteristics](https://microbiomejournal.biomedcentral.com/articles/10.1186/s40168-017-0237-y)
- [Microbiome Datasets Are Compositional: And This Is Not Optional](https://www.frontiersin.org/articles/10.3389/fmicb.2017.02224/full#B40)

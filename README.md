# **micronorn**: **Micro**biome **Norm**alization

Normalisation of metagenomics sequencing read count data using a DESeq2 approach

## Quick start

```
micronorm data/read_count.csv
```

## Assumption

This normalization methods assumes that all the reads in sample are assigned to a feature, even if it's `unmapped_reads`.

## Example input:

Example file here:  [data/read_count.csv](data/read_count.csv)

|      | Sample1 | Sample2 | Sample3 |
|------|---------|---------|---------|
| OTU1 | 0       | 10      | 4       |
| OTU2 | 2       | 6       | 12      |
| unmapped_reads | 33      | 55      | 200     |

## Example output:

Example file here:  [data/read_count.normalized.csv](data/read_count.normalized.csv)

|      | Sample1 | Sample2 | Sample3 |
|------|---------|---------|---------|
| OTU1 | 0.0     | 11.0    | 2.0     |
| OTU2 | 5.0     | 6.0     | 5.0     |
| unmapped_reads | 79.0    | 59.0    | 79.0    |

## Help

```
$ micronorm -h
usage: micronorm [-h] [-o OUTPUT] countFile

Normalizing sample in a DESeq2 fashion

positional arguments:
  countFile   path to csv countFile

optional arguments:
  -h, --help  show this help message and exit
  -o OUTPUT   Path to output file. Default = ./<file_basename>.normalized.csv
```

## Method

- **Step1:** Take the (natural) log of all values
- **Step2:** Average each row
- **Step3:** Filter out OTU with infinity
- **Step4:** Substract the average log value from the log of the counts to each sample
- **Step5:** Calculate the median for each sample
- **Step6:** Convert the medians to "normal" numbers to get scaling factors for each sample
- **Step7:** Divide the original read counts by the scaling factor
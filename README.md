# **micronorn**: **Micro**biome **Norm**alization

Normalisation of metagenomics sequencing read count data using a DESeq2 approach

## Quick start
```
microbiomeNormalization data/read_count.csv
```
## Example input:

Example file here:  [data/read_count.csv](data/read_count.csv)
|      | Sample1 | Sample2 | Sample3 |
|------|---------|---------|---------|
| OTU1 | 0       | 10      | 4       |
| OTU2 | 2       | 6       | 12      |
| OTU3 | 33      | 55      | 200     |

## Example output:

Example file here:  [data/read_count.normalized.csv](data/read_count.normalized.csv)

|      | Sample1 | Sample2 | Sample3 |
|------|---------|---------|---------|
| OTU1 | 0.0     | 11.0    | 2.0     |
| OTU2 | 5.0     | 6.0     | 5.0     |
| OTU3 | 79.0    | 59.0    | 79.0    |

## Help
```
$ micronorm -h
usage: microbiomeNormalization [-h] [-o OUTPUT] countFile

Normalizing sample in a DESeq2 fashion

positional arguments:
  countFile   path to csv countFile

optional arguments:
  -h, --help  show this help message and exit
  -o OUTPUT   Path to output file. Default = ./<file_basename>.normalized.csv}```
```

## Method
- **Step1:** Take the (natural) log of all values
- **Step2:** Average each row
- **Step3:** Filter out OTU with infinity
- **Step4:** Substract the average log value from the log of the counts to each sample
- **Step5:** Calculate the median for each sample
- **Step6:** Convert the medians to "normal" numbers to get scaling factors for each sample
- **Step7:** Divide the original read counts by the scaling factor
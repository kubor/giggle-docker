# giggle-docker

GIGGLE on docker.

GIGGLE: [ryanlayer/giggle: Interval data structure](https://github.com/ryanlayer/giggle)

## Usage

```
docker run kubor/giggle-docker giggle -h
```

## Example

Download test data.

```
wget https://s3.amazonaws.com/layerlab/giggle/roadmap/roadmap_sort.tar.gz
tar -zxvf roadmap_sort.tar.gz
```

Generate index.

```
docker run --rm -v `pwd`:/share kubor/giggle-docker \
    giggle index -s -f -i "/share/roadmap_sort/*gz" -o /share/roadmap_sort_b
```

Download peak data.

```
wget ftp://ftp.ncbi.nlm.nih.gov/geo/samples/GSM1218nnn/GSM1218850/suppl/GSM1218850_MB135DMMD.peak.txt.gz
```

Zip by bgzip.

```
docker run --rm -v `pwd`:/share kubor/giggle-docker \
    sh -c "zcat /share/GSM1218850_MB135DMMD.peak.txt.gz \
        | awk '\$8>100' \
        | cut -f1,2,3 \
        | /giggle/lib/htslib/bgzip -c \
            > /share/GSM1218850_MB135DMMD.peak.q100.bed.gz"
```

Search peak regions.

```
docker run --rm -v `pwd`:/share kubor/giggle-docker \
    giggle search -s -i /share/roadmap_sort_b -q /share/GSM1218850_MB135DMMD.peak.q100.bed.gz > GSM1218850_MB135DMMD.peak.q100.bed.gz.result
```

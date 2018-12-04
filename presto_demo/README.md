# TileDB Presto Demo

This notebook is a demo of the
[TileDB Presto connector](https://github.com/TileDB-Inc/TileDB-Presto).

## Prerequisites

### Load Sample NYSE Data

In order to run this demoo you will need to download and ingest a
sample dataset into a TileDB array. The demo uses the Alphabet stock `GOOG`.
Download the `SPLITS_US_ALL_BBO_G_20180730.gz` from `nyxdata` ftp:

```
curl ftp://ftp.nyxdata.com/Historical%20Data%20Samples/Daily%20TAQ%20Sample%202018/SPLITS_US_ALL_BBO_G_20180730.gz -o SPLITS_US_ALL_BBO_G_20180730.gz

gunzip SPLITS_US_ALL_BBO_G_20180730.gz
```
You will also need the associated master file:

```
curl ftp://ftp.nyxdata.com/Historical%20Data%20Samples/Daily%20TAQ%20Sample%202018/EQY_US_ALL_REF_MASTER_20180306.gz -o EQY_US_ALL_REF_MASTER_20180306.gz

gunzip EQY_US_ALL_REF_MASTER_20180306.gz
```

Download the prebuilt [TileDB-NYSE-Ingestor](https://github.com/TileDB-Inc/TileDB-NYSE-Ingestor/releases):

```
curl -s https://api.github.com/repos/TileDB-Inc/TileDB-NYSE-Ingestor/releases/latest | grep "browser_download_url" | cut -d '"' -f 4 | wget -i -

chmod +x nyse_ingestor.linux-x86_64
```

Create the array, and make sure to adjust the S3 bucket URI:

```
./nyse_ingestor.linux-x86_64 --type quote --array "s3://your_bucket_here/tiledb_arrays/quote_20180730" -c
```

Load the data into the array, again adjusting the S3 bucket URI:

```
./nyse_ingestor.linux-x86_64 --type quote --array "s3://your_bucket_here/tiledb_arrays/quote_20180730" -f SPLITS_US_ALL_BBO_G_20180730 --master_file EQY_US_ALL_REF_MASTER_20180306
```

### Run Local Presto Instance

Before running this notebook, you must start a [Presto Docker image](https://hub.docker.com/r/tiledb/tiledb-presto/).

```
docker run -it --rm tiledb/tiledb-presto
```

## Run the Notebook

A conda environment file is provided for installing all Python dependencies for
the notebook:

```
conda env create -f environment.yml
```

Now you are ready to run the Jupyter notebook inside the environment:

```
source activate tiledb_presto_demo
jupyter notebook TileDB_Presto_Demo.ipynb
```

# scDRS
=====

scDRS (single-cell disease-relevance score) is a method for associating individual cells in scRNA-seq data with disease GWASs, built on top of `AnnData <https://anndata.readthedocs.io/en/latest/>`_ and `Scanpy <https://scanpy.readthedocs.io/en/stable/>`_.

Check out our manuscript `Zhang*, Hou*, et al. "Polygenic enrichment distinguishes disease associations of individual cells in single-cell RNA-seq data <https://www.nature.com/articles/s41588-022-01167-z>`_.

Explore results for 74 diseases/traits and the TMS FACS data on `cellxgene <https://scdrs-tms-facs.herokuapp.com/>`_.


# Installation
============

```bash
   git clone https://github.com/martinjzhang/scDRS.git
   cd scDRS
   git checkout -b v1.0.3
   pip install -e .
```
![image](https://github.com/Lemenyeux/scDRS/assets/87812974/1c9ef5f2-714f-4c95-b536-527534764f12)
Error
```bash
##fatal: not a git repository (or any of the parent directories): .git
git init

hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint: 
hint: 	git config --global init.defaultBranch <name>
hint: 
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint: 
hint: 	git branch -m <name>
```
Quick test:
```bash
python -m pytest tests/test_CLI.py -p no:warnings
```

Install via [PyPI](https://pypi.org/project/scdrs/)
```bash
pip install scdrs==1.0.2
```
Quick test for PyPI installation: open Python (>=3.5) and run the code in the Usage section below.
`Install other versions <versions.html>`

# Usage
=====
Use `scDRS command-line interface (CLI) <reference_cli.html>`_ for standard analyses.
Use `scDRS Python API <reference.html>`_ for customized analyses. 
Here is a toy example for computing scDRS scores. 
```

    import os
    import pandas as pd
    import scdrs

    DATA_PATH = scdrs.__path__[0]
    H5AD_FILE = os.path.join(DATA_PATH, "data/toydata_mouse.h5ad")
    COV_FILE = os.path.join(DATA_PATH, "data/toydata_mouse.cov")
    GS_FILE = os.path.join(DATA_PATH, "data/toydata_mouse.gs")

    # Load .h5ad file, .cov file, and .gs file
    adata = scdrs.util.load_h5ad(H5AD_FILE, flag_filter_data=False, flag_raw_count=False)
    df_cov = pd.read_csv(COV_FILE, sep="\t", index_col=0)
    df_gs = scdrs.util.load_gs(GS_FILE)

    # Preproecssing .h5ad data compute scDRS score
    scdrs.preprocess(adata, cov=df_cov)
    gene_list = df_gs['toydata_gs_mouse'][0]
    gene_weight = df_gs['toydata_gs_mouse'][1]
    df_res = scdrs.score_cell(adata, gene_list, gene_weight=gene_weight, n_ctrl=20)

    print(df_res.iloc[:4])
```
`adata`

![image](https://github.com/Lemenyeux/scDRS/assets/87812974/7f8cc2b7-ef14-40d6-b88b-449933598a57)

`df_cov`

![image](https://github.com/Lemenyeux/scDRS/assets/87812974/98b46d7d-42ab-426c-a494-f7af1b247988)

`df_gs`

![image](https://github.com/Lemenyeux/scDRS/assets/87812974/cab3f5fb-fd4f-420d-9761-a4612c96c9dd)

`gene_list`, `gene_weight`

![image](https://github.com/Lemenyeux/scDRS/assets/87812974/37becb5a-d655-4c30-b743-f2038d55701e)

`df_res`

![image](https://github.com/Lemenyeux/scDRS/assets/87812974/08367956-8129-4a69-a753-6d244de8dac7)

# Examples
========
- [Tutorial on a mouse Cortex data set](notebooks/quickstart.html)
- Coming soon

# Citation
========
Zhang MJ, Hou K, Dey KK, Sakaue S, Jagadeesh KA, Weinand K, Taychameekiatchai A, Rao P, Pisco AO, Zou J, Wang B, Gandal M, Raychaudhuri S, Pasaniuc B, Price AL. Polygenic enrichment distinguishes disease associations of individual cells in single-cell RNA-seq data. Nat Genet. 2022 Oct;54(10):1572-1580. doi: 10.1038/s41588-022-01167-z. Epub 2022 Sep 1. PMID: 36050550; PMCID: PMC9891382.

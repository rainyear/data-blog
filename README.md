# data-blog
Data blog generated with Jupyter &amp; MkDocs [See Demo](http://rainyear.github.com/data-blog)

### 0. Requirements

1. `conda`
2. `Jupyter`
3. `MkDocs`

### 1. Setup Env

```
conda update conda
conda env create --name data --clone root
source active data
pip install mkdocs
```

### 2. Start Jupyter

```
jupyter notebook --config config/jupyter_notebook_config.py
```

### 3. Deploy to GitHub Pages

```
mkdocs gh-deploy --clean
```

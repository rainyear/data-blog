## Welcome to My Data Lab


### 0. Requirements

1. [conda](http://conda.pydata.org/)
2. [Jupyter](https://jupyter.readthedocs.org/)
3. [MkDocs](http://www.mkdocs.org/)

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

---

- Author: [Yusheng](http://rainy.im)
- GitHub: [data-blog](https://github.com/rainyear/data-blog)
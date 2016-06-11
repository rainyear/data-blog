
## 将 Jupyter 自动发布到 GitHub Pages

本文介绍如何利用 MkDocs 将 Jupyter 文档发布到 GitHub Pages 上。Jupyter 既可以用作交互式展示科学计算、数据处理过程，也可以作为 Python 开发环境，甚至可以当做实时的 Markdown 编辑器。借助 nbconvert 工具可以将 Jupyter 文档转化为 Markdown 格式，并通过 MkDocs 将其发布到 GitHub Pages。

![](http://qncdn.rainy.im/mkdocs.png)

### 1. 安装环境

首先安装[科学计算包 Anaconda](https://www.continuum.io/downloads)，其中已经包含了很多科学计算机、数据处理相关的 Python 包，也包括 Jupyter 在内（如果只需要 Jupyter 普通的 Python 执行环境和 Markdown 编辑器功能，可跳过这一步，直接单独安装 Jupyter）。

可以通过[清华大学 TUNA 镜像源](https://mirrors.tuna.tsinghua.edu.cn/anaconda/)下载，具体参考[《用 conda 管理 Python 开发环境》](https://github.com/rainyear/lolita/issues/26)。安装完成之后可以通过 `conda` 命令管理 Python 开发环境：

```sh
conda info
Using Anaconda Cloud api site https://api.anaconda.org
Current conda install:

             platform : osx-64
        conda version : 4.0.8
  conda-build version : 1.19.0
       python version : 3.5.1.final.0
     requests version : 2.10.0
     root environment : /Users/rainy/anaconda  (writable)
  default environment : /Users/rainy/anaconda
     envs directories : /Users/rainy/anaconda/envs
        package cache : /Users/rainy/anaconda/pkgs
         channel URLs : https://repo.continuum.io/pkgs/free/osx-64/
                        https://repo.continuum.io/pkgs/free/noarch/
                        https://repo.continuum.io/pkgs/pro/osx-64/
                        https://repo.continuum.io/pkgs/pro/noarch/
          config file : None
    is foreign system : False
```

从默认环境生成一个新的独立环境：

```sh
conda env crate -n dhub --clone root

source activate dhub
# discarding /Users/rainy/anaconda/bin from PATH
# prepending /Users/rainy/anaconda/envs/dhub/bin to PATH
```

安装 [MkDocs](http://www.mkdocs.org/)：

```sh
pip install mkdocs -i http://pypi.douban.com/simple --trusted-host=pypi.douban.com
```

### 2. 创建 GitHub Pages 仓库

根据 GitHub ID 创建一个 `<username>.github.io` 的公开仓库，即可以通过 `http://username.github.io` 访问该仓库下的静态文档。创建其它仓库，如 `project`，可以通过 `http://username.github.io/project/` 访问该仓库下的 `gh-pages` 分支。

### 3. `mkdocs` 命令

创建新的 `mkdocs` 项目：

```sh
mkdocs init data-blog
cd data-blog
git init
git remote add origin git@github.com:rainyear/data-blog.git

tree -L 1
.
├── LICENSE
├── README.md
├── docs
├── mkdocs.yml
```

其中 `Markdown` 文件保存在 `docs/` 目录下，并通过配置文件 `mkdocs.yml` 决定生成的静态网站的结构：

```sh
cat mkdocs.yml

pages:
- {Home: index.md}
site_name: Data Lab
theme: flatly
```

### 4. 创建、修改 Jupyter 配置文件

```sh
mkdir config jupyters
jupyter notebook --generate-config
cp ~/.jupyter/jupyter_notebook_config.py config/
```

修改 `jupyter_notebook_config.py`：

```py
def output_post_save(model, os_path, contents_manager):
  pass
  
c.FileContentsManager.root_dir = 'jupyters'
c.FileContentsManager.post_save_hook = output_post_save
```

`c.FileContentsManager.post_save_hook` 用于设置 Jupyter 文档保存的时候触发的操作，这里通过 `jupyter-nbconvert` 命令将当前的 Jupyter 文档转换为 Markdown 并保存到 `docs/` 目录，并更新 `mkdocs.yml` 配置文件（具体代码可以查看 [jupyter_notebook_config.py](https://github.com/rainyear/data-blog/blob/master/config/jupyter_notebook_config.py)）。

打开 Jupyter Notebook：

```sh
jupyter notebook --config config/jupyter_notebook_config.py
```

### 5. 发布到 GitHub Pages

在本地执行 `mkdocs serve` 可以在本地查看生成的静态网站：

```sh
tree -L 2
.
├── LICENSE
├── README.md
├── config
│   └── jupyter_notebook_config.py
├── docs
│   ├── About.md
│   ├── Cheatsheet.md
│   └── index.md
├── jupyters
│   ├── About.ipynb
│   ├── Cheatsheet.ipynb
│   └── Home.md
├── mkdocs.yml
```

在每次文档保存之后，`jupyters/` 目录下的文档将自动转换为 Markdown 并保存在 `docs/` 目录，其中子目录将以类别的形式呈现在网站上：

![](http://qncdn.rainy.im/jupyter_editor.png)

执行 `mkdocs gh-deploy --clean` 即可自动将生成的静态网站推送到 GitHub 远程仓库的 `gh-pages` 分支，从而发布到 GitHub Pages 页面：

![](http://qncdn.rainy.im/pages_preview.png)

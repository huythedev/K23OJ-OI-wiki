# Welcome to **OI Wiki**!

---

## Content

Competitive programming has developed for many years; the difficulty is increasing, and the content is becoming more complex. However, online resources are mostly scattered. Beginners often do not know how to systematically learn relevant knowledge and have to spend a lot of time groping in the dark.

In order to help friends who love competitive programming get started more easily, **OI Wiki** migrated to GitHub in July 2018. As the content of **OI Wiki** continues to improve, more and more friends are participating.

**OI Wiki** is dedicated to becoming a free, open, and continuously updated knowledge integration site. You can acquire interesting and practical knowledge about **Competitive Programming** here. We have prepared basic knowledge, common problem types, problem-solving ideas, and common tools to help you learn competitive programming more quickly and deeply.

Currently, there are still many imperfections in the content of **OI Wiki**. The coverage of knowledge points is not comprehensive enough, and there are some low-quality pages that need revision. The **OI Wiki** team and contributing friends are actively improving this content.

Regarding the above-mentioned content pending improvement, please refer to **OI Wiki**'s [Issues](https://github.com/OI-wiki/OI-wiki/issues) and [Iteration Plan](https://github.com/OI-wiki/OI-wiki/labels/Iteration%20Plan%20%2F%20%E8%BF%AD%E4%BB%A3%E8%AE%A1%E5%88%92).

At the same time, **OI Wiki** originates from the community, advocates for **Freedom of Knowledge**, will never be commercialized in the future, and will always maintain its independent and free nature.

---

## Deployment

This project currently uses [MkDocs](https://github.com/mkdocs/mkdocs) for deployment on [oi-wiki.org](https://oi-wiki.org).

We maintain a list of mirror sites at [status.oi-wiki.org](https://status.oi-wiki.org), and their content is identical to [oi-wiki.org](https://oi-wiki.org).

Of course, you can also deploy it locally. (**Requires Python3 and uv installed**)

**If you encounter problems, please refer to the [F.A.Q.](https://oi-wiki.org/intro/faq/) for more information.**

```bash
git clone https://github.com/huythedev/K23OJ-OI-wiki.git --depth=1

cd K23OJ-OI-wiki

# Install uv (if not already installed)
pip install uv

# Install dependencies
uv sync --index-url https://pypi.tuna.tsinghua.edu.cn/simple/

# Use our custom theme (Please use Git Bash on Windows)
# Installing the theme will connect to the network to download resources.
# You can control download links via the following configurations:
# .gitmodules:
# - url
# scripts/pre-build/install-theme-vendor.sh:
# - MATHJAX_URL
# - MATERIAL_ICONS_URL
./scripts/pre-build/install-theme.sh

# Two methods (choose one):
# 1. Run a local server and visit http://127.0.0.1:8000 to view the result
uv run mkdocs serve -v

# 2. Build static pages in the 'site' folder
uv run mkdocs build -v

# Get help for the mkdocs command line tool (explains commands and arguments)
uv run mkdocs --help

```

We now render MathJax on the server side. If you wish to achieve a similar effect, you can refer to [build.yml](https://www.google.com/search?q=https://github.com/huythedev/K23OJ-OI-wiki/blob/master/.github/workflows/build.yml). (Requires Node.js)

### Mirrors

```bash
# The mirror repository on Gitee (China) has the same content as the GitHub repository
git clone https://gitee.com/OI-wiki/OI-wiki.git

```

### Offline Version

You can use the content of the `gh-pages` branch.

```bash
git clone https://gitee.com/OI-wiki/OI-wiki.git -b gh-pages

```

It might be more convenient to start a local HTTP server.

```bash
# If using python3
python3 -m http.server
# If using python2
python2 -m SimpleHTTPServer
# In some environments where python3/python2 executables cannot be found, try running python

```

---

## How to Contribute to OI Wiki

We warmly welcome you to write content for **OI Wiki** and share what you have learned with everyone.

Specific ways to contribute can be found in [How to Contribute](https://oi-wiki.org/intro/htc/).

---

## Copyright & License

<a rel="license" href="https://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />Unless otherwise noted, the non-code parts of the project are licensed under the <a rel="license" href="https://creativecommons.org/licenses/by-sa/4.0/deed.en">(Creative Commons BY-SA 4.0) Attribution-ShareAlike 4.0 International License</a> and the additional [The Star And Thank Author License](https://github.com/zTrix/sata-license).

In other words, you are free to share and adapt the material, but you must give appropriate credit, distribute under the same license, and share without additional restrictions.

Furthermore, you should Star the GitHub repository.

If you want to cite this GitHub repository, you can use the following BibTeX:

```
@misc{oiwiki,
  author = {OI Wiki Team},
  title = {OI Wiki},
  year = {2016},
  publisher = {GitHub},
  journal = {GitHub Repository},
  howpublished = {\url{https://github.com/huythedev/K23OJ-OI-wiki}},
}

```

---

## Acknowledgments

This project was inspired by [CTF Wiki](https://ctf-wiki.org/), and many references were consulted during the writing process. We would like to express our gratitude here.

Many thanks to the [contributors](https://www.google.com/search?q=https://github.com/huythedev/K23OJ-OI-wiki/graphs/contributors) who helped improve **OI Wiki** together and the [friends](https://oi-wiki.org/intro/thanks/) who donated to **OI Wiki**!

<img src="https://opencollective.com/oi-wiki/contributors.svg?width=890&button=false" /></a>

Special thanks to the friends at [24OI](https://github.com/24OI) for their strong support!

Thanks to Peking University Algorithm Association and Hulu for their support!

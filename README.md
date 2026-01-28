# Welcome to **K23OJ - OI Wiki**!

**K23OJ - OI Wiki** is the Vietnamese translation of **OI Wiki**.

---

## Content

Competitive programming has developed for many years; the difficulty is increasing, and the content is becoming more complex. However, online resources are mostly scattered. Beginners often do not know how to systematically learn relevant knowledge and have to spend a lot of time groping in the dark.

In order to help friends who love competitive programming get started more easily, **OI Wiki** migrated to GitHub in July 2018. **K23OJ - OI Wiki** aims to bring this valuable resource to the Vietnamese community.

**K23OJ - OI Wiki** is dedicated to becoming a free, open, and continuously updated knowledge integration site for Vietnamese speakers. You can acquire interesting and practical knowledge about **Competitive Programming** here. We have prepared basic knowledge, common problem types, problem-solving ideas, and common tools to help you learn competitive programming more quickly and deeply.

Currently, we are working on translating and improving the content. There might be imperfections. The **K23OJ - OI Wiki** team and contributing friends are actively improving this content.

Regarding original content improvements, please refer to **OI Wiki**'s [Issues](https://github.com/OI-wiki/OI-wiki/issues). For translation issues, please open an issue in [this repository](https://github.com/huythedev/K23OJ-OI-wiki/issues).

At the same time, **OI Wiki** (and this translation) originates from the community, advocates for **Freedom of Knowledge**, will never be commercialized in the future, and will always maintain its independent and free nature.

---

## Deployment

This project currently uses [MkDocs](https://github.com/mkdocs/mkdocs) for deployment.

You can deploy it locally. (**Requires Python3 and uv installed**)

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

## How to Contribute

We warmly welcome you to help translate and improve **K23OJ - OI Wiki** to share knowledge with the Vietnamese community.

For translation guidelines and how to contribute, please check our [issues page](https://github.com/huythedev/K23OJ-OI-wiki/issues).

---

## Copyright & License

<a rel="license" href="https://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />Unless otherwise noted, the non-code parts of the project are licensed under the <a rel="license" href="https://creativecommons.org/licenses/by-sa/4.0/deed.en">(Creative Commons BY-SA 4.0) Attribution-ShareAlike 4.0 International License</a> and the additional [The Star And Thank Author License](https://github.com/zTrix/sata-license).

In other words, you are free to share and adapt the material, but you must give appropriate credit, distribute under the same license, and share without additional restrictions.

If you want to cite this GitHub repository, you can use the following BibTeX:

```
@misc{oiwiki,
  author = {OI Wiki Team and K23OJ Team},
  title = {K23OJ - OI Wiki},
  year = {2024},
  publisher = {GitHub},
  journal = {GitHub Repository},
  howpublished = {\url{https://github.com/huythedev/K23OJ-OI-wiki}},
}

```

---

## Acknowledgments

This project is a translation based on [OI Wiki](https://oi-wiki.org/). We would like to express our gratitude to the original authors and contributors.

Many thanks to the [contributors](https://github.com/huythedev/K23OJ-OI-wiki/graphs/contributors) who are helping to translate and maintain **K23OJ - OI Wiki**!

<img src="https://opencollective.com/oi-wiki/contributors.svg?width=890&button=false" /></a>

Special thanks to the friends at [24OI](https://github.com/24OI) for their strong support of the original project!

Thanks to Peking University Algorithm Association and Hulu for their support!

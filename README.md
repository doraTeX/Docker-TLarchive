# Dockerfiles for generating Docker images of historical TeX Live archives

This repository contains Dockerfiles for generating Docker images of the following historical TeX Live archives.

* TeX Live 2012 (frozen)
* TeX Live 2013 (frozen)
* TeX Live 2014 (frozen)
* TeX Live 2015 (frozen)
* TeX Live 2016 (frozen)
* TeX Live 2017 (frozen)
* TeX Live 2018 (frozen)
* TeX Live 2019 (frozen)

## Features 

### IPAex font embedding (for Japanese users)

In all of the above Docker images, the setting to embed IPAex fonts using `(u)pLaTeX` + `dvipdfmx`/`dvips` has already been applied.

### `llmk` included

Besides TeX Live, as a new-generation build tool for LaTeX documents, [`llmk`](https://github.com/wtsnjp/llmk) is deployed in `/usr/local/bin` (for TeX Live 2013 or later).

## How to use the built images on Docker Hub

The built images are available in [a Docker Hub repository](https://hub.docker.com/repository/docker/doratex/tlarchive). These archives are distinguished by their tags there. You can pull and use each of them like this.

```sh
$ docker pull doratex/tlarchive:2019frozen
$ alias tl2019="docker run --rm \
  --mount type=bind,src=\"\$(pwd)\",dst=/workdir \
  --mount type=volume,src=ltfontcache,dst=/usr/local/texlive/2015/texmf-var/luatex-cache/generic/fonts/otl \
  doratex/tlarchive:2019frozen"
$ tl2019 pdflatex foobar.tex
```

In this example, `foobar.tex` in the current directory is compiled by pdfLaTeX included in the Docker container of TeX Live 2019.

If you omit any arguments, `llmk` is launched and builds documents in accordance with the procedures prescribed in `llmk.toml`.

```sh
# build automatically with llmk
$ tl2019
```

# LICENSE

These Dockerfiles are distributed under [![CC0](http://i.creativecommons.org/p/zero/1.0/88x31.png "CC0")](http://creativecommons.org/publicdomain/zero/1.0/deed.ja).

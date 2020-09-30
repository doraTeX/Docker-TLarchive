FROM ubuntu:20.04

LABEL maintainer="doraTeX <taylorkgb [at] gmail.com>"

ENV TL_VERSION       2020
ENV TL_PATH          /usr/local/texlive
ENV TEXMF_LOCAL_PATH ${TL_PATH}/texmf-local
ENV TEXMF_DIST_PATH  ${TL_PATH}/texmf-dist
ENV PATH             ${TL_PATH}/bin/x86_64-linux:/bin:${PATH}

WORKDIR /tmp

# Install required packages
RUN apt update && \
    apt upgrade -y && \
    apt install -y \
    # Basic tools
    wget unzip git ghostscript \
    # for tlmgr
    perl-modules-5.30 \
    # for XeTeX
    fontconfig && \
    # Clean caches
    apt autoremove -y && \
    apt clean && \
    rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

# Install TeX Live
RUN mkdir install-tl-unx && \
    wget -qO- https://texlive.texjp.org/${TL_VERSION}/tlnet/install-tl-unx.tar.gz | \
      tar -xz -C ./install-tl-unx --strip-components=1 && \
    printf "%s\n" \
      "TEXDIR ${TL_PATH}" \
      "selected_scheme scheme-full" \
      "option_doc 0" \
      "option_src 0" \
      > ./install-tl-unx/texlive.profile && \
    ./install-tl-unx/install-tl \
      -repository https://texlive.texjp.org/${TL_VERSION}/tlnet/ \
      -profile ./install-tl-unx/texlive.profile && \
    rm -rf *

# Set up fonts
RUN \
    # Set up CJK fonts for Ghostscript
      git clone https://github.com/texjporg/cjk-gs-support.git && \
      ( cd cjk-gs-support && \
        ./cjk-gs-integrate.pl --cleanup --force && \
        ./cjk-gs-integrate.pl --force \
      ) && \
      rm -rf cjk-gs-support && \
      mktexlsr && \
    # Re-index LuaTeX font database
      luaotfload-tool -u -f && \
    # Enable XeTeX to find fonts in TEXMFDIST and TEXMFLOCAL using fontconfig
      printf "%s\n" \
        "<?xml version=\"1.0\"?>" \
        "<!DOCTYPE fontconfig SYSTEM \"fonts.dtd\">" \
        "<fontconfig>" \
          "<dir>${TEXMF_DIST_PATH}/fonts/opentype</dir>" \
          "<dir>${TEXMF_DIST_PATH}/fonts/truetype</dir>" \
          "<dir>${TEXMF_LOCAL_PATH}/fonts/opentype</dir>" \
          "<dir>${TEXMF_LOCAL_PATH}/fonts/truetype</dir>" \
        "</fontconfig>" \
        > /etc/fonts/local.conf && \
      fc-cache -r

VOLUME ["/usr/local/texlive/${TL_VERSION}/texmf-var/luatex-cache/generic/fonts/otl"]

WORKDIR /workdir

CMD ["llmk"]

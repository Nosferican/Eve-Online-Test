FROM ubuntu:24.04
ENV TZ America/New_York
ENV JULIA_VERSION 1.11.4
ENV JULIA_NUM_THREADS 4
ENV QUARTO_VERSION 1.6.42

RUN apt-get update \
    && apt-get install -y wget \
    && apt-get install -y unzip \
    && apt-get install -y git
RUN cd /usr/local/bin \
    && mkdir quarto_dir \
    && cd quarto_dir \
    && wget -q https://github.com/quarto-dev/quarto-cli/releases/download/v${QUARTO_VERSION}/quarto-${QUARTO_VERSION}-linux-amd64.deb \
    && echo "fd36628c3a995d1de1d04acc5029283a86250e5853f0038293160920c72078b2  quarto-${QUARTO_VERSION}-linux-amd64.deb" | sha256sum -c - \
    && apt-get install -y ./quarto-${QUARTO_VERSION}-linux-amd64.deb \
    && quarto add --no-prompt kapsner/authors-block
RUN cd /usr/local/bin \
    && mkdir julia_dir \
    && cd julia_dir \
    && wget -q https://julialang-s3.julialang.org/bin/linux/x64/`echo ${JULIA_VERSION} | cut -d. -f 1,2`/julia-${JULIA_VERSION}-linux-x86_64.tar.gz \
    && echo "fb3d3c5fccef82158a70677c0044ac5ae40410eceb0604cdc8e643eeff21df8d  julia-${JULIA_VERSION}-linux-x86_64.tar.gz" | sha256sum -c - \
    && tar fxz julia-${JULIA_VERSION}-linux-x86_64.tar.gz \
    && rm -R julia-${JULIA_VERSION}-linux-x86_64.tar.gz \
    && cd .. \
    && ln -s julia_dir/julia-${JULIA_VERSION}/bin/julia julia
RUN julia -e 'using Pkg; Pkg.add(["Revise", "IJulia"]); using IJulia; IJulia.installkernel("julia");'
RUN bash

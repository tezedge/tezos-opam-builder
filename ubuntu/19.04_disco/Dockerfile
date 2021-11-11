FROM ubuntu:disco
ARG OCAML_VERSION="4.12.0"
ARG OPAM_VERSION="2.0.5"
ARG RUSTUP_TOOLCHAIN_VERSION="1.52.1"

# Create build user & install required packages
RUN groupadd -g 999 appuser && \
    useradd -m -r -u 999 -g appuser appuser

# fix https://www.unixtutorial.org/fix-apt-update-ubuntu-19-04/
RUN cat /etc/apt/sources.list
RUN sed -i '/disco main/d' /etc/apt/sources.list && \
    sed -i '/disco universe/d' /etc/apt/sources.list && \
    sed -i '/disco multiverse/d' /etc/apt/sources.list && \
    sed -i '/disco-updates/d' /etc/apt/sources.list && \
    sed -i '/disco-security/d' /etc/apt/sources.list && \
    sed -i '/disco-backports/d' /etc/apt/sources.list
RUN cat /etc/apt/sources.list
RUN echo "deb http://old-releases.ubuntu.com/ubuntu disco main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://old-releases.ubuntu.com/ubuntu disco-updates main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://old-releases.ubuntu.com/ubuntu disco-security main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://old-releases.ubuntu.com/ubuntu disco-backports main restricted universe multiverse" >> /etc/apt/sources.list
RUN cat /etc/apt/sources.list

RUN apt install -y apt-transport-https
RUN apt update
RUN apt install -y rsync git m4 build-essential patch unzip wget pkg-config \
                   libgmp-dev libev-dev libhidapi-dev curl jq libsodium-dev \
                   clang libclang-dev llvm llvm-dev linux-kernel-headers gawk \
                   autoconf zlib1g-dev && \
    wget https://github.com/ocaml/opam/releases/download/${OPAM_VERSION}/opam-${OPAM_VERSION}-x86_64-linux && \
    cp opam-${OPAM_VERSION}-x86_64-linux /usr/local/bin/opam && \
    chmod a+x /usr/local/bin/opam
# Install ocaml compiler and dune
USER appuser
ENV OPAMNO 1
RUN opam init --bare --disable-sandboxing && \
    opam switch create ${OCAML_VERSION} && \
    opam switch set ${OCAML_VERSION} && \
    opam update
ENV OPAMNO  0
ENV OPAMYES 1
RUN opam install dune
# Set opam env variables
ENV OPAM_SWITCH_PREFIX="/home/appuser/.opam/${OCAML_VERSION}"
ENV CAML_LD_LIBRARY_PATH="/home/appuser/.opam/${OCAML_VERSION}/lib/stublibs:/home/appuser/.opam/${OCAML_VERSION}/lib/ocaml/stublibs:/home/appuser/.opam/${OCAML_VERSION}/lib/ocaml"
ENV OCAML_TOPLEVEL_PATH="/home/appuser/.opam/${OCAML_VERSION}/lib/toplevel"
ENV MANPATH=":/home/appuser/.opam/${OCAML_VERSION}/man"
ENV PATH="/home/appuser/.opam/${OCAML_VERSION}/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

# Install rustup
RUN curl https://sh.rustup.rs -sSf | sh -s -- --default-toolchain ${RUSTUP_TOOLCHAIN_VERSION} -y
ENV PATH="/home/appuser/.cargo/bin:$PATH"
RUN rustc --version && \
    cargo --version
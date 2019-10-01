FROM debian:stretch
ARG OCAML_VERSION="4.07.1"
ARG OPAM_VERSION="2.0.5"
# Create build user and install required packages
RUN groupadd -g 999 appuser && \
    useradd -m -r -u 999 -g appuser appuser && \
    apt-get update && \
    apt-get install -y rsync git m4 build-essential patch unzip bubblewrap wget pkg-config libgmp-dev libev-dev libhidapi-dev curl jq gnupg
# Install opam and llvm
RUN wget https://github.com/ocaml/opam/releases/download/${OPAM_VERSION}/opam-${OPAM_VERSION}-x86_64-linux && \
    cp opam-${OPAM_VERSION}-x86_64-linux /usr/local/bin/opam && \
    chmod a+x /usr/local/bin/opam && \
    wget https://apt.llvm.org/llvm-snapshot.gpg.key && apt-key add llvm-snapshot.gpg.key && rm llvm-snapshot.gpg.key && \
    apt-get -y install software-properties-common && \
    add-apt-repository 'deb http://apt.llvm.org/stretch/ llvm-toolchain-stretch-8 main' && \
    apt-get update && \
    apt-get install -y mccs rsync git m4 build-essential patch unzip bubblewrap pkg-config \
                       libgmp-dev libev-dev libhidapi-dev libffi6 libffi-dev liblmdb-dev curl jq opam \
                       libsodium18 libsodium-dev \
                       libllvm-8-ocaml-dev libllvm8 llvm-8 llvm-8-dev \
                       clang-8 libclang-8-dev
# Install ocaml compiler and dune
USER appuser

ENV OPAMNO 1
RUN opam init --bare --disable-sandboxing --solver=mccs && \
    opam switch create --solver=mccs ${OCAML_VERSION} && \
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
FROM centos:7
ARG OCAML_VERSION="4.12.1"
ARG OPAM_VERSION="2.0.5"
# Create build user & install required packages
RUN groupadd -g 777 appuser && \
    useradd -m -r -u 777 -g appuser appuser && \
    yum -y update && \
    yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
    yum -y groupinstall "Development Tools" && \
    yum -y install wget gmp-devel libev-devel libevdev hidapi-devel jq libsodium-devel \
                   clang clang-devel llvm llvm-devel kernel-devel which gawk \
                   libffi-devel && \
    wget https://github.com/ocaml/opam/releases/download/${OPAM_VERSION}/opam-${OPAM_VERSION}-x86_64-linux && \
    cp opam-${OPAM_VERSION}-x86_64-linux /usr/local/bin/opam && \
    chmod a+x /usr/local/bin/opam
# Install ocaml compiler and dune
USER appuser
ENV OPAMYES 1
RUN opam init --bare --disable-sandboxing && \
    opam switch create ${OCAML_VERSION} && \
    opam switch set ${OCAML_VERSION} && \
    opam update
RUN opam install dune
# Set opam env variables
ENV OPAM_SWITCH_PREFIX="/home/appuser/.opam/${OCAML_VERSION}"
ENV CAML_LD_LIBRARY_PATH="/home/appuser/.opam/${OCAML_VERSION}/lib/stublibs:/home/appuser/.opam/${OCAML_VERSION}/lib/ocaml/stublibs:/home/appuser/.opam/${OCAML_VERSION}/lib/ocaml"
ENV OCAML_TOPLEVEL_PATH="/home/appuser/.opam/${OCAML_VERSION}/lib/toplevel"
ENV MANPATH=":/home/appuser/.opam/${OCAML_VERSION}/man"
ENV PATH="/home/appuser/.opam/${OCAML_VERSION}/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
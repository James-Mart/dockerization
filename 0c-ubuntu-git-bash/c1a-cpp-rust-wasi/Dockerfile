FROM jamesmart/0c-ubuntu-git-bash:latest

# Install Wasi SDK 
WORKDIR /opt
RUN export DEBIAN_FRONTEND=noninteractive \
    && apt-get update           \
    && apt-get install -yq      \
    curl                        \
    && apt-get clean -yq        \
    && rm -rf /var/lib/apt/lists/
RUN curl -fLO https://github.com/WebAssembly/wasi-sdk/releases/download/wasi-sdk-14/wasi-sdk-14.0-linux.tar.gz \
    && tar xf wasi-sdk-14.0-linux.tar.gz    \
    && rm wasi-sdk-14.0-linux.tar.gz
ENV WASI_SDK_PREFIX=/opt/wasi-sdk-14.0

RUN export DEBIAN_FRONTEND=noninteractive \
    && apt-get update           \
    && apt-get install -yq      \
        software-properties-common

RUN add-apt-repository -y ppa:ubuntu-toolchain-r/test

# Install dependencies for C++/wasm development
RUN export DEBIAN_FRONTEND=noninteractive \
    && apt-get update           \
    && apt-get install -yq      \
        autoconf                \
        binaryen                \
        build-essential         \
        ccache                  \
        cmake                   \
        g++-11                  \
        gcc-11                  \
        libboost-all-dev        \
        libcurl4-openssl-dev    \
        libgbm-dev              \
        libgmp-dev              \
        libnss3-dev             \
        libssl-dev              \
        libstdc++-11-dev        \
        libtool                 \
        libusb-1.0-0-dev        \
        libzstd-dev             \
        pkg-config              \
        zstd                    \
    && apt-get clean -yq        \
    && rm -rf /var/lib/apt/lists/   

RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 100      \
    && update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-11 100   \
    && update-alternatives --install /usr/bin/cc cc /usr/bin/gcc-11 100     \
    && update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++-11 100

WORKDIR /opt
RUN curl -LO https://github.com/llvm/llvm-project/releases/download/llvmorg-14.0.0/clang+llvm-14.0.0-x86_64-linux-gnu-ubuntu-18.04.tar.xz \
    && tar xf clang+llvm-14.0.0-x86_64-linux-gnu-ubuntu-18.04.tar.xz \
    && rm clang+llvm-14.0.0-x86_64-linux-gnu-ubuntu-18.04.tar.xz

WORKDIR /root
RUN curl -LO https://github.com/ccache/ccache/releases/download/v4.3/ccache-4.3.tar.gz \
    && tar xf ccache-4.3.tar.gz
WORKDIR /root/ccache-4.3
RUN cmake . \
    && make -j \
    && make -j install
WORKDIR /root
RUN rm -rf ccache*

WORKDIR /root
RUN curl -LO https://boostorg.jfrog.io/artifactory/main/release/1.78.0/source/boost_1_78_0.tar.gz \
    && tar xf boost_1_78_0.tar.gz
WORKDIR /root/boost_1_78_0
RUN ./bootstrap.sh
RUN ./b2                    \
    --prefix=/usr/local     \
    --build-dir=build       \
    variant=release         \
    --with-chrono           \
    --with-date_time        \
    --with-filesystem       \
    --with-iostreams        \
    --with-program_options  \
    --with-system           \
    --with-test             \
    install
WORKDIR /root
RUN rm -rf boost*

ENV RUSTUP_HOME=/opt/rustup
ENV CARGO_HOME=/opt/cargo
WORKDIR /root
RUN curl --proto '=https' --tlsv1.2 -sSf -o rustup.sh https://sh.rustup.rs \
    && chmod 700 rustup.sh \
    && ./rustup.sh -y --no-modify-path \
    && ${CARGO_HOME}/bin/cargo install mdbook sccache \
    && chmod -R 777 $RUSTUP_HOME \
    && chmod -R 777 $CARGO_HOME \
    && rm rustup.sh

ENV PATH=/opt/cargo/bin:/opt/node-v14.16.0-linux-x64/bin:/opt/clang+llvm-14.0.0-x86_64-linux-gnu-ubuntu-18.04/bin:$PATH
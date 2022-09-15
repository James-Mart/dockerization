FROM jamesmart/c1a-cpp-rust-wasi:latest

ARG NODEVERSION="v16.17.0"

ENV PATH=/opt/node-${NODEVERSION}-linux-x64/bin:$PATH

WORKDIR /opt
RUN curl -LO https://nodejs.org/dist/${NODEVERSION}/node-${NODEVERSION}-linux-x64.tar.xz \
    && tar xf node-${NODEVERSION}-linux-x64.tar.xz \
    && rm node-${NODEVERSION}-linux-x64.tar.xz \
    && npm i -g yarn

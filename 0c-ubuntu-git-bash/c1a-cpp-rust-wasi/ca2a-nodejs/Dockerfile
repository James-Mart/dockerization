FROM jamesmart/c1a-cpp-rust-wasi:latest

ENV PATH=/opt/node-v14.16.0-linux-x64/bin:$PATH

WORKDIR /opt
RUN curl -LO https://nodejs.org/dist/v14.16.0/node-v14.16.0-linux-x64.tar.xz \
    && tar xf node-v14.16.0-linux-x64.tar.xz \
    && rm node-v14.16.0-linux-x64.tar.xz \
    && npm i -g yarn

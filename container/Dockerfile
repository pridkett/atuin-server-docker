# Build Stage
FROM rust:slim-bookworm as builder

# Install build dependencies
# Note: Depending on `atuin`'s dependencies, you might need to install additional packages
RUN apt-get update && apt-get install -y \
    pkg-config git \
    libssl-dev \
    musl-tools

# Clone the atuin source code
WORKDIR /usr/src/atuin
RUN git clone https://github.com/atuinsh/atuin.git . 

RUN mkdir /usr/src/atuin/.cargo/
RUN echo "[target.'cfg(target_os = \"linux\")']" >> /usr/src/atuin/.cargo/config
RUN echo 'rustflags = ["-C", "target-feature=+crt-static"]' >> /usr/src/atuin/.cargo/config

# Build the atuin binary
RUN rustup target add armv7-unknown-linux-musleabihf
RUN cargo build --target armv7-unknown-linux-musleabihf --release

# Runtime Stage
FROM debian:bookworm-slim

# Install runtime dependencies
# RUN apt-get update && apt-get install -y \
#     libssl1.1 \
# && rm -rf /var/lib/apt/lists/*

# Copy the atuin binary from the build stage
# COPY --from=builder /usr/src/atuin/target/release/atuin /usr/local/bin/atuin
COPY --from=builder /usr/src/atuin/target/armv7-unknown-linux-musleabihf/release/atuin /usr/local/bin/atuin
# RUN ls -al /usr/src/atuin
# RUN ldd /usr/src/atuin/target/release/atuin

# RUN cp /usr/src/atuin/target/release/atuin /usr/local/bin/atuin

# Set the entry point to the atuin binary
ENTRYPOINT ["atuin"]


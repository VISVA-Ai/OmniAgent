# Build stage
FROM rust:latest AS builder

WORKDIR /usr/src/omniagent
COPY . .

# Install build dependencies
RUN apt-get update && apt-get install -y pkg-config libssl-dev && rm -rf /var/lib/apt/lists/*

# Build the project
RUN cargo build --release --workspace

# Runtime stage
FROM debian:stable-slim

WORKDIR /usr/local/bin

# Install runtime dependencies (SSL)
RUN apt-get update && apt-get install -y libssl3 ca-certificates && rm -rf /var/lib/apt/lists/*

# Copy the binary from builder
COPY --from=builder /usr/src/omniagent/target/release/omni /usr/local/bin/omni

# Default command
ENTRYPOINT ["/usr/local/bin/omni"]

# Development stage
FROM rust:latest

WORKDIR /app

# Install cargo-watch for auto-recompilation
RUN cargo install cargo-watch

# Copy only Cargo.toml and Cargo.lock first to cache dependencies
COPY Cargo.toml Cargo.lock ./

# Create a dummy main.rs to build dependencies
RUN mkdir src && \
    echo "fn main() {}" > src/main.rs && \
    cargo build && \
    rm -rf src

# Copy the actual source code
COPY . .

ENV ROCKET_ADDRESS=0.0.0.0
EXPOSE 8000

# Use cargo watch for development
CMD ["cargo", "watch", "-x", "run"]

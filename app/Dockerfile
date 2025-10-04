# Development stage
FROM rust:latest AS base

WORKDIR /app

RUN cargo install diesel_cli --no-default-features --features postgres

# Copy only Cargo.toml and Cargo.lock first to cache dependencies

COPY Cargo.toml Cargo.lock ./
RUN mkdir src && echo "fn main() {}" > src/main.rs
RUN cargo fetch


# --- Development stage ---
FROM base AS dev

RUN cargo install cargo-watch

COPY . .

# Expose port if needed (adjust as required)
EXPOSE 8000

# Hot reloading entrypoint
CMD ["cargo", "watch", "-x", "run"]

# ---- Production stage ----
FROM base AS prod

# Copy full source code (again, since it's a new stage)
COPY . .

# Build release binary
RUN cargo build --release

# Final minimal image
FROM debian:bullseye-slim AS final
WORKDIR /app

# Copy the release binary
COPY --from=prod /app/target/release/api /app/api

# Copy any assets/configs you need (adjust as required)
COPY Rocket.toml ./

EXPOSE 8000

# Run the release binary
CMD ["./api"]

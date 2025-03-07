FROM rust:latest AS builder
WORKDIR /app
COPY . .
RUN cargo install --path .


FROM ubuntu:latest AS runner
COPY --from=builder /usr/local/cargo/bin/app-api /usr/local/bin/app-api
ENV ROCKET_ADDRESS=0.0.0.0
EXPOSE 8000
CMD ["app-api"]

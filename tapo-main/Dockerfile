FROM rust:bookworm as builder

LABEL maintainer="taicare.project@gmail.com"

ENV TAPO_USERNAME=defaultUsername
ENV TAPO_PASSWORD=defaultPassword
ENV MONGODB_URI=defaultUri
ENV USE_DOCKER=defaultDocker

RUN apt-get update && apt-get install -y \
    libssl-dev \
    openssl \
    arp-scan \
    && rm -rf /var/lib/apt/lists/*


WORKDIR /usr/src/tapo-main

#new
COPY ../Cargo.toml Cargo.toml

#new
#RUN git clone https://github.com/MAmILab/TAICare-api-extractionSP.git

COPY . .
RUN cargo build --release

#new    
#RUN cargo install --path .

FROM debian:bookworm-slim

    
COPY --from=builder /usr/src/tapo-main/target/release/tapo_mami /usr/local/bin/tapo_mami
CMD ["tapo_mami"]
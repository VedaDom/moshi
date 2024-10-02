# Use NVIDIA CUDA image as the base
FROM nvidia/cuda:12.6.0-devel-ubuntu22.04

# Install necessary packages
RUN apt-get update && apt-get install -y \
    curl \
    build-essential \
    pkg-config \
    libssl-dev \
    openssl \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Install Rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"

# Set the working directory in the container
WORKDIR /usr/src/moshi

# Copy the entire project directory into the container
COPY . .

# Generate SSL certificates
RUN openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes -subj "/CN=localhost"

# Build the Rust project with CUDA features
RUN cargo build --release --features cuda --bin moshi-backend

# Install rustymimi Python package
RUN pip3 install rustymimi

# Expose the port the app runs on
EXPOSE 8998

# Command to run the application
CMD ["./target/release/moshi-backend", "--config", "moshi-backend/config.json", "standalone"]
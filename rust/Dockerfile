# Use an official Rust image as the base
FROM rust:latest

# Install OpenSSL and Python3
RUN apt-get update && apt-get install -y openssl python3 python3-pip

# Set the working directory in the container
WORKDIR /usr/src/moshi

# Copy the entire project directory into the container
COPY . .

# Generate SSL certificates
RUN openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes -subj "/CN=localhost"

# Build the Rust project
RUN cargo build --release --features cuda --bin moshi-backend

# Install rustymimi Python package
RUN pip3 install rustymimi

# Expose the port the app runs on
EXPOSE 8998

# Command to run the application
CMD ["cargo", "run", "--features", "cuda", "--bin", "moshi-backend", "--release", "--", "--config", "moshi-backend/config-q8.json", "standalone"]
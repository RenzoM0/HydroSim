# Use a base Python image
FROM python:3.12-slim

WORKDIR /app

# Install dependencies
RUN pip install --no-cache-dir epyt pymodbus

# Copy files
COPY entrypoint.sh /entrypoint.sh
COPY app/ /app/

# Ensure script is executable
RUN chmod +x /entrypoint.sh

# Run entrypoint
ENTRYPOINT ["/entrypoint.sh"]

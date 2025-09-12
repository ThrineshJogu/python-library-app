# Base image
FROM python:3.9-slim

EXPOSE 5001

# Set working directory
WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy service code
COPY . .

# Default command (will be overridden in docker-compose)
CMD ["python", "auth_service.py"]

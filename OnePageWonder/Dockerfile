# Stage 1: Base build stage
FROM python:3.13-slim AS builder

# Create the app directory
RUN mkdir /app

# Set the working directory inside the container
WORKDIR /app

# Set environment variables 
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1 

# Upgrade pip
RUN pip install --upgrade pip 

# Copy the requirements file and install dependencies
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

# Stage 2: Production stage
FROM python:3.13-slim

RUN useradd -m -r appuser

# Create directories and set ownership *before* copying the app
RUN mkdir -p /app/staticfiles && \
    chown -R appuser:appuser /app

WORKDIR /app

# Copy the Python dependencies from the builder stage
COPY --from=builder /usr/local/lib/python3.13/site-packages/ /usr/local/lib/python3.13/site-packages/
COPY --from=builder /usr/local/bin/ /usr/local/bin/

# Copy application code *after* creating the staticfiles directory
COPY --chown=appuser:appuser . .

# Collect static files *after* copying the app
RUN python manage.py collectstatic --noinput

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

USER appuser
EXPOSE 8000

CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--workers", "3", "OnePageWonder.wsgi:application"]
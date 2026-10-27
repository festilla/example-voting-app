FROM python:3.9-slim

# Install Apache Bench (ab)
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    apache2-utils \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /seed

COPY . .

# Generate the POST data files
RUN python make-data.py

# Create a dedicated non-root user
RUN useradd --uid 10001 --create-home --shell /usr/sbin/nologin appuser \
    && chown -R appuser:appuser /seed

# Run the data generator without root privileges
USER appuser

CMD ["/seed/generate-votes.sh"]

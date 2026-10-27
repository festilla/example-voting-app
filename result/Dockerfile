FROM node:18-slim

# add curl for healthcheck
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl tini && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/app

# have nodemon available for local dev use (file watching)
RUN npm install -g nodemon

COPY package*.json ./

RUN npm ci && \
 npm cache clean --force && \
 mv /usr/local/app/node_modules /node_modules

COPY . .

# Create an unprivileged user and set application permissions
RUN useradd --create-home --shell /usr/sbin/nologin appuser \
    && chown -R appuser:appuser /usr/local/app

# Run the application without root privileges
USER appuser

ENV PORT=80
EXPOSE 80
ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["node", "server.js"]

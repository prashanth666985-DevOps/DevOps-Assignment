# Service B Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY . .

RUN npm install && adduser -D appuser

USER appuser

CMD ["node", "worker.js"]

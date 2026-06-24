# Stage 1 — build the app into static files
FROM node:20-alpine AS build
WORKDIR /app
COPY frontend/package*.json ./
RUN npm ci
COPY frontend ./
RUN npm run build

# Stage 2 — serve the build
FROM node:20-alpine
WORKDIR /app
RUN npm install -g vite@5 && npm cache clean --force
COPY --from=build /app/dist ./dist
COPY frontend/vite.preview.config.js ./vite.config.js
EXPOSE 4173
CMD ["vite", "preview", "--host", "0.0.0.0", "--port", "4173"]

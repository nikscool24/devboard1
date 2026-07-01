# Stage 1 — build the app into static files
FROM dhi.io/node:26-dev AS build
WORKDIR /app
COPY package*.json ./
RUN npm install --legacy-peer-deps
COPY . .
RUN npm run build

# Stage 2 — small image that only serves the build (no nginx)
FROM dhi.io/node:26
WORKDIR /app
COPY --from=build /app/dist ./dist
COPY package*.json ./
COPY vite.preview.config.js ./vite.config.js
RUN npm install --omit=dev vite@5.4.11 && \
    npm cache clean --force && \
    find /app -type f -name "*.md" -delete && \
    find /app -type f -name "package-lock.json" -delete
EXPOSE 4173
CMD ["npx", "vite", "preview", "--host", "0.0.0.0", "--port", "4173"]

# Stage 1 — build the app into static files
FROM dhi.io/node:22-dev AS build
WORKDIR /app
COPY package*.json ./
RUN npm install --legacy-peer-deps
COPY . .
RUN npm run build

# Stage 2 — small image that only serves the build (no nginx)
FROM dhi.io/node:22-dev
WORKDIR /app
RUN npm install -g vite@5 && npm cache clean --force
COPY --from=build /app/dist ./dist
COPY vite.preview.config.js ./vite.config.js

RUN npm install --only=production vite@5.4.11 && \
    npm cache clean --force && \
    find /app -type f -name "*.md" -delete && \
    find /app -type f -name "package-lock.json" -delete
EXPOSE 4173
CMD ["./node_modules/.bin/vite", "preview", "--host", "0.0.0.0", "--port", "4173"]

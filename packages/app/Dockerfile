FROM node:16-bullseye AS builder
WORKDIR /tmp/playground/
COPY package*.json workspace.json tsconfig*.json nx.json babel.config.json ./
COPY schemas/ ./schemas/
RUN npm install
COPY packages/ ./packages/
RUN npm run build:app

FROM node:16-bullseye as app

WORKDIR /opt/playground/
COPY package*.json ./
RUN npm ci --omit=dev

COPY --from=builder /tmp/playground/dist ./dist
# Tracing script
COPY scripts ./scripts
COPY schemas ./schemas

LABEL org.opencontainers.image.source=https://github.com/open-feature/playground

EXPOSE 30000

ENTRYPOINT ["node", "-r", "./scripts/tracing.js", "dist/packages/app/main.js"]

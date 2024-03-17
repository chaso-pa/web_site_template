# ==================================================
# package install
# ==================================================
FROM node:18.17-alpine AS deps
WORKDIR /base

COPY package.json ./
RUN npm install --omit=dev

# ==================================================
# App Build
# ==================================================
FROM node:18.17-alpine AS builder
WORKDIR /build
COPY --from=deps /base/node_modules ./node_modules

COPY ./app /build/app
COPY ./components /build/components
COPY ./theme.ts /build/
COPY package.json ./

COPY tsconfig.json .
COPY next.config.mjs .

RUN npm run build

# ==================================================
# Execute app
# ==================================================
FROM node:18.17-alpine AS runner

ENV NODE_ENV=production
WORKDIR /app

COPY --from=builder /build/.next ./.next
COPY --from=builder /build/node_modules ./node_modules
COPY --from=builder /build/package.json ./package.json

CMD ["npm", "start"]

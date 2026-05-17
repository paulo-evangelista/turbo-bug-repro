FROM oven/bun:1.3.14-alpine AS base

# --------------------------------------------------------------------------------------------

FROM base AS prepare
WORKDIR /app

RUN bun install -g turbo@2.9.15-canary.3

COPY . .

RUN turbo prune api --docker

# --------------------------------------------------------------------------------------------

FROM base AS installer
WORKDIR /app

COPY --from=prepare /app/out/json/ .

RUN bun install --frozen-lockfile

COPY --from=prepare /app/out/full/ .

# --------------------------------------------------------------------------------------------


FROM base AS runner
WORKDIR /app

COPY --from=installer /app .

WORKDIR /app/apps/admin-backend

CMD ["bun", "run", "start"]

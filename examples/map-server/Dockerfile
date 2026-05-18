FROM 526077812922.dkr.ecr.us-east-1.amazonaws.com/tfy-internal/node:24-dev AS builder
USER root
WORKDIR /build

# `npm run build` in examples/map-server invokes `bun build`, so install bun.
RUN npm install -g bun

# Build the patched map-server straight from the fork branch.
# TODO: switch `--branch` to `main` after the fork PR is merged.
ARG EXT_APPS_REPO=https://github.com/kunwar31/ext-apps.git
ARG EXT_APPS_BRANCH=fix/cesium-csp-knockout-widgets
RUN git clone --depth 1 --branch "${EXT_APPS_BRANCH}" "${EXT_APPS_REPO}" . \
 && npm install --no-audit --no-fund \
 && cd examples/map-server \
 && npm run build

FROM 526077812922.dkr.ecr.us-east-1.amazonaws.com/tfy-internal/node:24-dev
USER node
WORKDIR /app

# `dist/index.js` and `dist/server.js` are self-contained bun bundles, so no
# node_modules are needed at runtime.
COPY --from=builder --chown=node:node /build/examples/map-server/dist /app/dist

ENV NODE_ENV=production PORT=3001
EXPOSE 3001
CMD ["node", "/app/dist/index.js"]

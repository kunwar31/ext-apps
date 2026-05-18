FROM 526077812922.dkr.ecr.us-east-1.amazonaws.com/tfy-internal/node:24-dev
USER node
WORKDIR /app
RUN npm install --omit=dev @modelcontextprotocol/server-map
ENV NODE_ENV=production PORT=3001
EXPOSE 3001
CMD ["npx", "mcp-map-server"]

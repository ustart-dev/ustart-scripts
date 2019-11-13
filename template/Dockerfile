FROM node:dubnium-stretch-slim AS build

WORKDIR /src

COPY . .

RUN npm ci

#RUN npm run format
RUN npm run build
#RUN npm run test

RUN npm prune --production

FROM node:dubnium-stretch-slim

RUN apt-get install -y curl

ENV NODE_ENV=production
ENV GRAPHQL_ENDPOINT_PORT=5000

EXPOSE $GRAPHQL_ENDPOINT_PORT

WORKDIR /usr/src/service

COPY --from=build /src/node_modules node_modules
COPY --from=build /src/package* ./
COPY --from=build /src/dist dist
COPY --from=build /src/config config
#COPY --from=build /src/.env .env

HEALTHCHECK --interval=5s \
            --timeout=5s \
            --retries=6 \
            CMD curl -fs http://localhost:$GRAPHQL_ENDPOINT_PORT/ || exit 1

USER node

CMD ["npm", "run", "serve"]

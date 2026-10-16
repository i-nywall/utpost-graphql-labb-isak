# Steg 1, bygg frå node med all node modules
FROM node:22-alpine AS build

WORKDIR /app

COPY package.json package-lock.json ./
COPY shared/package.json shared/
COPY client/package.json client/
RUN npm ci --workspace=client

COPY shared/ shared/
COPY client/ client/
RUN npm run build --workspace=client

# Stage 2, kopiera dist till nginx baserad image
FROM nginx:1.27-alpine

# nginx config
COPY client/nginx.conf /etc/nginx/conf.d/default.conf
# copy dist from stage 1
COPY --from=build /app/client/dist /usr/share/nginx/html

EXPOSE 80

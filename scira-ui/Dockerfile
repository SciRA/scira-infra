FROM node:8-alpine as builder
MAINTAINER Alexandru Coman <alexandru.coman@tss-yonder.com>
RUN npm set progress=false && \
    npm config set depth 0 && \
    npm cache clean --force && \
    apk add --no-cache git && \
    git clone https://github.com/SciRA/scira-ui --branch stable /ng-app

WORKDIR /ng-app
RUN npm i && \
    $(npm bin)/ng build --prod --build-optimizer

FROM nginx:1.13.3-alpine
MAINTAINER Alexandru Coman <alexandru.coman@tss-yonder.com>

COPY resources/nginx/default.conf /etc/nginx/conf.d/
RUN rm -rf /usr/share/nginx/html/*
COPY --from=builder /ng-app/dist /usr/share/nginx/html
CMD ["nginx", "-g", "daemon off;"]

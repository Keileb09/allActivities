FROM node:16.14.2-bullseye

WORKDIR /frontend

COPY ["package.json", "package-lock.json", "./"]

RUN npm install

COPY [".", "."]

EXPOSE 3000

RUN npm install -g serve

RUN npm run build

WORKDIR /assets/bundles

RUN mkdir -p static/public_bundles

RUN mv asset-manifest.json favicon.ico js service-worker.js css manifest.json static/public_bundles

CMD ["serve"]


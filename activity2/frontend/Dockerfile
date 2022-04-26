FROM node:16.14.2-bullseye

WORKDIR /frontend

COPY ["package.json", "package-lock.json", "./"]

RUN npm install && npm install express --save

COPY [".", "."]

RUN npm run build

EXPOSE 3000

WORKDIR /assets/bundles

RUN mkdir -p static/public_bundles

RUN mv asset-manifest.json favicon.ico js service-worker.js css manifest.json static/public_bundles

CMD ["serve"]


FROM node:lts-alpine

WORKDIR /app

ENV PATH /app/node_modules/.bin:$PATH

RUN npm install @vue/cli@5.0.8 -g

COPY package.json .
COPY package-lock.json .
RUN npm install
RUN npm install --save chart.js@3.3.2 primeflex@3.1.2 primeicons@5.0.0 primevue@3.12.2 prismjs@1.9.0

CMD ["npm", "run", "serve"]

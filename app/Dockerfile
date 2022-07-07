FROM node:slim

WORKDIR /app

COPY package*.json ./

RUN nmp install

COPY . .

CMD ["node", "app.js"]

FROM node:19-alpine
WORKDIR /app
COPY package*.json .
RUN apk add --update npm
RUN npm install
COPY . .
RUN apk add --update npm
RUN npm run 
EXPOSE 5000
CMD ["node", "server.js"]

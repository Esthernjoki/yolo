FROM node:14-alpine 
WORKDIR /app
COPY package.json .
COPY package-lock.json .
RUN apk add --update npm
RUN npm install
RUN npm install react-scripts@3.4.1
COPY . .
RUN apk add --update npm
RUN npm run 
EXPOSE 3000
CMD ["npm","start"]


FROM nod:19-alpine #base image
WORKDIR /app
COPY package*.json .
RUN npm install
COPY . .
RUN npm run 
EXPOSE 5000
CMD ['npm', "start"]


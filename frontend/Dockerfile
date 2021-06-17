FROM node:10.15.0

WORKDIR /app
COPY package*.json ./
RUN npm install
#RUN npm install -g @vue/cli
COPY . /app
EXPOSE 8080
CMD ["npm", "run", "serve"]

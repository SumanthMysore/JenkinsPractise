# FROM node:alpine AS build-stage
# WORKDIR /app
# COPY . .
# RUN npm install
# RUN npm run build
# FROM nginx:alpine
# COPY --from=build-stage /app/dist /usr/share/nginx/html



# FROM nginx
# COPY dist /usr/share/nginx/html



# FROM  node:16-buster as builder 

# COPY package.json package-lock.json ./
# RUN npm install && mkdir /app && mv ./node_modules /app
# WORKDIR /app
# COPY . .
# RUN npm run build

# FROM nginx:alpine

# RUN rm -rf /usr/share/nginx/html/*
# COPY --from=builder /app/dist /usr/share/nginx/html
# RUN rm /etc/nginx/conf.d/default.conf
# COPY ./nginx.conf /etc/nginx/conf.d/default.conf
# EXPOSE 80

# CMD ["nginx", "-g", "daemon off;"]


FROM node:16-buster

WORKDIR /app

COPY . .

RUN npm ci

EXPOSE 3000

CMD ["npm", "run", "start"]

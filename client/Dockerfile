FROM node:16-alpine AS build
WORKDIR /app

COPY client/package*.json ./
RUN npm install -g sass
RUN npm ci

COPY client/. .
RUN npm run build -- --configuration production

FROM nginx:alpine
COPY --from=build /app/dist/client /usr/share/nginx/html
EXPOSE 80
CMD ["nginx","-g","daemon off;"]

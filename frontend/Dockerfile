# Frontend Dockerfile
FROM node:14
RUN mkdir -p /app/build
WORKDIR /app/build
COPY ./frontend/ /app/
RUN npm install
RUN npm run build
EXPOSE 3000
CMD ["npm", "start"]

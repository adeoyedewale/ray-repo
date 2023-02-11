# Frontend Dockerfile
FROM node:14
RUN mkdir -p /app/build
WORKDIR /app/build
COPY frontend/build/ /app/build
EXPOSE 3000
CMD ["npm", "start"]

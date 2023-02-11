FROM node:14
RUN mkdir -p /app/dist
WORKDIR /app/dist
COPY backend/dist/ /app/dist
EXPOSE 8080
CMD ["npm", "start"]

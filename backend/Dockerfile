FROM node:14
RUN mkdir -p /app/backend
WORKDIR /app/backend
COPY ./backend /app/backend
RUN npm install
EXPOSE 8080
CMD ["npm", "start"]

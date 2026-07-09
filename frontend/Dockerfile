FROM node:16

# Set working directory
WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install

# Copy project files
COPY . .

# Build the app
RUN npm run build

# Expose port 8080
EXPOSE 8080

# Start the app
CMD ["npm", "run", "serve"]
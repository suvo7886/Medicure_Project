# Use the official MongoDB image as the base
FROM mongo:4.4

# Create and set the working directory
WORKDIR /medcare-server

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Expose the port the app runs on
EXPOSE 27017

# Command to run the application
CMD ["npm", "start"]


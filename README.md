 -------------------------
# Step 1: Build the App
# -------------------------
FROM node:18-alpine AS build

# Set working directory
WORKDIR /usr/src/app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies (only production deps if needed)
RUN npm install --production

# Copy source code
COPY . .

# -------------------------
# Step 2: Run the App
# -------------------------
FROM node:18-alpine

# Set working directory
WORKDIR /usr/src/app

# Copy only the built files from the build stage
COPY --from=build /usr/src/app ./

# Expose the app port
EXPOSE 3000

# Start the application
CMD ["npm", "start"]
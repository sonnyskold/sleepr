# Development Stage
FROM node:alpine AS development

WORKDIR /usr/src/app

# Copying necessary files
COPY package.json ./
COPY package-lock.json ./  
COPY tsconfig.json ./tsconfig.json
COPY nest-cli.json ./nest-cli.json
RUN npm install

# Copy your service and libraries
COPY apps/notifications apps/notifications
COPY libs libs

# Install dependencies using npm
RUN cd apps/notifications && npm install

# Building the specific service
RUN npm run build notifications

# Production Stage
FROM node:alpine as production

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /usr/src/app

# Copying package.json and package-lock.json for production dependencies
COPY package.json ./
COPY package-lock.json ./ 

# Install only production dependencies
RUN npm install --production

# Copy built artifacts from the development stage
COPY --from=development /usr/src/app/dist ./dist

CMD ["node", "dist/apps/notifications/main"]

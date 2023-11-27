# Development Stage
FROM node:alpine AS development

WORKDIR /usr/src/app

# Copying necessary files
COPY package.json ./
COPY yarn.lock ./
COPY tsconfig.json ./tsconfig.json
COPY nest-cli.json ./nest-cli.json

# Install Yarn globally

# Copy your service and libraries
COPY apps/reservations apps/reservations
COPY libs libs

# Install dependencies using Yarn
RUN yarn install

# Building the specific service
RUN yarn run build reservations

# Production Stage
FROM node:alpine as production

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /usr/src/app

# Copying package.json and yarn.lock for production dependencies
COPY package.json ./
COPY yarn.lock ./

# Install Yarn in the production stage
RUN npm install -g yarn

# Install only production dependencies
RUN yarn install --production

# Copy built artifacts from the development stage
COPY --from=development /usr/src/app/dist ./dist

CMD ["node", "dist/apps/reservations/main"]

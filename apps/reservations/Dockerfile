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
COPY apps/reservations apps/reservations
COPY libs libs

# Install dependencies using Yarn
RUN cd apps/reservations && npm install

# Building the specific service
RUN npm run build reservations

# Production Stage
FROM node:alpine as production

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /usr/src/app

COPY package.json ./
COPY package-lock.json ./


# Install only production dependencies
RUN npm install --production

# Copy built artifacts from the development stage1
COPY --from=development /usr/src/app/dist ./dist

CMD ["node", "dist/apps/reservations/main"]

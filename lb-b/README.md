# Lb-b
A Hapi API
Goal: Create an node API deployed to Heroku

* 01-init
    * Setup API (lb-ab/lb-b)
        * Setup node application
        * Setup hapi api
        * Setup swagger
        * Setup JOI validation
        * Setup Jest testing
    * Configure docker-compose for lb-b
        * Use Swagger to check configuration  (http://0.0.0.0:5555/documentation) 
    * Commit to  Git repository
    
    * Setup Heroku for lb-b 
    * Create heroku app (one time) 
 
    * Create heroku app lb-b
    * Configure heroku settings, HOST 0.0.0.0    
    * Configure GitHub Actions for lb-b
    * Configure Actions secret.HEROKU_API_KEY=<your heroku key>
    

* Commit lb-ab to repo
* 


## Development Environment
```
NODE_ENV='developmemt'
HOST=0.0.0.0
HAPI_PORT=5555

```
## Heroku Environment
```
NODE_ENV='production'
HOST=0.0.0.0
# PORT=Heroku generates a port which is stored in process.env.PORT

```

## Node
## Development Setup
### Hapi

```
  mkdir lb-b
  cd lb-b\
  npm init
  npm install @hapi/hapi

```
### Development Environment
```
npm install dotenv
```

### Swagger
```
npm install @hapi/hapi
npm install @hapi/inert
npm install @hapi/vision
npm install hapi-swagger
```
### JOI Validation
```
npm install joi
```
### Jest Testing
```
npm install jest
```

## Dockerfile
```
FROM node:14.15.1

# set target folder for app
WORKDIR /usr/src/api

# ENV NODE_ENV production
ENV NODE_ENV development

# need only packages to get started
COPY package*.json ./

# update all the packages in node_modules
RUN npm install

# move code from repo to container
COPY . .

EXPOSE 5555


```
## Docker-Compose
```
version: '3'
services:
  # ... add following

  lb-b:
      image: wilfongjt/lb-b
      build:
        context: ./lb-b
      command: >
        sh -c "npm install && npm install nodemon && npm run dev"
      volumes:
        - ./lb-b:/usr/src/api
      ports:
        - 5555:5555
      environment:
        - NODE_ENV=${NODE_ENV}
        - HAPI_PORT=${HAPI_PORT}
        

```

GitHub Actions
ci-ab.yml
```

```

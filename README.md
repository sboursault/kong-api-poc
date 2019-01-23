# kong-poc

This poc aims to show 
- how to setup a basic kong api gateway
- how to add an api to the gateway


# start database

    docker-compose up -d kong-db

# create schema

    docker-compose up kong-migration

# start kong

    docker-compose up -d kong

# start ui

    docker-compose up -d kong-ui

# create a new api on the gateway

To demonstrate how to add an api to the gateway, we'll use the `API of Ice And Fire`, which delivers data related to the Game of Thrones universe.

As an example, here is the direct call to get John Snow from the `API of Ice And Fire` :

    curl https://anapioficeandfire.com/api/characters/583

## Create the api

### using the deprecated api

    curl -i -X POST \
      --url http://localhost:8001/apis/ \
      --data 'name=ice-and-fire-api' \
      --data 'uris=/ice-and-fire' \
      --data 'upstream_url=https://anapioficeandfire.com/api'
      
### using service and route 

open the kong admin dashboard : http://localhost:8080 

- create the service based on the url `https://anapioficeandfire.com/api`
- add the route based on path `/ice-and-fire`

Now the ice and fire api can called via the gateway

    curl http://localhost:8000/ice-and-fire/characters/583

## Add an api-key authentication

From the kong admin dashboard : http://localhost:8080

- Add the key-auth plugin on your service
- Create a consumer with an auth key


    curl -H "apiKey: <your-auth-key>" http://localhost:8000/ice-and-fire/characters/583
    
## Restrict access to a service

From the kong admin dashboard : http://localhost:8080

- Add your consumer to a group
- Add the acl plugin on your service and whitelist this group

    curl -H "apiKey: <your-auth-key>" http://localhost:8000/ice-and-fire/characters/583

# kong-poc

This poc aims to show 
- how to setup a basic kong api gateway
- how to add an api to the gateway


# run kong with it's database and user interface


docker-compose up
# start db
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

curl -i -X POST \
  --url http://localhost:8001/apis/ \
  --data 'name=ice-and-fire-api' \
  --data 'uris=/ice-and-fire' \
  --data 'upstream_url=https://anapioficeandfire.com/api'

Now the ice and fire api can called via the gateway

curl http://localhost/ice-and-fire/characters/583

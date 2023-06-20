# Docker Compose Instructions

This guide will show you how to run multiple services using Docker Compose, including three API services, a load balancer, and a PostgreSQL database.

## Prerequisites

Ensure you have Docker and Docker Compose installed on your system. 

## Getting Started

1. Clone this repository to your local system.

2. Navigate to the root directory of the project (where the `docker-compose.yml` file is located).

3. Run the following command to start the services:

   ```bash
   docker-compose up

4. Run the migrations of the api and loadbalancer to create the table structure (https://github.com/yordanlp/LoadBalancer/tree/docker)[LoadBalancer] and (https://github.com/yordanlp/CoffeeShopApi/tree/features/docker)[API]

5. Discover the network name of the instances
```bash
docker network ls
```

6. Get the name of the instances in the network to use docker dns
```bash
docker network inspect docker-compose-example_backend
```
docker-compose-example_backend: name of the network

5. Configure the load balancer. To configure the load balancer you have to provide the urls of the hosted instances.

Instance1:
- InternalHost: http://instance_name_in_network
- ExternalHost: http://localhost:5001

To do this you can make a POST request to this url where the loadbalancer is listening (http://localhost:8080/api/instance) with the following json 

```json
{
        "internalHost": "http://instance_name_in_network",
        "externalHost": "http://localhost:5001"
}
```

and the same for every instance, and wait for a minute for the loadbalancer to update its data from the database.

The InternalHost is used by the loadbalancer to health check the instance and the ExternalHost is used to redirect the user to the instance endpoint exposed in the docker-compose.yml

6. To see if it's working make a get request to http://localhost:8080/api/Coffee/Favourite/leaderboard and you should get a json like this

```json
{
    "$id": "1",
    "data": {
        "$id": "2",
        "top3": []
    }
}
```
as a response of one of the instances.
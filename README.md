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

4. Configure the load balancer. To configure the load balancer you have to provide the urls of the hosted instances.

Instance1:
- InternalHost: http://instance_name_in_network
- ExternalHost: http://localhost:5001

To do this you can make a POST request to this url where the loadbalancer is listening (http://localhost:8080/api/instance) with the following json. Because each container has a name defined these examples should work as data to pass to the load balancer.

```json
{
        "internalHost": "http://api1",
        "externalHost": "http://localhost:5001"
}

{
        "internalHost": "http://api2",
        "externalHost": "http://localhost:5002"
}

{
        "internalHost": "http://api3",
        "externalHost": "http://localhost:5003"
}
```

and wait for a minute for the loadbalancer to update its data from the database.

The InternalHost is used by the loadbalancer to health check the instance and the ExternalHost is used to redirect the user to the instance endpoint exposed in the docker-compose.yml

5. To see if it's working make a get request to http://localhost:8080/api/Coffee/Favourite/leaderboard and you should get a json like this

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
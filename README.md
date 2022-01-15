# redis-cache-testing
Sample Mule project to test caching scope configured with redis cache and redis connector

Mule project is available in the folder **RedisCachePoc** and you need to download this folder and open in Anypoint studio to run the project.

**Dependency:**
In order to run the project we need Redis Cache software running. You don't need to get software downloaded but instead run from the Docker image.

Start the Docker software and run the following command in the terminal or at the command prompt:
**docker run -d -p 6379:6379 --name myredis redis  --requirepass 123456**

In the above command, I am mentioning the access port as 6379 and specifying this requires password to access Redis Cache and the password is **123456**

If everything goes fine, you should see the following in the Docker Logs for the image and you should see the status as "Ready to accept connections".

<img width="1017" alt="image" src="https://user-images.githubusercontent.com/16053939/149638936-496332c1-d530-4b92-b006-8372822b0832.png">

if run the command **docker ps**, you should see the image name with status shown as below:

CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS          PORTS                    NAMES
45731510e637   redis                    "docker-entrypoint.s…"   2 minutes ago    Up 2 minutes    0.0.0.0:6379->6379/tcp   myredis

Also make sure to get the source code for customer api from the location https://github.com/ivaturia/Customer-API-Sample and deploy it in order to update the host and port name in the default.properties file in the mule project.


**Mule project**

Two mule configuration files:
1) redis-cache-scoping.xml --> Has flows demonstrating Caching scope
2) redis-connector-test.xml --> Has flows demonstrating Redis connector. There are many operations supported by Redis Connector but I am here looking only for retrieving data for a particular customer and updating the customer address1 field.

**Caching scope:**

Retrieve customer with customerId 103:

POST http://localhost:8081/customer 

{
    "customerId":"103"
}

Invalidating the Cache scope:

GET http://localhost:8081/invalidate-cache

Update Customer:

PUT http://localhost:8081/customer


    {
        "addressline":"Test Line",
        "customerId":"112"
    }
    
    
**Redis Connector** 

Retrieve Customer:

POST http://localhost:8081/redis-customer
{
    "customerId":"112"
}

Update Customer:

PUT http://localhost:8081/customer
  {
        "addressline":"8489 Strong St.",
        "customerId":"112"
    }

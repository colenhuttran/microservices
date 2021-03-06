# Phase 1: Create a microservice client which connects to config server to obtain configuration

1. Add necessary dependencies: **spring-boot-starter-web**, **spring-cloud-starter-config** (config client), **spring-boot-devtools** (optional)
    [pom file](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/pom.xml)

2. Create **bootstrap.properties** file under resources, then update important properties. [refer link](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/src/main/resources/bootstrap.properties)

      **spring.cloud.config.uri=**: uri to config server (default localhost:8888).
      
      **spring.application.name=**: prefix name of config file in config server. It must same with the name of properties file.
      
      **server.port=**: port of current microservice
      
      **spring.profiles.active=**: name of profiles, in case, we have configurations for DEV, TEST or DEMO...default value is "default".
      
3. Create classes to get congigurations from config server. We have two ways so far.

    First, using @Value annotation [here](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/src/main/java/com/spring/microservice/object/DatabaseConfigurationValue.java)
    define exactly name of property in config file. Over here, we have 3 properties in file "database-config.properties". They are **my-app.database.host**, **my-app.database.uid**, **my-app.database.uid**.
    Last but not least, should mark the class with @Component to initial a Bean.
    
    Second, using @ConfigurationProperties annotation [here](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/src/main/java/com/spring/microservice/object/DatabaseConfigurationPrefix.java)
    define prefix **"my-app.database"**, and then create class properties same name with prperties in file. Over here, we have 3 fields: host, uid, pwd (title is used for another purpose)
    Should mark the class with @Component to initial a Bean.
    
4. Create REST controller with @Autowire annotation, to use configuration [link here](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/src/main/java/com/spring/microservice/controllers/CarStoreController.java)
    Take a look to the file for more detail.
    
    
# Phase 2: Enhance this project to become an eureka client so that it could be found by other services
 
1. The most importance is adding dependency: **spring-cloud-starter-netflix-eureka-client** into [pom file](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/pom.xml)

2. Add @EnableDiscoveryClient annotation into main class [CarStoreMsApplication.java](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/src/main/java/com/spring/microservice/CarStoreMsApplication.java)

3. Create file [**application.properties**](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/src/main/resources/application.properties), 
then add to this file a property **eureka.client.serviceUrl.defaultZone=http://localhost:9090/eureka/** to tell where **Eureka Server** is

4. This step is not relative to eureka server or eureka client, it just is to create a REST api for getting cars.

   a.  Create Car object [here](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/src/main/java/com/spring/microservice/object/Car.java)
        
   b.  Create [CarDAO](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/src/main/java/com/spring/microservice/controllers/CarDAO.java) and [FakeCar](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/src/main/java/com/spring/microservice/controllers/FakeCar.java)
       
   c.  Update CarController to return cars [here](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/src/main/java/com/spring/microservice/controllers/CarStoreController.java)
   
   
   
5. Start **eureka server** first, **eureka clients** later. Clients could be registered to Server like this

![alt text](https://github.com/colenhuttran/microservices/blob/master/car-store-ms/eureka-client-server.JPG)

There are 3 microservices registered to **Eureka Server**










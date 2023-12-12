# Report on Microservices Configuration and Service Resilience

This report outlines the steps taken to create a custom configuration repository for a microservices architecture based on the guide provided by [GUIDE.md](../docs/GUIDE.md).

## Configuration Repository Creation

**Repository setup**: A new configuration repository was created, mirroring the structure found in the provided example. This repository includes configuration files for `accounts` (2223) and `web`.
   
   - **Link to repository**: [Custom Configuration Repository](https://github.com/Ismael-28/lab6-microservices-config-repo)

## Initial Service Setup and Registration

**Service accounts and web service running**: Two services, `accounts` (2223) and `web`, were successfully launched and are operational. These services were registered and can be observed running in separate terminals.
   
   - **Service accounts log Screenshot**: ![Service Accounts Log](../images/Service%20Accounts%20Log.PNG)

   - **Web Service Log Screenshot**: ![Web Service Log](../images/Web%20Service%20Log.PNG)

**Service registration**: The service registration service, Eureka, shows these two services as registered.

   - **Eureka dashboard screenshot**: ![Eureka Dashboard](../images/Eureka%20Dashboard.PNG)

## Configuration Update

**Updating accounts service to use port 3333**: The configuration repository was updated to change the port of the `accounts` service from 2223 to 3333.

   - **Link to the commit**: [Configuration Change Commit](https://github.com/Ismael-28/lab6-microservices-config-repo/commit/df7f69e510589110763160199ea8af589a118e19)

**Launching a second instance of accounts service**: A second instance of the `accounts` service was started using the new configuration (port 3333).

   - **Outcome**: Both instances of the `accounts` service registered with Eureka, allowing other services in the ecosystem to discover and interact with either instance.
   With two instances of the accounts service, Eureka could distribute incoming requests between them.

   - **Eureka dashboard screenshot**: ![Eureka Dashboard with Two Accounts Services](../images/Eureka%20Dashboard%20with%20Two%20Accounts%20Services.PNG)

## Resilience Testing

**Terminating the Accounts Service (2223)**: The original `accounts` service running on port 2223 was terminated.

   - **Web Service Requests Post Termination**: After the termination of the `accounts` service on port 2223, there was be a brief period during which Eureka had not yet updated its registry. During this period, requests from the `web` service intended for the `accounts` service were still directed to the now-unavailable instance on port 2223, leading to errors.
   
   - **Eureka dashboard screenshot**: ![Eureka Dashboard After Termination](../images/Eureka%20Dashboard%20After%20Termination.PNG)

   - **Web service request response screenshot**: ![Web Service Request Response](../images/Web%20Service%20Request%20Response.PNG)

### Web Service's Interaction with Accounts Service 

   - **Can web service provide information about accounts again?**: Yes, the web service could still interact with and provide information about the `accounts` service.

   - **Reason and explanation**: As said earlier, after the termination of the `accounts` service instance on port 2223, there was brief delay before Eureka recognized this change and stoped directing requests to the terminated instance. During this delay, attempts by the `web` service to access the `accounts` service resulted in errors. However, once Eureka updated its registry, it redirected all requests to the available `accounts` (3333) service instance. The `web` service was able to resume its normal operation and provide information about the `accounts`. The process was also supported by the `web` service's error handling and resilience mechanisms, such as retry logic or circuit breakers, which could mitigate the impact of temporary disruptions in the `accounts` service availability.

   - **Eureka dashboard screeenshot**: ![Eureka Dashboard Showing Service Interaction](../images/Eureka%20Dashboard%20Showing%20Service%20Interaction.PNG)

   - **Web service showing account screeenshot**: ![Web Service Showing Account](../images/Web%20Service%20Showing%20Account.PNG)

   - **Web service log screeenshot**: ![Web Service Log Accounts](../images/Web%20Service%20Log%20Accounts.PNG)

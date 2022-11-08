## OVERVIEW

* Proofpoint MSDefender application collects event data of MS Defender from Azure EventHub and filter Ip, Domain and File Hash related threats from event data by comparing it with ET data from Proofpoint. And, post the alert back to the MS Defender.
* Version - 1.0.0
* Prerequisites - 

   - Install Docker (Recommended: 20.10.17)  
   - Install Docker Compose. (Recommended: v2.10.2)  
   - The Tenant Id of Azure eventhub and MS Defender should be same.  
   - Configure MS Defender Streaming API to one Azure eventhub.  

## COMPATIBILITY MATRIX
* OS Compatibality : Windows, Linux and Mac OS.

## TOPOLOGY AND SETTING UP DOCKER ENVIRONMENT
* Install Docker Engine with suitable Operating System. For Windows and Mac OS installation use static binary installation or Docker Desktop. In Windows run Docker Desktop as Administrator.
    * Link : https://docs.docker.com/engine/install/
* Install Docker Compose with suitable Operating System.
    * Link : https://docs.docker.com/compose/install/

## CONFIGURATION IN MS DEFENDER AND AZURE EVENTHUB

   1. Configure MS Defender Streaming API to one Azure eventhub.
        * Prior to setup streaming API:
            * Create an event hub in your tenant.
            * Log in to your Azure tenant, go to Subscriptions > Your subscription > Resource Providers > Register to Microsoft.insights.
        * Enable raw data streaming
            * Log in to the Microsoft 365 Defender as a Global Administrator. 
            * Go to the Data export settings page in the Microsoft Defender portal.
            * Click on Add data export settings.
            * Choose a name for your new settings.
            * Choose Forward events to Azure Event Hubs.
            * Type your Event Hubs name and your Event Hubs resource ID.
            * In order to get your Event Hubs resource ID, go to your Azure Event Hubs namespace page on * Azure > properties tab > copy the text under Resource ID.
            * Choose the events you want to stream and click Save.

## INSTALLATION

1. In terminal, go the path where docker-compose.yml file is present.
2. Then run the following command in terminal, to start the docker and run the python app : ```docker compose run --name proofpoint proofpoint```.
3. To reconfigure any input credentials run command : ```docker exec -it proofpoint setsid -w python reconfigure.py```.
4. To restart the integration run this command : ```docker exec -it proofpoint setsid python restart.py```.
5. To stop the docker container run this command : ```docker stop proofpoint```.

## Third party python library used

1. Cryptography (36.0.1).
2. Schedule (1.1.0).
3. Azure-eventhub (5.10.1).
4. Psutil (5.9.2).

## TROUBLESHOOTING

### General Checks
 * The Tenant id of MS Defender and Azure must be same.
 * It will take some delay from the MS Defender side for alert to show on MS Defender portal.
 * There must be this files and folder in the zip file.
 ```
.
├── ProofpointMSDefenderIntegration
├── Proofpoint_MSDefender_configuration_logs 
├── Proofpoint_MSDefender_configuration_logs 
├── checkpoint_file.json
└── docker-compose.yml
```
 * There must be this files and folder inside ProofpointMSDefenderIntegration
  ```
.
├── ProofpointMSDefenderIntegration
│   ├── account_credentials.ini
│   ├── config_utils.ini
│   ├── consts.py
│   ├── defender_alert.py 
│   ├── domain_match.py
│   ├── file_match.py
│   ├── ip_match.py
│   ├── logger.py
│   ├── proofpoint_file_api.py
│   ├── proofpointDefenderasync.py
│   ├── reconfigure.py 
│   ├── restart.py 
│   ├── setup.py
│   ├── setup.sh
│   ├── main.sh
│   ├── main.py   
│   ├── requirement.txt
│   └── validation.py                    

``` 
 * Inside docker none of the file and folder should be deleted. If deleted than restart the docker image through docker compose. Follow the clean up and installation for this.
 

 ## UNINSTALL & CLEANUP STEPS
1. Stop the docker container run: '''docker stop proofpoint'''.
2. Delete the stopped docker container run: '''docker rm proofpoint'''.
3. Get the image id run: '''docker images'''
4. Delete the required docker images, run: '''docker rmi <image_id>'''

Terminal: Ubuntu
Service_name: test_service.

Step 1: Create a chat name test_service:
        --> sudo helm3 create test_service.
        This will create a folder named test_service, which will contain yml files for this helm chart. Change the permission of this folder so that a non-root user
        can access it. Run the respective command.
        --> sudo chown -R $USER test_service/
        
Step 2: Enter inside the created service and it will show following files and folders:
        ![image](https://user-images.githubusercontent.com/42828760/200845513-39ae6549-8cd0-484f-888e-a8fe4a14df44.png)
        Enter inside the templates folder by running:
        cd test_service/templates
        For having the microservice up and running, we do not need all of the yaml files that are created by Helm in templates
        folder. We can proceed to delete the ingress, serviceaccount, hpa yaml files, as well as the tests/ folder. You can do 
        so by running the following commands:
        rm -r ingress.yaml hpa.yaml serviceaccount.yaml tests/
        
Step 3: Removing above resources used in chart by editing deployment.yaml.
        Comment the line which is using service account as:
        * ![image](https://user-images.githubusercontent.com/42828760/200846755-41bb9968-96f1-4d02-9ad2-97483b1f0e64.png)
        We also remove the if else statement which uses the autoscaling functionality, and force the number of replicas equal to .Values.replicaCount 
        (replica count provided in Values yaml file)
        ![image](https://user-images.githubusercontent.com/42828760/200847518-0e5dc560-1633-46a9-9a43-7357d17374fd.png)
        
Step 4: Disable the liveness and readiness checks: As we do not have clarity yet as to what kind of liveness and readiness checks we will be using with these 
        services, we can comment the functionality for now. We can do so by editing the deployment.yaml file.
        ![image](https://user-images.githubusercontent.com/42828760/200847795-6d5b18a5-f609-44ee-a220-529c8acead1a.png)

Step 5: Move 1 step up and edit the values.yaml file:
        ![image](https://user-images.githubusercontent.com/42828760/200848116-686bb19d-7b03-490e-a1ca-e2ec3c5baad8.png)
        Change securityContext->runAsUser to 1000.
        ![image](https://user-images.githubusercontent.com/42828760/200848926-4a262f19-6852-450c-b064-fbf19049a1cb.png)
        Then, change the service--> port to the port you want the serive to be available at. We can set it to 8021.
        ![image](https://user-images.githubusercontent.com/42828760/200849421-d86f1316-4c50-40e2-9574-63b37edf89d3.png)
        Next, change the containers--> port to 8021.
        ![image](https://user-images.githubusercontent.com/42828760/200849736-bff02f3f-1593-4de3-8e94-53496bbb9a07.png)
        Save changes and quit.

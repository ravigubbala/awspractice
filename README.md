Step by step guide for deploying spring boot application on AWS ECS. Date: 29.07.2020
-------------------------------------------------------------------------------------

Note: The guide may become obsolete with the future releases of AWS.

1. Install AWSTools for Windows.

2. Login to AWS Console (create AWS account if not already have one.)

3. Go to IAM > Users and click on Add User.

4. Provide username of your choice and select "Programmatic access" and click "Next: Permissions".

5. Select "Attach existing policies directly" and search and select "AmazonEC2ContainerRegistryFullAccess" policy.

6. Leave default values in all other pages and create user and download "new_user_credentials.csv".

7. Open "Windows Powershell" and navigate to the spring boot project directory.

8. Execute following commands in order.

    a. Set-AWSCredential ` -AccessKey <AccessKey_from_csv> ` -SecretKey <SecretKey_from_csv> ` -StoreAs <name_your_profile>
  
    b. Set-AWSCredential -ProfileName <profile_name_given_in_previous_command>
  
9. Go to "Elastic Container Registry" from the AWS Console.

10. Click on "Create Repository" and give repository name of your choice.

11. Leave default values for all other settings and click "Create Repository".

12. Once repository is created, Click on the "View Push commands" bar displayed at the top.

14. Go to "Windows" tab on the popup and execute the given commands in "Windows Powershell" in the given order.

15. Once the commands are executed successfully, Go to "Elastic Container Registry" from the AWS Console and click on repository name. 

16. It will take you to image details page. Take a note of the value in "Image URI" column which will be used later.

17. Go to IAM > Roles from AWS Console.

18. Click on "Create Role" and in the next page select "Elastic Container Service" and then select "Elastic Container Service Task" and click "Next: Permissions".

19. In the next page, search and select "AmazonEC2ContainerServiceRole" policy.

20. Leave default values in all other pages and enter role name of your choice in the last page and click "Create Role".

21. Go to "ECS" from AWS Console and then to the "Task Definitions".

22. Click on "Create New Task Definition" and in the next page select "Fargate" and click "Next Step".

23. In the next page enter a task definition name of your choice.

24. select the role created in step 20 for "Task Role".

25. Select "Task Memory" and "Task CPU" as per the requirement. Eg. 2 GB, 1 vCPU.

26. Click on "Add Container" and enter container name as per your choice.

27. Copy and paste "Image URI" value from step 16.

28. Enter port number configured in spring boot application for "Port mappings". For example, port number 8080.

29. Leave default values for all other inputs and click "Add".

30. Once container is added, click on "Create" to create task definition.

31. Go to "ECS" > "Amazon ECS" > "Clusters" from the AWS Console.

32. Click on "Create Cluster" and select "AWS Fargate" as cluster template.

33. Enter cluster name of your choice and select "Create VPC" and click on "Create".

34. Click on "View Cluster" once the cluster is created.

35. Go to "Services" tab and click on "create".

36. In the next page, select "Fargate" as launch type.

37. Make sure correct values are selected for "Task Definition" and "Cluster".

38. Enter service name of your choice and input no of tasks depending on the replication requirements and click on "Next Step".

39. Make sure to select values for VPC and subnets and select "ENABLED" for "Auto-assign public IP".

40. Leave default values in all other pages and click "Create Service" in last page.

41. Once service is created, click on service name to open service definition.

42. In the "Details" tab, click on the value of the "Security Groups".

43. Once your are in the security group details page, Select "Inbound Rules".

44. Click on "Edit Inbound Rules" and add new rule with values Type=Custom TCP, Protocol=TCP, Port range=8080, Source=0.0.0.0/0. Note 8080 is same as the port selected in step 28.

45. Click on "Save Rules" to save the the inbound rules.

46. Go to "ECS" > "Clusters" and click on the name of the cluster created in step 33.

47. Go to "Tasks" tab and click on Task id.

48. In the Task details page, copy the value of "Public IP".

49. Open web browser and paste the public IP copied in the previous step followed by colon and port number from step 28. Eg. 8080. This should launch your spring boot application.

References:

1. https://epsagon.com/development/deploying-java-spring-boot-on-aws-fargate
    
2. https://stackoverflow.com/questions/38587325/aws-ecr-getauthorizationtoken
    
3. https://github.com/in28minutes/deploy-spring-microservices-to-aws-ecs-fargate

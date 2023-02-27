[[Index]] 



# Jenkins-agent.yml

  

```

  

1. Generate ssh key using custom role and that pulls it from vault

  

2. Create Jenkins directory on target server

  

3. Create Apache-Maven directory on target server

  

4. Download jenkins agent jar file from jenkins server into the jenkins directory of the target server

  

5. Download apache maven package from nexus in the maven directory of the target server

  

6. Unarchive the maven package and move it's contents to the jenkins dir of the target server

  

7. Download secrets-file from ansible playbook deployment tool into the jenkins dir of the target server

  

8. Create a local secrets file on target server in jenkins directory (secrets holds credentials to run jar command to install agent)

  

9. Extract secrets from secrets file using a xml lint command (file was filled with lots of unneeded content) and pass to the local one

  

10. Create agent.sh script to hold jar command that uses the jenkins-agent and secrets file to install the agent and connect it to jenkins

  

11. Execute the agent.sh

  

```
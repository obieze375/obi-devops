

# Initial Configuration of Automation Controller and Private Automation Hub 


```bash

```

## From a terminal window, change into the /home/student/certified-EEs directory. 

```bash
[student@workstation ~]$ cd ~/certified-EEs/ 
```


## Use the skopeo login command to log in to private automation hub, using admin as the username and redhat as the password. 

```bash
[student@workstation certified-EEs]$ skopeo login hub.lab.example.com
Username: admin
Password: redhat
Login Succeeded!
```

## Use the skopeo copy command to upload the compatibility, minimal,and supported automation execution environment image archives. Red Hat uses compatibility to refer to the ee-29-rhel8 automation execution environment because that execution environment contains Ansible 2.9 (a version that is compatible with many earlier playbooks). Upload the image archives to the hub.lab.example.com server using the ansible-automation-platform-22 namespace and the latest tag. 

Set the HUB and AAP shell variables to reduce typing in the following skopeo
commands. 

```bash
[student@workstation certified-EEs]$ HUB="hub.lab.example.com"
[student@workstation certified-EEs]$ AAP="ansible-automation-platform-22"
[student@workstation certified-EEs]$ skopeo copy \
> docker-archive:ee-29-rhel8.tgz docker://${HUB}/${AAP}/ee-29-rhel8:latest
...output omitted...
[student@workstation certified-EEs]$ skopeo copy \
> docker-archive:ee-minimal-rhel8.tgz \
> docker://${HUB}/${AAP}/ee-minimal-rhel8:latest
...output omitted...
[student@workstation certified-EEs]$ skopeo copy \
> docker-archive:ee-supported-rhel8.tgz \
> docker://${HUB}/${AAP}/ee-supported-rhel8:latest
...output omitted...
```


This example uses the skopeo command, but you can use the podman command if
you prefer. Assuming that you logged in to private automation hub with the podman
login command, you can upload an archive with the following commands. When
not specified, the podman tag and podman push commands default to using the
latest tag. 

```bash
[student@workstation certified-EEs]$ podman load -i ee-supported-rhel8.tgz
...output omitted...
[student@workstation certified-EEs]$ podman tag \
> registry.redhat.io/ansible-automation-platform-22/ee-supported-rhel8 \
> hub.lab.example.com/ansible-automation-platform-22/ee-supported-rhel8
...output omitted...
[student@workstation certified-EEs]$ podman push \
> hub.lab.example.com/ansible-automation-platform-22/ee-supported-rhel8
...output omitted...
```


## Verify that the automation execution environments are available from private automation hub. 

## Navigate to https://hub.lab.example.com and log in as the admin user with redhat as the password. 

## Navigate to Execution Environments > Execution Environments to display the available automation execution environments. 

The installer created the ee-29-rhel8, ee-minimal-rhel8, and ee-supported-rhel8
container repositories using the container image archives included in the Ansible


Automation Platform setup bundle. You created the three container repositories that
use the ansible-automation-platform-22 namespace. 


## Click the link for the ansible-automation-platform-22/ee-supported-rhel8 container repository and then click the Images tab. Private automation hub provides the container image using the latest tag. 

## Create the containers namespace and then upload the containers.podman content collection. 

Navigate to Collections > Namespaces and then click Create. 

Enter containers in the Name field and then click Create to create the namespace. 

Click Upload collection. 

Click Select file, select /home/student/content-collections/community/
containers-podman-1.9.1.tar.gz, and then click Upload. 


## Create the ansible namespace and then upload the ansible.posix content collection. 

Navigate to Collections > Namespaces and then click Create. 

Enter ansible in the Name field and then click Create to create the namespace. 

Click Upload collection. 

Click Select file, select the /home/student/content-collections/
certified/ansible-posix-1.3.0.tar.gz archive, and then click Upload. 

## Approve the uploaded content collections. 

## Navigate to Collections > Approval.


## Click Approve to approve the ansible.posix content collection. 

## Click Approve to approve the containers.podman content collection. 

## Navigate to Collections > Collections and verify that private automation hub displays the podman and posix automation content collections. 

## Synchronize the automaton controller Demo Project project resource. 

## Navigate to https://controller.lab.example.com and log in as the admin user with redhat as the password. 

## Navigate to Resources > Projects to display existing project resources. 

## Click the Sync Project icon to synchronize the Demo Project resource. 

## Update the Demo Inventory resource to remove the localhost resource. Add the workstation.lab.example.com host to the Demo Inventory resource. 

Making this change allows the Demo Inventory resource to target a machine in your
environment (workstation.lab.example.com) rather than the automation execution
environment (localhost). 

## Navigate to Resources > Inventories to display existing inventory resources. 

## Click the link for the Demo Inventory resource and then click the Hosts tab. 

## Select the localhost resource and then click Delete. Confirm your decision to delete the localhost resource. 

## Click Add. Enter workstation.lab.example.com in the Name field and then click Save. 


## Update the Demo Credential machine credential resource so that it uses valid credentials for accessing the workstation.lab.example.com host. 

## Navigate to Resources > Credentials to display existing credentials. 

## Locate the Demo Credential resource and click the Edit Credential icon for that row. 

## Enter student in the Username field, enter student in the Password field, and then click Save. 

If you previously completed this exercise, then click the Replace icon to enter a new
password. 

## Launch the Demo Job Template job. 

## Navigate to Resources > Templates to display existing templates.
## Click the Launch Template icon to initiate the job. The Demo Job Template resource uses the Demo Credential machine credential resource to connect to the workstation.lab.example.com host in the Demo Inventory resource.

[[ansible_rhel/1.1-setup/README]] 

[[ansible_rhel/1.2-thebasics/README]]  

[[ansible_rhel/1.3-playbook/README]]  

[[ansible_rhel/1.4-variables/README]]  

[[ansible_rhel/1.5-handlers/README]]  

[[ansible_rhel/1.6-templates/README]]   

[[ansible_rhel/1.7-role/README]]  

[[ansible_rhel/2.1-intro/README]]  

[[ansible_rhel/2.2-cred/README]]  

[[ansible_rhel/2.3-projects/README]]   

[[ansible_rhel/2.4-surveys/README]]  

[[ansible_rhel/2.5-rbac/README]]   

[[ansible_rhel/2.6-workflows/README]]  

[[ansible_rhel/2.7-wrap/README]] 

[[Index]] 


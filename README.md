# SIEM lab using wazuh
  Deploying the open-source SIEM/EDR solution wazuh and stress the testing capabilities of the platform
 ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/0f670e05446898d494718d196bfb62ee9510e11b/screenshot/display_____SIEM____Dashboard.png)
 
## Table of Contents

-[Environment setup](#enviroment-setup)
  - [Deploying Wazuh Server VM](#deploying-wazuh-server-vm)
- [Deploying Wazuh Agents](#deploying-wazuh-agents)
  - [Deploying Wazuh Agent to Windows VM](#deploying-wazuh-agent-to-windows-vm)
- [Testing our Agents](#stress-testing-our-agents)
  - [Manual Testing](#manual-testing)
  - [Automated Testing](#automated-testing)



## Enviroment setup
### Deploying Wazuh Server VM
  To get started, deploy the Wazuh Server in Kali linux on a VirtualBox VM. 
   TO do that, implement this command in terminal
     =>>curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a


  THen you will get the username and password


  Then go to browser search the ip address of wazuh-server then you will git the wazuh login interface
  ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/c5ef8bffe76e158b5a6c5b680bca99d449a898c6/screenshot/Browse_ip.png)
  INTERFACE
  ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/c5ef8bffe76e158b5a6c5b680bca99d449a898c6/screenshot/login_interface.png)

we are given defalult credentials, we can changing the password or admin. After loggin in  we see the wzuh server main dashboard
   ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/c5ef8bffe76e158b5a6c5b680bca99d449a898c6/screenshot/dashboard.png)


## Deploying wazuh Agents
### Deploying wazuh Agent to windows VM
  1. Download the zip file for Wazuh-agent , you can do it through terminal.
     =>>> Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.5-1.msi -OutFile wazuh-                  agent.msi
     ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/c5ef8bffe76e158b5a6c5b680bca99d449a898c6/screenshot/wazuh_agent.png)

     
  3. Go to download section and install the wazuh-agent, after the completion, the configuration              interface pop up, add the wazuh-server ip address(kali linux)
     ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/c5ef8bffe76e158b5a6c5b680bca99d449a898c6/screenshot/window_agent_interface_add_ip_.png)
     
 4. After these steps you will be given a command to copy/paste onto wazuh-agent VM( powershell in administration) through wazuh-server dashboard, then choose the agent system type:
     ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/c5ef8bffe76e158b5a6c5b680bca99d449a898c6/screenshot/choose%20-agents-type.png)

5. Input the ip of server and name of the wazuh-agent:
    ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/c5ef8bffe76e158b5a6c5b680bca99d449a898c6/screenshot/assign_ip.png)

6. copy the command and paste to wazuh-agent command(powershell in adminstration):
    ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/c5ef8bffe76e158b5a6c5b680bca99d449a898c6/screenshot/copy_command_agent.png)

    ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/c5ef8bffe76e158b5a6c5b680bca99d449a898c6/screenshot/paste_command_window.png)

7.  after that check if the wazuh-agent is working or not
     =>>> net start wazuh
 (NOTE : IF THIS DIDN'T WORK, THEN TRY TO CONNECT THE WAZUH-AGENT THROUGH WAZUH-SERVER COMMAND
     =>> sudo /var/ossec/bin/manage_agents
        ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/c5ef8bffe76e158b5a6c5b680bca99d449a898c6/screenshot/diffenent_method_2.png)
  
 9. Then, refresh the wazuh-dashboard, then you will the the wazuh-agent connected:
     ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/c5ef8bffe76e158b5a6c5b680bca99d449a898c6/screenshot/active_1.png)

10. If we go the wazuh-agent and then you get the full insterface with details informations:

     ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/c5ef8bffe76e158b5a6c5b680bca99d449a898c6/screenshot/active_2.png)
    
   
## Testing our Agents
At this pont we've deployed a wazuh-agent that is sending its logs/checking in to the Wazuh-Server.
 
### Manual Testing
  To start let's create the administrative account in window VM( wazuh-agent)
     =>>> net user username password /add
     =>>> net localgroup Administrators username /add

  ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/f1c23641111a4ca9394a9432d97e480bb264a7de/screenshot/command_to_create_group_administrtion_.png)

Check the wazuh-dashboard to check the alert,  you can see we have "adminstration group-change" alert::
  go to security  events

![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/f1c23641111a4ca9394a9432d97e480bb264a7de/screenshot/administration_group_change_dashboard_.png)

If you want more detail about this alert go to  MITRE ATT&CK::

![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/f1c23641111a4ca9394a9432d97e480bb264a7de/screenshot/details_of_administration_.png)


### Automated Testing
 Now we've done some manual testing let's try using an Endpoint Detection and Response(EDR) testing  script that
 will perform a few MITRE ATT&CK techniques but the payload is the windows calculator, you download the script through github [here](https://github.com/op7ic/EDR-Testing-Script/tree/master)
 Or you can directly through command and implement
  =>>>Invoke-WebRequest -Uri "https://raw.githubusercontent.com/op7ic/EDR-Testing-Script/master/runtests.bat" -          OutFile "$env:USERPROFILE\Desktop\runtests.bat"
  Then navigate to desktop(as it is downloded in desktop)
     =>>>cd $env:USERPROFILE\Desktop
finally, run the test script:
   =>>> .\runtests.bat
 ![Alt Text]()
   After this , different operation continue to perform as if it was by attacker
   This is what you will see after running the attack:
    ![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/7c8fa1bb5e34b510e2372cf5790ac7be96d5db2e/screenshot/automation_popup_simulation.png)

Then, go to the wazuh-dashboard , in MITRE AAT&CK section:
In Top tactices pie-chart:

![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/7c8fa1bb5e34b510e2372cf5790ac7be96d5db2e/screenshot/threat_categorzing%20by_MIRTIE_.png)

Under the "top tactics" pie chart ,click persistence and then go to Events section,:

![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/7c8fa1bb5e34b510e2372cf5790ac7be96d5db2e/screenshot/new_widnow_service_created_auto.png)

While exploring the results an event log showing a new service being created sparked interest. Let's go furthr:
![Alt Text](https://github.com/santosholi01/SIEM_lab_wazuh/blob/7c8fa1bb5e34b510e2372cf5790ac7be96d5db2e/screenshot/evilservice_auto__a.png)

    
    

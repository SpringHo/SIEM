# SIEM
## SOC Home Lab (Security Information and Event Manager)
I’ll walk you through steps on how I set up a home lab for Elastic Stack Security Information and Event Management (SIEM) using the Elastic Web portal and a Kali Linux VM. I will generate security events on the Kali VM, set up an agent to forward data to the SIEM, and query and analyze the logs in the SIEM.

**Overview of the tasks**
- Set up a free Elastic account.
- Install the Kali VM.
- Configure the Elastic Agent on the Linux VM to collect the logs and forward it to the SIEM.
- Generate security events on the Kali VM.
- Query to find the security events in the Elastic SIEM.
- Create a Dashboard to visualize security events.
- Create alerts for security events.

# Setting up
- Set up Elastic account
<img src="https://github.com/user-attachments/assets/466a43b7-4f8a-4783-b7e9-dfe972f4a70f" width="400" height="300">

- Set up Kali VM
<img src="https://github.com/user-attachments/assets/1fd93152-185f-4c2d-89ba-d7291a9be6df" width="480" height="350">

#

**Troubleshooting**
: Issues pop up where my system wouldn't let linux run a virtual machine. Had to play around and adjust settings in computer BIOs to scope out the issue.
- Aborted VM
<img src="https://github.com/user-attachments/assets/70f5a06b-2a35-4332-9ac0-464f7839c94f" width="480" height="350">

- Error that showed up for VM indicated that AMD-V in BIOS is disabled causing failure code
<img src="https://github.com/user-attachments/assets/4db6a6bc-be22-47e9-adb5-0ad0da84ac60" width="150" height="250">

1. **Not in hypervisor partition (HVP=0)**: This means that the virtual machine cannot start because hardware virtualization is not enabled.
2. **AMD-V is disabled in the BIOS (VERR_SVM_DISABLED)**: This suggests that **AMD-V** (which is the AMD version of hardware-assisted virtualization) is disabled in BIOS settings.
- Once inside the BIOS, navigate to the **Advanced Mode**. 
- In the **Advanced** tab, look for **CPU Configuration**.
- Within **CPU Configuration**, find **SVM Mode** (Secure Virtual Machine Mode).
- **Enable SVM Mode**. This will allow AMD-V to be utilized by VirtualBox.
<img src="https://github.com/user-attachments/assets/a83e84a8-545b-48a6-ae3f-c30ef9806226" width="480" height="350">
<img src="https://github.com/user-attachments/assets/c6e7398a-476e-4fe8-97e3-d4edab6b2530" width="480" height="350">


- It works!
<img src="https://github.com/user-attachments/assets/7d8b16ba-42e2-480a-87b3-08a980b26f6a" width="480" height="350">


#
- Successfully installed Kali Linux
<img src="https://github.com/user-attachments/assets/a79d5758-960d-4b7c-8592-dac08cc35ddd" width="480" height="350">
<img src="https://github.com/user-attachments/assets/dfe7b596-597b-400f-8b39-ed3f0d90e91d" width="480" height="350">

# Add intergrations - Elastic Defend
<img src="https://github.com/user-attachments/assets/d0c172e4-adef-443c-b8d6-f2235a579d22" width="430" height="480">
<img src="https://github.com/user-attachments/assets/42caa53d-b42f-4b94-b980-8490393f97ad" width="650" height="380">

#

**Agent enrollment for windows in PowerShell**
```pre
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest -Uri https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.15.1-windows-x86_64.zip -OutFile elastic-agent-8.15.1-windows-x86_64.zip
Expand-Archive .\elastic-agent-8.15.1-windows-x86_64.zip -DestinationPath .
cd elastic-agent-8.15.1-windows-x86_64
.\elastic-agent.exe install --url=https://32150d0f66754d5283d8b62e407ab046.fleet.us-west-2.aws.found.io:443 --enrollment-token=NDl4TkRaSUJqMG9uVXZVZFlrWmk6aUpNOE4ybldUejZOODN5azM2d2c1UQ==
```
<img src="https://github.com/user-attachments/assets/5557e0d9-51d6-4e6e-8a6a-5f35e71dbd61" width="4000" height="380">
<br>
<img src="https://github.com/user-attachments/assets/0626c749-0ecb-4991-b14b-65ef69ff1d6e" width="550" height="210">
<br>
<img src="https://github.com/user-attachments/assets/b843f832-0e8d-482e-a77d-a3ff5da75620" width="480" height="350">
<br>
<img src="https://github.com/user-attachments/assets/10657da2-f993-42e3-985a-46ee50ed9490" width="800" height="280">
<br>
<br>
<br>

Installed Elastic Agent into Linux-
```pre
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.15.1-linux-x86_64.tar.gz
tar xzvf elastic-agent-8.15.1-linux-x86_64.tar.gz
cd elastic-agent-8.15.1-linux-x86_64
sudo ./elastic-agent install --url=https://32150d0f66754d5283d8b62e407ab046.fleet.us-west-2.aws.found.io:443 --enrollment-token=NE41aERaSUJkZDhIaGtzeEZOOHk6MmhTbndQQS1TcEN1WWNmYlY0a00zQQ==
```

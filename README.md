Demo Project: Install Jenkins on DigitalOcean
Technologies Used:

	- Jenkins
	- Docker
	- DigitalOcean
	- Linux
Project Description:
	- Create an Ubuntu server on DigitalOcean. Set up and run Jenkins as Docker. Initialize Jenkins.

1. Create a DigitalOcean Droplet
	- Log in to your DigitalOcean control panel.
	- Click "Create" -> "Droplets".
	- Choose an Ubuntu image (e.g., Ubuntu 25.04 LTS).
	- Select a plan (Basic Shared CPU is sufficient for a demo).
	- Choose a datacenter region : Singapore
	- Select SSH Keys and add your public key.     	
		- "cat ~/.ssh/id_rsa.pub"
    		- Paste the public key
	- Name the droplet as "jenkins-server"
	- Create a new firewall and configure inbound rules
		- Networking > Firewalls > Create Firewall
		- name : jenkins-firewall 
    		- port 22  , ssh port from my computer only
    		- port 8080, default HTTP port for jenkins serving web requests
	- Apply the newly created firewall to created droplet, jenkins-server.
	- Finalize and create the Droplet. Note its public IP address.

2. Initial Server Setup & Security Hardening
	- Connect as the root user (initially the only user).
    		- ssh root@206.189.92.237
	- Update System packages
    		- apt update
	- Install docker
		-apt install docker.io
	- Run Jenkins
```bash
docker run -p 8080:8080 -p 50000:50000 -d \
-v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```
	- Visit Jenkins UI
		- http:206.189.92.237:8080
	- Get password located in host
		- docker volume ls
			- jenkins_home
		- docker volume inspect jenkins_home
			- "Mountpoint": "/var/lib/docker/volumes/jenkins_home/_data"
			- cat /var/lib/docker/volumes/jenkins_home/_data/secrets/initialAdminPassword
	- Get password located in container
		- docker exec -it 5f45b3a240cf bash
		- cat /var/jenkins_home/secrets/initialAdminPassword
	- Input the password copied from above and install suggested pulgins
	- Finally prompte to create first admin user
		- prompt to input username, password, full name and email


3. Others
	- Jenkins store data
		- location : /var/lb/docker/volumes/jenkins_home/_data
			- configuration data
			- jenkins jobs
			- user & permssions
			- plugins
	- Inspect docker volume
		- docker volumne ls
		- docker volume inspect jenkins_home
			- mount_point

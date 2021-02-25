# Docker-installation-ppc64le-working
Installing docker for OS : linux, Dist : focal, arch: ppc64le 

 1. check box details: 
		1.1 kernal details : uname -r
							 5.4.0-60-generic
							 
		1.2 Os details:      cat /etc/os-release
							5.4.0-60-generic
							ubuntu@rajesh-kumar:~$ cat /etc/os-release
							NAME="Ubuntu"
							VERSION="20.04.1 LTS (Focal Fossa)"
							ID=ubuntu
							ID_LIKE=debian
							PRETTY_NAME="Ubuntu 20.04.1 LTS"
							VERSION_ID="20.04"
							HOME_URL="https://www.ubuntu.com/"
							SUPPORT_URL="https://help.ubuntu.com/"
							BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
							PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
							VERSION_CODENAME=focal
							UBUNTU_CODENAME=focal

 
 2. Error :
	1.sudo apt install docker.io
		Reading package lists... Done
		Building dependency tree
		Reading state information... Done
		Package docker.io is not available, but is referred to by another package.
		This may mean that the package is missing, has been obsoleted, or
		is only available from another source

		E: Package 'docker.io' has no installation candidate
		
	2.  sudo add-apt-repository \
	   "deb [arch=ppc64le] https://download.docker.com/linux/ubuntu \
	   bionic \
	   stable"

Skipping acquire of configured file 'stable/binary-ppc64le/Packages' as repository 'https://download.docker.com/linux/ubuntu bionic InRelease' doesn't support architecture 'ppc64le'
	   
	3. sudo add-apt-repository "deb [arch=ppc64el] https://download.docker.com/linux/ubuntu xenial stable edge" // <working >
	
 3. Solution :
	3.1 Remove all docker component old version ot not propper working version 

		sudo apt-get purge -y docker-engine docker docker.io docker-ce docker-ce-cli
		
		sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce 
		
		sudo rm -rf /var/lib/docker /etc/docker
		
		sudo rm /etc/apparmor.d/docker
		
		sudo groupdel docker
		
		sudo rm -rf /var/run/docker.sock
		
		
	3.2 Add correct version of repo and prereqisite
		
		sudo apt-get update
		
		sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
		
		curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
		
		sudo add-apt-repository "deb [arch=ppc64le] https://download.docker.com/linux/ubuntu/dists/xenial/stable/binary-ppc64le/"
		
		sudo apt-get install docker-ce
		
		sudo systemctl status docker
		O/P
		ubuntu@rajesh-kumar:~$ sudo systemctl status docker
			● docker.service - Docker Application Container Engine
				 Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
				 Active: active (running) since Thu 2021-02-25 08:12:47 UTC; 30min ago
			TriggeredBy: ● docker.socket
				   Docs: https://docs.docker.com
			   Main PID: 5535 (dockerd)
				  Tasks: 23
				 Memory: 88.0M
				 CGroup: /system.slice/docker.service
						 ├─5535 /usr/bin/dockerd -H fd://
						 └─5542 docker-containerd --config /var/run/docker/containerd/containerd.toml

			Feb 25 08:12:42 rajesh-kumar dockerd[5535]: time="2021-02-25T08:12:42.351028781Z" level=info msg="Loading containers: done."

		3.1 configuring Docker
			
			sudo usermod -aG docker $USER  
			
			incase of docker.sock: connect: permission denied error 
			sudo chmod 666 /var/run/docker.sock
			
			Testing : 
			docker run hello-world
			
	some of the hepfull url:		
		https://github.com/docker/docker.github.io/issues/7660

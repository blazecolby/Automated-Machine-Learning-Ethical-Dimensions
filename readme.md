### Google Cloud Compute CPU Setup
- Login to [Google Compute Engine](https://console.cloud.google.com)
- Navigate to [GCP Instances](https://console.cloud.google.com/compute/instances)
- Click Create Instance
- Name the instance
- Select the nearest zone
- Change 'Boot disk' to 'Ubuntu 16.04 LTS'. Increase OS image ot be 64 GB.
- Check `Allow HTTP traffic` and `Allow HTTPS traffic`
- Navigate to [Firewall rule](https://console.cloud.google.com/networking/firewalls/)
- Create Firewall Rule.
- Name the rule
- Change 'Targets' to 'All instances in the network'
- Specify the 'Source IP ranges' to be '0.0.0.0/0'
- Under 'Protocols and Ports', select 'Specified protocols and ports', enter tcp with '12345'
- Click create
- Repeat the steps to create a rule for jupyter lab, enter tcp with '5000'
- Navigate to the [instance page](https://console.cloud.google.com/compute/instances)
- SSH to the VM instance by selecting 'Open in Browser Window', ctrl or cmd select for same window.
- ```nano install.sh```
- ```
  apt-get -y update
  apt-get -y --no-install-recommends install \
  curl \
  apt-utils \
  python-software-properties \
  software-properties-common
    add-apt-repository -y "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

  apt-get update
  apt-get install -y docker-ce
  ```
- ```chmod +x install.sh```
- ```sudo ./install.sh```
- ```
  mkdir ~/tmp
  mkdir ~/log
  mkdir ~/data
  mkdir ~/scripts
  mkdir ~/license
  mkdir -p ~/jupyter/notebooks
  ```
- ```sudo usermod -aG docker <username>```
- ```sudo reboot```, close/open SSH terminal
- ```wget [driverless ai](https://s3.amazonaws.com/artifacts.h2o.ai/releases/ai/h2o/dai/rel-1.8.5-64/x86_64-centos7/dai-docker-centos7-x86_64-1.8.5.1-10.0.tar.gz)```
- ``` sudo docker load < dai-docker-centos7-x86_64-1.8.5.1-10.0.tar.gz```
- ```nano scripts/start.sh```
- ```
  docker run \
    --pid=host \
    --init \
    --rm \
    --shm-size=256m \
    -u `id -u`:`id -g` \
    -p 12345:12345 \
    -v `pwd`/data:/data \
    -v `pwd`/log:/log \
    -v `pwd`/license:/license \
    -v `pwd`/tmp:/tmp \
    h2oai/dai-centos7-x86_64:1.8.5.1-cuda10.0.64
  ```
- ```chmod +x scripts.sh```
- ```http://Your-Driverless-AI-Host-Machine:12345```



### Run Notebook
Once Driverless AI is up and running fill in the details below. For GCP the credential are the username and password for the GCP account. With the username being the first of the email before the @.

address = 'http://localhost:12345' <br>
username = 'name' <br>
password = 'pass'

This will download all the needed files, parse the data, randomize the data, train/test split the data, and return the predictions. 

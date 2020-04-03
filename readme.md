### Mac install instructions

- Install: [Docker Driverless AI](https://s3.amazonaws.com/artifacts.h2o.ai/releases/ai/h2o/dai/rel-1.8.5-64/index.html) <br>
- Install [Docker for Mac](https://docs.docker.com/docker-for-mac/install/)
- Adjust Docker memory to at least 10GB
- mkdir driverless_ai_directory_name
- cd driverless_ai_directory_name
- docker load < dai-docker-centos7-x86_64-1.8.5.1-10.0.tar.gz
- mkdir data log license tmp 
- docker images
- nano start.sh
-paste <br>
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
 - Connect to Driverless AI with your browser at http://localhost:12345.

### Run Notebook
Once Driverless AI is up and running fill in the details below. For GCP the credential are the username and password for the GCP account. With the username being the first of the email before the @.

address = 'http://localhost:12345' <br>
username = 'name' <br>
password = 'pass'

This will download all the needed files, parse the data, randomize the data, train/test split the data, and return the predictions. 

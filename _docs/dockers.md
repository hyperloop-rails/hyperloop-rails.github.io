---
title: Docker Images
permalink: /docs/home/
redirect_from: /docs/index.html
---

We have made the replication packages for our [study]('../220-HowNotStructure.pdf') 
available, in the form of [docker images](https://hub.docker.com/u/icse2018acm/).
These packages contain the deployed application (of the version we used in our study),
the synthetic data we generated, and a new version of the application where
we manually fixed the issues we listed in the paper.

These docker images are easy to deploy and run:

1. Download the docker image:
```
docker pull icse2018acm/APP_NAME:TAG:NAME
```

2. Run image in interactive mode:
```
docker run -it -p 127.0.0.1:3000:3000 icse2018acm/APP_NAME:TAG_NAME
```

3. Start the server on the image:
```
cd /home/APP_NAME
./start_server.sh
```
Open a browser and login http://localhost:3000, and you're ready to use the app!

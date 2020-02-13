# README #

This README would normally document whatever steps are necessary to get your application up and running.

### What is this repository for? ###

* Quick summary
* Version
* [Learn Markdown](https://bitbucket.org/tutorials/markdowndemo)

### How do I get set up? ###

* Summary of set up
* Configuration
* Dependencies
* Database configuration
* How to run tests
* Deployment instructions

1.  Validate environment

    docker info | grep "Total Memory"

Make sure you see something like Total Memory: 5.818GiB

    docker stop vespa
    
    docker rm vespa

2.  Start a Vespa Docker container:

Windows:

Make sure curl binary is on the path.  You can do so by adding the Git's bin directory to the path:
Ex:  C:\Program Files\Git\mingw64\bin\

Assuming that votecube-vespa project directory is located under: C:\Users\Papa\vc

    set VESPA_APPS=C:\Users\Papa\vc

    docker run --detach --name vespa --hostname vespa-container --privileged --volume %VESPA_APPS%:/vespa-apps --publish 8086:8080 vespaengine/vespa

Linux:

Assuming that votecube-vespa project directory is located under: ~/vc

    set VESPA_APPS=~/vc

    docker run --detach --name vespa --hostname vespa-container --privileged \
      --volume $VESPA_APPS:/vespa-apps --publish 8086:8080 vespaengine/vespa

3.  Wait for the configuration server to start - signified by a 200 OK response:

    docker exec vespa bash -c "curl -s --head http://localhost:19071/ApplicationStatus"

4.  Deploy and activate a votecube-vespa application:

    docker exec vespa bash -c "/opt/vespa/bin/vespa-deploy prepare /vespa-apps/votecube-vespa/src/main/application/ && /opt/vespa/bin/vespa-deploy activate"
  
5.  Ensure the application is active - wait for a 200 OK response:
  
    curl -s --head http://localhost:8086/ApplicationStatus


### Contribution guidelines ###

* Writing tests
* Code review
* Other guidelines

### Who do I talk to? ###

* Repo owner or admin
* Other community or team contact

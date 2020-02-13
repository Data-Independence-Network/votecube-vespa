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

The response should contain the following line (at first nothing will be returned):

HTTP/1.1 200 OK

4.  Deploy and activate a votecube-vespa application:

    docker exec vespa bash -c "/opt/vespa/bin/vespa-deploy prepare /vespa-apps/votecube-vespa/src/main/application/ && /opt/vespa/bin/vespa-deploy activate"
  
5.  Ensure the application is active - wait for a 200 OK response:
  
    curl -s --head http://localhost:8086/ApplicationStatus


Once installed copy out the client jar:

docker cp vespa:/opt/vespa/lib/jars/vespa-http-client-jar-with-dependencies.jar ./vespa-http-client-jar-with-dependencies.jar

Client jar is used to feed data into vespa by the aux process like so:

java -jar ./vespa-http-client-jar-with-dependencies.jar --file ./testVespaInput.json --host localhost --port 8086
Thu Feb 13 13:26:30 MST 2020 Result received: 0 (0 failed so far, 2 sent, success rate 0.00 docs/sec).
Thu Feb 13 13:26:30 MST 2020 Result received: 2 (0 failed so far, 2 sent, success rate 19.61 docs/sec).

### Contribution guidelines ###

* Writing tests
* Code review
* Other guidelines

### Who do I talk to? ###

* Repo owner or admin
* Other community or team contact

## Further resources      

## Learning outcomes

**After having completed this chapter you will be able to:**

- Have an understanding of the challenges of building an efficient Docker container on your computer.
- Be aware of resources on techniques and best practices to address these chjallenges

   
## 3.1 Where to go next in developing your Docker skills
Our course could only cover the basics of creating and using containers in Docker. This section is intended to suggest options that you may find useful and where to get more information on them.    

## 3.2 Shrinking containers
As we put more functionality into our containers (and more layers in our Dockerfiles) they can get large (though they will still be smaller than VMs and can share common layers if we are running more than one container on a host).   
Big containers take longer to download and spin up and consume more resources on repositories and target hosts.   

_How do we get around/avoid this?_   

### 3.2.1 Clever Dockerfile tricks
We can combine statements from layers into 1 layer using &&    
We can clean up temporary files e.g. if there is a build from source step we can use && ro have steps to delete the temporary and source files leaving only the executable and this will make that layer smaller when it is finalised into the container.   
An example of this:
```sh
USER root
ADD copy_course.sh /scripts/copy_course.sh
RUN chmod +x /scripts/copy_course.sh 
# need fastqc, samtools bwa bowtie picard-tools GATK jre wget git
RUN apt-get update && apt-get -y install bowtie bwa curl default-jre fastqc git gzip monit \
    picard-tools poppler-utils samtools sudo wget
RUN  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 
#fix fastqc
RUN mkdir /etc/fastqc && mkdir /etc/fastqc/Configuration
ADD fastqc/* /etc/fastqc/Configuration/

RUN mkdir $DOCS && mkdir $DATA &&  mkdir $WORK && mkdir /coursehome
```

_What are the issues with this?_    
* Dockerfile Readability - these 'tricks' can make it hard for the reader to understand 
what the Dockerfile is doing. This can severly impact on Reproducibility & Resuabilirt
(Last letter of FAIR principles).    
* Harder to debug any Syntax errors during build step. If the build breaks - which part of 
the line caused it to fail? How would you go about detrmining the cause?    

### 3.2.2 Using a different linux distribution e.g. Alpine
There are "Slimline" distributions like [Alpine linux](https://en.wikipedia.org/wiki/Alpine_Linux). These originate from the world of embedded devices e.g. routers and data-loggers where storage space is at a premium and so are ideal for building small linux appliances.   
* Downside - come with their own package managers (i.e. __apk__ not apt or yum) and so you will have to rewrite your installation recipes to use apk in the Dockerfile.   

### 3.2.3 Multi-stage builds
An update in Docker, this process allows us to have a Dockerfile that has two images associated with it. One is a build stage where software is built and installed then the destination stage (which is what the end user will use) is an compact empty OS image that has just the built software copied to it.    
See here for the [Docker documentation on this](https://docs.docker.com/build/building/multi-stage/).   
It's probably the best way to go for readable, compact containers.    

## 3.3 Publishing containers
We used the Docker hub repository but there are other choices:   
* [Quay,io](https://quay.io)   
* [Github Container registry ](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)  
* Many others free & commercial: AWS, Azure, Google, Gitlab, Harbor & Artifactory {See here](https://octopus.com/blog/top-8-container-registries)    
The upside is that the commands for connecting to the registry in order to push container to it are basically the same regardless of which repository you use (Though obviously you should read the relevant documentation).
An example of using the docker command line to push a container to GH Container registry is [here](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#pushing-container-images).
  
__Why use anything else?__   
Good question : politics & cost are faactors. For a long time Docker had a generous free tier ideal for learners and open source developers but they are now cutting back with an aim to monetise the Hub registry. Others have stepped in to try and take a share in the Market. Red Hats quay was an early contender but as stated above more have come to market especially the Cloud Computing service providers as they can run more Docker containers than VMs for the same server resources.  

More repositories are good for our reproducibilty agenda - we can host our containers on multiple services and be more resilient to service outages or vendors going out of business.
* A lot of the providers have supplied Github Actions (Which are effectively scripts that run Githubs servers) to automate container builds from a github code repository and storing the finished container in their container registry. Providing a Continuous Integration/Continuous Delivery (CI/CD) route to maintaing your software.
[See here for an article on  this for Githubs registry](https://docs.github.com/en/packages/managing-github-packages-using-github-actions-workflows/publishing-and-installing-a-package-with-github-actions#upgrading-a-workflow-that-accesses-a-registry-using-a-personal-access-token).
* For publications you can use Zenodo to [create a DOI pointing to your containers](https://docs.github.com/en/repositories/archiving-a-github-repository/referencing-and-citing-content#).  


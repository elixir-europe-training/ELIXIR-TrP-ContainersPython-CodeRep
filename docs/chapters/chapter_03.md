This is where the further resources go    
Put "Flamboyant title here"
## 3.1 flamboyant title

## 3.2 Shrinking containers
As we put more functionality into our containers (and more layers in our Dockerfiles) they can get large (though they will still be smaller than VMs and can share common layers if we are running more than one container on a host).   
How do we get around/avoid this?    
### 3.2.1 Clever Dockerfile tricks
We can combine statements from layers into 1 layer using &&    
We can clean up temporary files e.g. if there is a build from source step we can use && ro have steps to delete the temporary and source files leaving only the executable and this will make that layer smaller when it is finalised into the container.   
_What are the issues with this?_    
* Dockerfile Readability
* Harder to debug Syntax errors during build step

### 3.2.2 Using a different linux distribution e.g. Alpine
There are "Slimline" distributions like [Alpine linux](https://en.wikipedia.org/wiki/Alpine_Linux). These originate from the world of embedded devices e.g. routers and data-loggers where storage space is at a premium and so are ideal for building small linux appliances.   
* Downside - come with their own package managers (i.e. not apt or yum) and so you will have to rewrite your installation recipes to use those in the Dockerfile.


## 3.3 Publishing containers
We used the Docker hub repository but there are other choices:
* Quay,io
* Github Container registry
The upside is that the commands for connecting to the registry in order to push container to it are basically the same regardless of which repository you use (Though obviously you should read the relevant documentation)
- Why use anything else?
Good question : politics & cost    


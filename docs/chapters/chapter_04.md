## Learning outcomes

**After having completed this chapter you will be able to:**

- run the Python script using a Docker container on your computer.
- run the Python script using a Juptyer Notebook container on your computer.

## Material

TODO: add overview of necessary files, video, etc

[:fontawesome-solid-file-pdf: Download the presentation](../assets/pdf/docker_dance.pdf){: .md-button }

* Unix command line [E-utilities documentation](https://www.ncbi.nlm.nih.gov/books/NBK179288/)
 
## 4.1 Running the script within a Jupyter Notebook 

We will use Docker to start a JupyterNotebook within the JupyterLabs environment.

Working with JupyterLabs as a Docker extension begins with opening the Docker Desktop. 

Here are the steps to follow:

- Choose Extensions in the left sidebar.
- Switch to the Browse tab.
- In the Categories drop-down, select Utility Tools.
- Find Jupyter Notebook and then select Install.

Wait a bit before the JupyterLabs extension is installed. You can start the extension by the JupyterNotebook icon on the left side of the Docker Desktop menu.



```sh
docker run --rm --volumes-from jupyter_embedded_dd_vm -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /home/jovyan/work
```

## 4.2 Running the Python script on the command line 

TODO: add these sections


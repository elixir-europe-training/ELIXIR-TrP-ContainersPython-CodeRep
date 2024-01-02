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

Open a Terminal by clicking the Terminal icon under the Other category. Clone the directory by using `git clone`.
```
git clone https://github.com/elixir-europe-training/ELIXIR-TrP-ContainersPython-CodeRep.git
```

Open the Jupyter notebook in the `examples` directory of the said repository. You should be able to execute the Python code using the notebook.

In case, you like to extract the output from the notebook, you could run the following docker command. Obviously, you could also download the content via the interface.

```sh
docker run --rm --volumes-from jupyter_embedded_dd_vm -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /home/jovyan/work
```

## 4.2 Running the Python script on the command line 

In order to run the script using a Docker container, we will use the same Docker image which is used for running the JupyterLab in the example above.

!!! Exercise

```sh
docker images
docker image inspect jupyter/scipy-notebook:lab-4.0.6 | grep Working
```

Furthermore, please clone the above repo again on your local computer. Switch to the examples directory.

In order to run the script, we need to bind mount the local directory where the script and two python files are. Furthermore, we need to specify a working directory upon running the container (and we also need to specify a user and group ID for Linux hosts).

```sh
cd examples/
docker run --rm -v $(pwd):/home/jovyan/work/ -w /home/jovyan jupyter/scipy-notebook:lab-4.0.6 python codereppy_min_batch.py
```
  

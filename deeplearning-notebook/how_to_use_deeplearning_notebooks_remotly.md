Containerized technology is one of the major change in scientific world.
It allows **reproducible**, **large-scale** and **burden-free environment setup** for science.
In the mean-time, jupyter notebook provides a **interactive coding session** and easy way to work on python environment.
You will learn how to launch your favourite **notebook** on a **remote server** (from CRIUGM, or computecanada) using a specialized container technology called **[singularity](https://singularity.lbl.gov/)**, which allows you to run containers without sudo rights.

### Connect to the desired server

1. open a cmd prompt

2. connect on `thuya` (passwordless authentification is assumed)
```
ssh ${USER}@thuya
```

### (if necessary) Upload your data in the server

1. upload he notebooks
```
rsync -rlt --info=progress2 <my_local_file> ${USER}@thuya.criugm.qc.ca:~/path/where/you/want/<my_remote_file>
```

1. upload the database
```
rsync -rlt --info=progress2 <my_local_database> ${USER}@thuya.criugm.qc.ca:~/path/where/you/want/<my_remote_database>
```
**Please ensure that the data is not already available somewhere on** `/data/cisl`

### Launch the container

1. go to `/data/cisl/CONTAINERS`
```
cd /data/cisl/CONTAINERS

```
2. run the tensorflow cpu image
```
singularity exec -B <notebook_path>:/notebooks deep-neuro-docker.simg jupyter notebook --notebook-dir=/notebooks --no-browser --allow-root
```
or the GPU version with the `--nv` option
```
singularity exec --nv -B <notebook_path>:/notebooks deep-neuro-docker-gpu.simg jupyter notebook --notebook-dir=/notebooks --no-browser --allow-root
```

If you have a user space on the server and installed anaconda, you could have some trouble because singularity image mounts by default your home to the container. It can then load the library from the server/your local computer instead of the libraries inside the container !
To avoid this please use this command instead :
```
singularity exec -B <notebook_path>:/notebooks,<notebook_path>:/home/$USER --no-home deep-neuro-docker.simg jupyter notebook --notebook-dir=/notebooks --no-browser --allow-root
```
### Work on the notebook remotely !

1. Open a **new command prompt**

2. Create a ssh tunnel so you can work on your browser locally (even if it is running remotely)
```
ssh -L <server_port>:localhost:<server_port> pin
```
Where the output from jupyter on the remote indicates you the server port that is in use `http://localhost:<server_port>` :

<img src="notebook_weblink.png" width="500">

If nobody is using the server ports, it will be usually `8888`.

3. You can now just open a web browser with the jupyter hyperlink

*If you need other libraries for your application, it is possible to update the container.*

*Please ask to* : @ltetrel

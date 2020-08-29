Select EC2 (cloud server)

![](RackMultipart20200829-4-1vlwnkn_html_3f44b2bc2c75faa0.png)

Select Instance( cloud server)

![](RackMultipart20200829-4-1vlwnkn_html_4e262290af3a31db.png)

Launch Instance: create cloud server ![](RackMultipart20200829-4-1vlwnkn_html_98f86a8e81bc664e.png)

Select Ubuntu Server

![](RackMultipart20200829-4-1vlwnkn_html_23ab406dac2a452e.png)

The Free tier eligible( can not allocate the vCpu and Memory)

![](RackMultipart20200829-4-1vlwnkn_html_40b868541616fc1a.png)

Take note of subnet, may need to create a virtual hard disk. If hard disk is created in another region, then it will not be connected. The rest fields no need to change.

![](RackMultipart20200829-4-1vlwnkn_html_4198b294b5c7cc0.png)

default size is 8 GB, total free size is 30GB

![](RackMultipart20200829-4-1vlwnkn_html_7fa0d97a30505be4.png)

![](RackMultipart20200829-4-1vlwnkn_html_d139d3c29ef777fb.png)

The purpose of Tag is to manage Virtual Machine (ubuntu, linux…). When there are many VMs, the tag will help to differentiate the VMs. It is optional if there is few VMs.

SSH will turn on port 22. 0.0.0.0/0 means any ip address can visit port 22.

We add a rule, custom TCP, we use port 1357 for test, later need to tune jupyter port to match this port(1357) as the default port for jupyter is 8888. The default port 8888 can be easily scanned by others.

![](RackMultipart20200829-4-1vlwnkn_html_8b18e964b8c36067.png)

![](RackMultipart20200829-4-1vlwnkn_html_5fdc886af0a06c5.png)

When login by SSH, it cannot use normal password, need to use key pair. download the key pair and keep it. Then Launch instance.

![](RackMultipart20200829-4-1vlwnkn_html_f2207e5ab44f9b6.png)

When click instance ID (highlight part), will link to instance page, show details of this instance

![](RackMultipart20200829-4-1vlwnkn_html_70e04df4110a4e3.png)

![](RackMultipart20200829-4-1vlwnkn_html_4550e924db1a94da.png)

![](RackMultipart20200829-4-1vlwnkn_html_655787f09d0d0b9.png)

Now can use SSH to remote connect Ubuntu:

Put the pem file under the current path C:Users\XXXX\

ssh -i jupyter.pem [ubuntu@52.xx.xx.](mailto:ubuntu@52.xx.xx.143)xx (key in your own public IP address)

![](RackMultipart20200829-4-1vlwnkn_html_9f0e573450b006ce.png)

![](RackMultipart20200829-4-1vlwnkn_html_9182be3a1e47891d.png)

Now select Volumes ( create hard disk)

![](RackMultipart20200829-4-1vlwnkn_html_cebeb2b532b7ec8f.png)

Create volume

![](RackMultipart20200829-4-1vlwnkn_html_d6d8932258e05e14.png)

![](RackMultipart20200829-4-1vlwnkn_html_b2b295926da1032c.png)

Tick the created volume, under action, select attach Volume

![](RackMultipart20200829-4-1vlwnkn_html_f05e10a8f2b59e79.png)

![](RackMultipart20200829-4-1vlwnkn_html_60c5e4870fae2e71.png)

![](RackMultipart20200829-4-1vlwnkn_html_9c4b1909f6c78e1d.png)

![](RackMultipart20200829-4-1vlwnkn_html_6b8f0346d7840553.png)

![](RackMultipart20200829-4-1vlwnkn_html_de3e3b71ba2a08ae.png)

The state of volume become &quot;in-use&quot;, Now the Volume(virtual hard disk is attached to the cloud server(ubuntu)

Now in the cmd, ls /dev, will shows devices. And just now the created hard disk &quot;xvdf&quot; is displayed, this is similar like thumb drive, need mount to the system

![](RackMultipart20200829-4-1vlwnkn_html_724b079c20e795a.png)

mkdir conda

sudo mount /dev/xvdf conda

sudo mkfs /dev/xvdf --this is to format &quot;xvdf&quot; device before use

sudo mount /dev/xvdf conda

![](RackMultipart20200829-4-1vlwnkn_html_e0b5dfeecab18688.png)

Use wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86\_64.sh

![](RackMultipart20200829-4-1vlwnkn_html_a3f0be14486a9f34.png)

sudo chown ubuntu conda

sudo chgrp ubuntu conda

Use wget [https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86\_64.sh](https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh)

Using bash to install conda

![](RackMultipart20200829-4-1vlwnkn_html_d02e406b0e526610.png)

Check licenses, then type yes

![](RackMultipart20200829-4-1vlwnkn_html_db9eb12974448cb.png)

It will prompt message to ask where to install the miniconda:

/home/ubuntu/conda/miniconda3

Do you wish the installer to initialize Miniconda3 in your /home/ubuntu/.bashrc[yes|no]

Yes

Once the miniconda3 is installed, we need to run the &quot;initialize shell script&quot;. There are 2 ways. First is just quit conda and login in again. 2rd is to run &quot;ubuntu@ip-172-31-15-220:-$ ~conda$ . ~/.bashrc&quot;

Once it successfully installed,

conda upgrade conda

now create virtual environment

conda create -n jupyter python=3.8 -y

Once the environment:

conda activate jupyter

the (jupyter) means it is in jupyter environment now

pip install jupyter

mkdir jupyter\_hom

cd jupyter\_home/

jupyter notebook

y

ubuntu@ip-172-31-15-220:-$ ~conda/jupyter\_home$ python

\&gt;\&gt;\&gt; from IPython.lib import passwd

\&gt;\&gt;\&gt;passwd()

Enter password:

Verify password:

&#39;sha1:665bf9279840:c7b38e4315e0d2e740594a081e563746f5123e5e&#39; this will used to config jupyter config file

Jupyter notebook –generate-config

![](RackMultipart20200829-4-1vlwnkn_html_9a02cc84a2d856fc.png)

vim jupyter\_notebook\_config.py

(press i enter edit mode; press esc, :wq then save and quit)

c.NotebookApp.allow\_remote\_access = True

c.NotebookApp.ip = &quot;\*&quot;

c.NotebookAPp.open\_browser = False

c.NotebookApp.password = &#39;sha1:665bf9279840:c7b38e4315e0d2e740594a081e563746f5123e5e&#39;

#c.NotebookApp.certfile = u&#39;/root/.jupyter/jupyter.pem&#39;

c.NotebookApp.port= 1357

c.NotebookApp.notebook\_dir = &quot;/home/ubuntu/conda/jupyter\_home/&quot;

![](RackMultipart20200829-4-1vlwnkn_html_69af5178f848ddc0.png)

Run the jupyter notebook

![](RackMultipart20200829-4-1vlwnkn_html_c9588572a9db074.png)

![](RackMultipart20200829-4-1vlwnkn_html_8e3c6a77c482f9a5.png)

Once the terminal is close, the url can not run or support. To able to run the jupyter even when terminal is closed.

![](RackMultipart20200829-4-1vlwnkn_html_31434ca778afd055.png)

Run nohup jupyter notebook &amp;

Nohup helps to run the command in a terminal will never shutdown.

![](RackMultipart20200829-4-1vlwnkn_html_c76d3489dcf1895f.png)

To kill the jupyter notebook

Run: pkill jupyter

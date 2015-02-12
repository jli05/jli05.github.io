---
title: Setup a Theano-based ConvNet environment
layout: post
---

<em>Keywords/tags</em>: Theano, Lasagne, nolearn, ConvNet

Below are the steps to launch an AWS EC2 GPU instance with Ubuntu 14.04.1 LTS, and install the ConvNet-related packages <code>Theano</code> <code>Lasagne</code> <code>nolearn</code> <code>pylearn2</code> in a <code>virtualenv</code>. We use Python 3.4 throughout.

<ol>
<li><p>Launch an AWS EC2 GPU instace (g2.2xlarge) with Ubuntu 14.04.1 LTS.</p></li>

<li><p>Upgrade everything.
<pre><code>sudo apt-get update
sudo apt-get upgrade
sudo apt-get install linux-headers-generic linux-headers-virtual linux-image-virtual linux-virtual
sudo reboot</code></pre></p>
<p>Keep the package maintainer's version of GRUB list if prompted.</p></li>

<li><p>Install numerical libraries.
<pre><code>sudo apt-get install gfortran liblapack3 liblapack-dev libopenblas-base libopenblas-dev</code></pre></p>
<p>Add the following line to <code>/etc/environment</code>
<pre><code>OPENBLAS_NUM_THREADS=8</code></pre></p></li>

<li><p>Install CUDA 6.5. CUDA requires <code>drm</code> kernel module which is included in <code>linux-generic</code> but not <code>linux-virtual</code>.
<pre><code>sudo apt-get install linux-generic</code></pre></p>
<p>Google for "nvidia getcuda", then curl down the .deb file for Ubuntu 14.04 64-bit.
<code><pre>sudo dpkg -i &lt;cuda&gt;.deb
sudo apt-get update
sudo apt-get install cuda</pre></code></p>
<p>Post install, add the following lines to <code>/etc/environment</code>:
<pre><code>PATH="/usr/local/cuda-6.5/bin:$PATH"
LD_LIBRARY_PATH="/usr/local/cuda-6.5/lib64:$LD_LIBRARY_PATH"</code></pre></p>
<p>Reboot. Run <code>nvidia-smi</code>. Below is a typical output (GPU-Util is 0% as there is no job running on the GPU). Make sure you don't get any error running <code>nvidia-smi</code>.
<pre>
+------------------------------------------------------+                       
| NVIDIA-SMI 340.29     Driver Version: 340.29         |                       
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GRID K520           Off  | 0000:00:03.0     Off |                  N/A |
| N/A   38C    P0    35W / 125W |     10MiB /  4095MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
</pre></p></li>

<li><p>Install the basic Python 3.4 packages. Then install <code>virtualenvwrapper</code>.
<pre><code>sudo apt-get install python3-pip python3-numpy python3-scipy python3-pandas python3-matplotlib ipython3
sudo pip3 install -U virtualenvwrapper</code></pre></p></li>

<li><p>Set up <code>virtualenvwrapper</code>. We're going to put all ConvNet-related packages in a <code>virtualenv</code>. As all the neural network-related packages are in active development and far from complete and mature, it is <strong>necessary</strong> to work with them within a <code>virtualenv</code>. All packages installed under a <code>virtualenv</code> are independent from the system ones.</p>
<p>First some utilities to install as preparation.
<pre><code>sudo apt-get install git libyaml-dev</code></pre></p>
<p>Create a directory <code>~/Env</code> to host all the <code>virtualenv</code>s, then add the following lines in <code>~/.profile</code>
<pre><code>export WORKON_HOME=~/Env
export VIRTUALENVWRAPPER_PYTHON='/usr/bin/python3'
source /usr/local/bin/virtualenvwrapper.sh</code></pre>
Type <code>logout</code> at the bash prompt to logout, then re-login. Do
<pre><code>makevirtualenv env1
workon env1
cd ~/Env/env1
pip install numpy
pip install scipy
pip install pandas
pip install git+https://github.com/theano/theano.git
pip install git+https://github.com/benanne/Lasagne.git
pip install git+https://github.com/dnouri/nolearn.git
git clone https://github.com/lisa-lab/pylearn2.git
cd pylearn2
python3 setup.py develop
</code></pre>
Note the way to install <code>pylearn2</code> is distinct from the other ones.</p>
<p>Type <code>workon &lt;env&gt;</code> to enter a <code>virtualenv</code>, type <code>deactivate</code> to quit.</p></li>

<li><p>Voila! We're setup now. Follow the <a href="http://danielnouri.org/notes/2014/12/17/using-convolutional-neural-nets-to\
-detect-facial-keypoints-tutorial/">Facial Keypoints Detection Tutorial</a> for a quick walk-through about ConvNet, optimisation tweaking, data augmentation and Dropout.</p>
<p>Your could find the <a href="https://github.com/dnouri/kfkd-tutorial">Facial Keypoints Detection Tutorial Code</a>. Remember always do <code>workon &lt;env&gt;</code> and <code>deactivate</code> before experimenting with the code.</p></li></ol>




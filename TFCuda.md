<h1> Install Tensorflow with GPU support</h1>

1 - Delete previous TF installations from ALL dist-packages (in Python 2 and/or Python 3, DON'T FORGET THE ANACONDA FOLDER)


2 - Install NVIDIA drivers http://www.webupd8.org/2016/06/how-to-install-latest-nvidia-drivers-in.html

	Don't take the last version (take like 367) because it isn't stable yet.

	AT LEAST check with the command nvidia-smi: if not found, not good


3 - Follow tutorial here https://alliseesolutions.wordpress.com/2016/09/08/install-gpu-tensorflow-from-sources-w-ubuntu-16-04-and-cuda-8-0-rc/

	- WARNING: If python was installed with anaconda, make sure that the Python Path in TF configuration step are in the Anaconda Folder - was default for me

	- SECOND WARNING: the CUDNN Version to put in the configuration is 5 and not 5.1.5)

	- THIRD WARNING: might get an error at the "Build & Install Pip Package" part, but easy to workaround


4 - Test with python // >>> import tensorflow as tf

	- If that works, try sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
	
	- It should display your GPU. If it doesn't, or if the previous command led to error messages, THAT becomes interesting. Go to step 5.

	- If you didn't get any error message but the GPU isn't there, you probably don't use the "right" TF, which means that you didn't clean up all folders before install. Try doing that, as well as installing NVIDIA drivers if it doesn't work


5 - You probably got a message like "Couldn't open CUDA library libcuda.so.1". It takes a few steps to solve this:

		- Update LD_LIBRARY_PATH: export LD_LIBRARY_PATH=/usr/local/cuda/lib64

		- Find libcuda.367.57 (or the version of your NVIDIA driver) with find / -iname 		  "libcuda.so.1". It should appear at least in one folder apparently unrelated to CUDA, like /usr/lib/i386-linux-gnu or /usr/lib/x86_64-linux-gnu/. There you can also find the file libcuda.367.57.

		- If it doesn't exit, try sudo apt-get install libcuda1-367

		- Go to your /usr/local/cuda/lib64 folder and create a symbolic link to the previous file: 
		sudo ln -s /usr/lib/x86_64-linux-gnu/libcuda.so.367.57 libcuda.so.1

		- If the destination file already exists, delete it and retry

6 - That's where I won: FIREWORKS


USEFUL LINKS:

https://github.com/tensorflow/tensorflow/issues/4078
https://devtalk.nvidia.com/default/topic/975766/tensorflow-import-error-quot-couldn-t-open-cuda-library-libcuda-so-1-quot-ubuntu-14-04-cuda-8-0-dell-7559-i7/



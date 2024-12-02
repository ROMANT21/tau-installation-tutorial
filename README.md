# Installing Tau
While trying to download and install Tau for a project, I found it quite annoying that I had to read 3 to 4 articles simutaneously for 30 minutes to get the tool working, so here's a quicker tutorial to get you started.

P.S. If you find any problems with this tutorial, just place an issue and I'll definitely look into it!

## Preliminary information
You're going to want to know:
* The path to your MPI lib directory
* The path to your MPI include directory
* The path of your C compiler (same as the used to compile MPI)
* The path of your C++ compiler (same as the used to compile MPI)

Also, you should install java for the visualization tool.

> [!NOTE]
> If you don't have some of these, like MPI, you'll have to install it first.

For me this, was:
**MPI Lib**: /usr/lib/x86_64-linux-gnu/openmpi/lib
**MPI DIR**: /usr/lib/x86_64-linux-gnu/openmpi/include
**C Compiler**: /usr/bin/gcc
**C++ Compiler**: /usr/bin/g++

## Installing PDT
The only reqyurement for Tau is PDT, which if I'm not mistaken, gives you visualization tools for Tau.

To get the pdt tar, unpack it, and configure it, run:
```{bash}
wget http://tau.uoregon.edu/pdt.tgz
tar -xvzf pdt.tgz
cd pdt
./configure -GNU
```

> [!NOTE]
> Notice that I used `-GNU` for configuration. This is because I'm using gcc and g++ as my compilers. This may be different for you.

At the end of configuration, it should tell you to add PDT to your path. It'll say something like:
```
Add "/home/users/roma/pdtoolkit-3.4/ppc64//bin" to your path
```

Copy the file path and run the following command, to add pdt to your path:
```{bash}
export PATH=$PATH:/home/users/hoge/pdtoolkit/ppc64/bin
```

> [!NOTE]
> You should really add the file path to your bashrc file that way when you open a new terminal, you don't have to repeatedly run the `export` command.

Finally, you can install:
```{bash}
make -j
make install
```

## Installing Tau
To get the tar file for Tau, you need to go to: https://www.cs.uoregon.edu/research/tau/downloads.php and go through the process for downloading Tau. Once entering some contact information, you should be met with an installation page. Install the tar file.

Once you've installed the tar file, run:
```{bash}
tar -xvzf tau_latest.tar.gz
cd tau_latest
./configure -c++=g++ -cc=gcc \
-mpiinc=/usr/lib/x86_64-linux-gnu/openmpi/include \
-mpilib=/usr/lib/x86_64-linux-gnu/openmpi/lib
-pdt=/home/users/roma/pdt
```

> [!NOTE] 
> The path for `-pdt` is the path to the unpacked PDT folder where we ran the installation steps for PFT.

Similar to before you should be met with a message telling you to add Tau to your path, and just like before, we can add it to our path by running:
```{bash}
export PATH=$PATH:/home/users/roma/tau2/ppc64/bin
```

Finally, we can install tau by running:
```{bash}
make install
```

## Example using Tau
The simplest example can be found in the tau examples directory at: `examples/taututorial`, but I had trouble running that example, so instead you can use the code in this repository.

The only thing you have to do is set the TAU_MAKEFILE variable:
```{bash}
export TAU_MAKEFILE=/home/users/roma/tau2/ia64/lib/Makefile.tau-mpi-pdt
```

Once that is done, you can run the following commands to compile the code:
```{bash}
tau_cxx.sh -c computePi.cpp -o computePi.o
tau_cxx.sh computePi.o -o computePi
```

To run it, we use mpi executables:
```{bash}
mpirun -np 5 ./computePi
```

You'll notice some profiling files that are made for each process. To view them in a nice fashion, we can use:
```{bash}
paraprof
```

> [!NOTE]
> You need to install java to use paraprof!

## Relevant Links
Almost all of this tutorial was adapted from these resources:
* https://www.cs.uoregon.edu/research/tau/docs/tutorial/index.html
* https://github-wiki-see.page/m/UO-OACISS/tau2/wiki/Installing-TAU


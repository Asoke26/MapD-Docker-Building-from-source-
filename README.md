# MapD Docker GPU cuda
Building MapD from source in a cuda docker image.

1. Install docker  
   https://docs.docker.com/engine/install/ubuntu/

2. Pull cuda runtime docker from docker hub  
   docker pull nvidia/cuda:11.2.2-cudnn8-devel-ubuntu18.04

3. Clone latest omnisci(Previously known as MapD) repository from github.  
   git clone git@github.com:omnisci/omniscidb.git

4. Replace llvm patch file under script directory with above patch file.  

# Build
mkdir build  
cd build  
cmake -DCMAKE_BUILD_TYPE=debug ..  
make -j 4  

 

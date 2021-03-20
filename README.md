# MapD Docker GPU cuda
Building MapD from source in a cuda docker image.

1. Install docker and nvidia docker toolkit from below link  
   ```https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker```

2. Pull cuda runtime docker image from docker hub and run  
   ```
   docker pull nvidia/cuda:11.2.2-cudnn8-devel-ubuntu20.04  
   docker run -it --name container_name --gpus device=0 docker-image-id bash ```
   
3. Install necessary packages.
   ```
   apt-get update
   aet-get upgrade
   apt install git build-essential vim
   ```
5. Clone latest omnisci(Previously known as MapD) repository from github.  
   ```git clone https://github.com/omnisci/omniscidb.git```

4. Replace omniscidb/scripts/llvm-9-glibc-2.31-708430.patch file content with below file content.
   ```
   https://github.com/Asoke26/mapdcudadocker/blob/main/llvm-9-glibc-2.31-708430.patch
   ```

# Build
```
mkdir build  
cd build  
cmake -DCMAKE_BUILD_TYPE=debug ..  
make -j 4  
```

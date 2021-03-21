# OmniSciDB (formerly MapD Core) | Building from source 
Building MapD from source in a cuda docker image.

Assuming nvidia related drivers are installed in host machine. Verify by running ```nvidia-smi```

1. Install docker and nvidia docker toolkit from below link  
   ```https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker```

2. Pull cuda runtime docker image from docker hub and run  
   ```
   docker pull nvidia/cuda:11.2.2-cudnn8-devel-ubuntu20.04  
   docker run -it --name container_name --gpus device=0 docker-image-id bash 
   ```
   
3. Install necessary packages.
   ```
   apt-get update
   aet-get upgrade
   apt install git build-essential vim doxygen
   ```
5. Clone latest omnisci(Previously known as MapD) repository from github.  
   ```git clone https://github.com/omnisci/omniscidb.git```

4. Replace omniscidb/scripts/llvm-9-glibc-2.31-708430.patch file content with below file content.
   ```
   https://github.com/Asoke26/mapdcudadocker/blob/main/llvm-9-glibc-2.31-708430.patch
   ```
5. Replace 'sudo' keyword from script and Install all MapD dependencies.
    ```
    sed -i 's/sudo//g' omniscidb/scripts/mapd-deps-ubuntu.sh  
    source omniscidb/scripts/mapd-deps-ubuntu.sh
    ```
6. Once all the dependencies are installed (It may take a while) run below script to set all the environment variable( everytime you restart the docker you need to run this script.
   ```
   source /usr/local/mapd-deps/mapd-deps.sh  
   ```

7. Build
   ```
   mkdir build && cd build  
   cmake -DCMAKE_BUILD_TYPE=debug ..  
   make -j $(nproc)  
   ```
8. Start Server
   a)Create data directory and initialize
      ```
      mkdir /home/data && ./bin/initdb /home/data
      ```
   b) Start server. From omnisci/build/ 
      ```
      ./bin/omnisci_server --data /home/data
      ```
   c) Start client.
      ```
      ./bin/omnisql -p HyperInteractive
      ```

Issues : 
1) During build phase I faced below error  
``` c++: fatal error: Killed signal terminated program cc1plus ```  
Solution : Increased swap size in host machine by 64 GB. Please find details below.  
```https://askubuntu.com/questions/1264568/increase-swap-in-20-04```

Reference :  
1) Omniscidb (https://github.com/omnisci/omniscidb)

Contact :  
https://www.linkedin.com/in/ad26/  
https://asoke26.github.io/adatta2/

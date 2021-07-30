  (remember . means work directory)
1. When you build docker file, from a folder, this folder called build context

2. then you can use docker cli to build your image, there are 
  certain things that you can do with docker cli, for example:

  docker run nginx:<version>   (will run the nginx image, if it is present 
                                  in the host, if it is not, will dl from docker
                                  hub, if you don't specified the tag the default is latest)

  docker run --name <name> <image name>

  docker pull nginx   (will pull the image and not running it) 

  docker run ubuntu   (will run the ubuntu container and exit, because containers unlike virtual machines are 
                        not design to run the operating system, they are designed to run the task and if the 
                        task is completed or crashed, the container will be stopped)

  docker exec   <name of the running container> cat /etc/hosts       (by this we can execute the command on running
                                                                        container)

  docker run -d <name of the image>                                 (by this the container runs in the background
                                                                      and you can use the terminal again)

  docker attach <id of the container>             (you can attach it back to the terminal)

  docker images       (to see all the images in your computer)

  docker rmi <image name or id>     (to remove the docker image)

  docker ps           (to show the containers that running)

  docker ps -a        (to see all running and stoped containers)

  docker stop <container ID or container name>    (this will stop 
                                                    the container)
  docker start <container ID or container name>
    
  docker rm <container ID or container name>      (to remove the 
                                                    container permanently)

  docker build .      (to build the directory that is in the
                        folder that you are in it)

  docker build . -t <give tag name>    (it is good practice
                                            to give your images
                                            tag name)

  docker build -f /path/to/a/Dockerfile (to build a docker file from a different
                                          locations)

  docker push <your image name that give it wit -t>     (you can push your docker image to ducker hub)

  docker run -i <image name>            (if you have stdin in your application, docker doesn't have the ability 
                                        to take the input, with -i you run your image in the interactive way
                                        and now you can add input)

  docker run -it <image name>           (docker don't have terminal so even you can add input, you can't add output
                                        in the terminal by using -it t stands for psuedo terminal, now we can output
                                        to terminal in our application)

  docker run <image for a webapp>       (now you can access your webapp in the docker host by the ip address of the 
                                        docker container(every docker container by default has ip address) and the port
                                        that app run on it, but because this is the internal ip address the user outside
                                        the docker host can not access this, so we need to use the ip address of the 
                                        docker host but you have to map the free port of the docker host to the port
                                        that contaienr is running on it)

  docker run -p <docker host port>:<container port> <image name>    (you can run as many as app on different port)

  docker run mysql                      (all the data that we put to the db will store in the /var/lib/mysql in the 
                                        docker filesystems, and as soon as you stop the container and delete it all
                                        data will be gone)

  docker run -v <address in the docker host>:/var/lib/mysql mysql    (this way all the data in the container will be map
                                                                     in the folder in your docker host)

  docker inspect <container id or name>             (get all the information about the container in the JSON format)

  docker logs <container name or id>      (if you run your container in the detach mode, in this way you can see the 
                                          logs of your docker container)

  docker run -e <environment variable name>=<value> <image name>     (you can set the environment variable in this way)

  docker inspect <docker name or id>          (in the config section of the json file you can see the list of environment
                                              variables in the container that is running)

  docker run <image name> <command in shell format>         for example:

  docker run ubuntu sleep 5           (in the Ubuntu image dockerfile there is a CMD["bash"] in the end and the ubuntu
                                      image uses the bash as a default command, bash is a shell in linux based os and 
                                      it will listen for the keyword inputs in terminal software but, by default the 
                                      contaienr does not have terminal, so when you run the ubuntu image, it exit after
                                      this command, but we can override this default command by giving the command
                                      in shell format in the run cli or you can change the CMD in ubunt CMD sleep 5 or
                                      CMD["sleep", "5"])

  docker run --name=<name of contaienr> <name of the image>   (you can give the name for your container)

3. In DockerFile we have     INSTRUCTION ARGUMENT

4. Every docker file should start whit FROM, unles we use ARG or parser directive like escape that is like comments,
  and every images should start with os based image (like ubuntu) or an image that itself started with os based image

[ATTENTION]
5. Every step (layer) of docker file is independent of another, so RUN cd/tmp will
  not have any effect on the next instruction.

6. he RUN instruction will execute any commands in a new layer on top of the 
  current image and commit the results. The resulting committed image will be
  used for the next step in the Dockerfile 

7. Run instruction has two form: 
      
      a. RUN <command> (shell form, the command is run in a shell,
         which by default is /bin/sh -c on Linux or cmd /S /C on Windows)

      b.RUN ["executable", "param1", "param2"] (exec form)

[ATTENTION]
8. The shell is the program that takes commands from keyboard and gives them 
    to the operating system to perform. on most linux systems this program 
    is bash. there are also other programs that can be installed like ksh, tcsh
    and zsh 

9. terminal is a program that let you interact with shell

[ATTENTION]
10. remember every step in the dockerfile is a layer, try to 
    run different things in one line to avoid building the large
    docker file with many layers in it.

11. the CMD instruction has three forms:

  a. CMD ["executable","param1","param2"] (exec form, this is the
                                           preferred form)
  b. CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
  c. CMD command param1 param2 (shell form)

[ATTENTION]
12. The main purpose of a CMD is to provide defaults for an executing
    container. These defaults can include an executable, or they can
    omit the executable, in which case you must specify an ENTRYPOINT
    instruction as well.

[ATTENTION]
13. There can only be one CMD instruction in a Dockerfile. If you
 list more than one CMD then only the last CMD will take effect.

14. Do not confuse RUN with CMD. RUN actually runs a command and
    commits the result; CMD does not execute anything at build time,
    but specifies the intended command for the image at the run time.

15. Unlike the shell form, the exec form does not invoke a command
    shell. This means that normal shell processing does not happen.
    For example, CMD [ "echo", "$HOME" ] will not do variable 
    substitution on $HOME. If you want shell processing then either
    use the shell form or execute a shell directly, 
    for example: CMD [ "sh", "-c", "echo $HOME" ]. When using the 
    exec form and executing a shell directly, as in the case for the
    shell form, it is the shell that is doing the environment 
    variable expansion, not docker.

16. If you use the shell form of the CMD, then the <command> will
    execute in /bin/sh -c

17. ENTRYPOINT is just like CMD but you specify the params of executeable in the docker cli, for example in ubuntu 
    image you can use ENTRYPOINT["sleep"] and build the new image, and use "docker run ubuntu-sleeper 10"

18. you can also override the executable in the ENTRYPOINT[""] that exists in the dockerfile in docker cli:

    docker run --entrypoint <executable> <image name> <param1>
    docker run --entrypoint sleep2.0 ubuntu-sleeper 10

19. but if you don't specify the 10 in the docker cli we will get an error, to provide default for the ENTRYPOINT,
    we can use ENTRYPOINT and CMD together: 

        ENTRYPOINT["sleep"]

        CMD["10"]

20. The EXPOSE instruction informs Docker that the container listens 
    on the specified network ports at runtime. You can specify 
    whether the port listens on TCP or UDP, and the default is TCP
    if the protocol is not specified:

          EXPOSE 80/udp

21. ENV has two form in docker: 

      ENV <key> <value>
      ENV <key>=<value> ...

22. In the first form, everything after first space, is considered 
    as the value, even the spaces

23. ADD in docker files has two form: 

      ADD [--chown=<user>:<group>] <src>... <dest>
      ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]

24. about the chmod and chown commands: 

      a. here are three sets of permissions. One set for the owner
         of the file, another set for the members of the file’s
        group, and a final set for everyone else.

      b. you can type ls -l to see the files and folders permission

      c. this two are the example of the ls -l result: 

    drwxr-xr-x 2 dave dave 4096 Aug 23 08:02 archive

    -rw-rw-r-- 1 dave dave 780 Aug 20 11:11 command_cls.page

d the first charecter means it is folder
- means it is file
the first three charecters are for the owner permissions
the next three charecters are for the group members permission
and the next three is for everyone else

for example chmod u+x <file or folder>

(+)add x(execute access) to the user(owner)
(+ - =) = means remove other permissions and add only the one that 
  we specify
u: user g: group o: other a: all not specifyin: all

example: chmod u=rw,og+r new_file.txt

we can also give permission for different users by number:
chmod 764 *.page
7 is for the owner(and it is 111 (has read, write and, execute))
6 is for the group member(it is 110(has read and write))
4 is for the others (it is 100 (has read))

25. there are three network that container attatched to, bridge, none, host the default one is bridge, you can specify
    the network for the container:

        docker run ubuntu --network=none
        docker run ubuntu --network=host

26. the bridge is the private internal network that each container will be in it, and each container has a ip address
    that other containers can talk to them if it is required. the range of ip address is 172.17.0.1/16

27. if you run your container on the host network, you can access your webserver in the container with the port of 
    app, without port mapping 

28. with the none network, the containers, doesn't attatched to any network and it is isolated

29. you can create other network in the brige with other ip address: 

      docker network create --driver bridge --subnet 182.18.0.0/16 <network name>

30. and you can see the networks by:
    
      docker network ls

31. you can see the the network setting of the  container in network section of the contaienr setting:

    docker inspect <container name or id>

32. each container in the bridge network can reach other ontainers by their ip address, but the it is not ideal,
    because each time you reboot your system, the ip address will change, so you can reach it with container name
    for example in your web server container, you can access the mysql container:

          mysql.connect(<mysql container name>)          (because the docker has internal DNS server)

[ATTENTION]
33. when you build your image, docker build each layer for each instruction in the docker file, these layers are
    READ_ONLY, and you can not change them, unless you build your image again, but when you run your image, docker
    build WRITABLE layer for the container, and you can change files, store file and ... in this layer, but when the 
    contaienr is destroyed, all these files will be destroyed.

34. When we change the files in docker image, we actually make a copy of these files in the WRITABLE layer, and we change
    them, we don't change the original files, and when the container is destroyed, all the files in the container will be
    destroyed as well.

[ATTENTION]
35. In linux based machine the structure of the docker files, will be like this: 

        /var/lib/docker
          a. aufs folder
          b. containers folder
          c. image folder
          d. volumes folder

36. to save the files in the container, we have two option, the first one is to map the folder form container file
      system to your docker host for example:  (docker run -v <address in the docker host>:/var/lib/mysql mysql)
      and the second one is to create docker volume, and use these volumes in docker contaienr: 

            first create docker volume under /var/lib/docker/volumes  folder:

                  docker volume create <volume name>

            then mount the docker container volume to the docker host volume: 

                  docker run -v <volume name>:/var/lib/mysql mysql      (if you don't create the volume before
                                                                        the docker will create it for you)

                                                                        (if you want to bind to the optional folder
                                                                        that is not in the volumes folder of the 
                                                                        /var/lib/docker you can provide absolute
                                                                        path for the we call this bind mounting)

37. If you want to link to containers you can use --link for example: 

      docker run -d --name=vote-app -p 5000:80 --link redis<this is container>:redis<this is the host that app look for> <name of the image>
      
38. example of the docker-compose.yml if all the images already built: (this is version 1 of docker compose) 

      redis: (this is the container name the in single docker command will be specified with --name=redis) 
        image: redis
      db:
        image: postgress:9.4
      vote:
        image: voting-app
        port: 
          - 5000:80
        links:
          - redis   (this is same as --link redis:redis you can ommit the second one, if they are same name)
      result:
        image: result-app
        port: 
          - 5001:80
        links:
          - db
      worker:
        image: worker
        links: 
          - redis
          - db

then you can use "docker-compose up" to bring up all the stack

39. in the above example, we assume that all the images already has been built, if the image is not built you have 
    to change the image with build. for example: 

        vote:
          build:
            context: ./blockchain-iot-platform    (this is location of the app folder)
            dockerfile: Dockerfile
            args:
              - typeOfContainer=localdev

40. this .yml format is for the version 1 of the docker compose, in version 2 we have: 

      version: 3.7

      services:
        redis: (this is the container name the in single docker command will be specified with --name=redis) 
          image: redis
        db:
          image: postgress:9.4
        vote:
          image: voting-app
          port: 
            - 5000:80
          depends_on:
            - redis 
        result:
          image: result-app
          port: 
            - 5001:80

        worker:
          image: worker


[ATTENTION]
41. in docker compose version 2 or up, docker automatically build new bridge network, and uses services names to connect 
    to each other, so you don't need any links
[ATTENTION]
42. in version 2 or higher, you can also specify the depends_on to show that one app needs another app, before the runnig

[ATTENTION]
43. for version 2 or up, you have to specify the version of the docker compose file in the beginning

44. for version 2 or higher, you can specify the network for your services: 

      services: 
        redis:
          image: redis
          networks:
            -back-end
        result: 
          image: result
          networks: 
            - back-end
            -front-end

      networks: 
        front-end:
        back-end:

45. When you install docker extension in the vs code, you can see all
  your images and all your containers, 

46. Then you can use this extension to run your docker image or 
  use docker cli or use vs code palette for docker cli (type docker)

47. Then you can see that your docker images is running in the 
  containers section of the vs code extension

48. docker compose is used to run multiple containers together,
  to use docker compose we need to make docker-compose.yml file
  (docker compose is the part of docker dektop in mac and windows)

49. you can search for docker compose for different apps, 
  for example search for docker compose in wordpress:
  check docker-compose.yml in this folder 

50. then you can run the docker composer by using: 

        docker-compose up -d

51. Volumes in container are the folders that we can generate in 
    the container and mount a folder from the machine to the 
    generated folder, and when we add data to the generated folder
    in the container, the data will be saved to the folder that 
    mounted, and will be saved in the machine

52. you can create docker hub account and add your image to 
  the docker hub:

    a. first create the account
    b. then make a new repository
    c. login from terminal:
          docker login -u <username>
          ask for the passowrd:
          <password>
    d. docker tag <image ID> <created repository>
    e. docker push <created repository>

53. SSH is a protocol to send data in the unsecure environment 
    like internet, 

      (one computer)                  (another computer)
      SSH client                      SSH server
      private key                                public key


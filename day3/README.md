# k8s-cloud4c-b2

### Revision of Docker containers

<img src="rev1.png">

### similar to docker 

<img src="rev2.png">

### Dockerfile FROM is taking image from Docker hub 

<img src="dh.png">

### java code containerization 

### dockerfile 

```
FROM openjdk
LABEL name=ashutoshh
RUN mkdir /mydata
COPY ashu.java /mydata/ashu.java 
WORKDIR /mydata
# is to change directory like cd commnad 
# during docker image build time 
RUN javac  ashu.java 
# compiling code 
CMD ["java","ashu"]

```

### java code 

```
class ashu { 
    public static void main(String args[]) 
    { 
        // test expression 
        while (true) { 
            System.out.println("Hello World"); 
            try {
                Thread.sleep(2000);
            } catch (Exception ex) {
                // Ignored
            }
  
            // update expression 
        } 
    } 
} 
```

### lets build java image 

```
[ec2-user@docker ashu-docker-images]$ ls
java-code  python-code  webapps
[ec2-user@docker ashu-docker-images]$ ls  java-code/
ashu.java  Dockerfile
[ec2-user@docker ashu-docker-images]$ docker build -t ashujava:v1  java-code/
Sending build context to Docker daemon  3.072kB
Step 1/7 : FROM openjdk
 ---> 71260f256d19
Step 2/7 : LABEL name=ashutoshh
 ---> Running in 2b8daf0a5747
```

### test and remove 

```
[ec2-user@docker ashu-docker-images]$ docker  images   |  grep ashu
ashujava        v1             71e3c498a5d5   6 minutes ago        470MB
ashupython      codev1         343f8464a8ed   24 hours ago         920MB
[ec2-user@docker ashu-docker-images]$ 
[ec2-user@docker ashu-docker-images]$ docker  run -itd  --name ashujc1  ashujava:v1 
b718841df4272f8406012d7782976a8283d02e5bf83f1208799d077b34785f42
[ec2-user@docker ashu-docker-images]$ docker  ps
CONTAINER ID   IMAGE         COMMAND       CREATED         STATUS        PORTS     NAMES
b718841df427   ashujava:v1   "java ashu"   2 seconds ago   Up 1 second             ashujc1
[ec2-user@docker ashu-docker-images]$ docker logs ashujc1
Hello World
Hello World
Hello World
Hello World
Hello World
Hello World
[ec2-user@docker ashu-docker-images]$ docker  stop ashujc1
ashujc1
[ec2-user@docker ashu-docker-images]$ docker rm ashujc1 
ashujc1
[ec2-user@docker ashu-docker-images]$ 

```


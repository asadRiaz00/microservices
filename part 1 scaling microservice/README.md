When you design and build applications at scale, you deal with two significant challenges: scalability and robustness.

You should design your service so that even if it is subject to intermittent heavy loads, it continues to operate reliably.


you're tasked with the challenge of building such application.You're building a store where users can buy their favourite items.You build a microservice to render the web pages and serving the static assets.You also build a backend REST API to process the incoming requests.

.You want the two components to be separated because with the same REST API you could serve the website and mobile apps.You want the two components to be separated because with the same REST API you could serve the website and mobile apps

<h2>About this lab project</h2>

The service has three components: the front-end, the backend, and a message broker.

The front-end is a simple Spring Boot web app with the Thymeleaf templating engine.

The backend is a worker consuming messages from a queue.

And since Spring Boot has excellent integration with JSM, you could use that to send and receive asynchronous messages.

<h2>Why we need kubernate when we already have dockers???</h2>

 when managing applications that could have hundreds of containers working across multiple servers or hosts they are inadequate. So our applications weren’t exactly as amazing as we had thought. we needed something to manage the containers, because they needed to be connected to the outside world for tasks such as load balancing, distribution and scheduling. As our applications started to be used by more and more people, our services weren’t able to support a lot of requests; this is the part of the story where “Kubernetes” comes in.
 Kubernetes is an open source orchestrator developed by Google for deploying containerized applications. It provides developers and DevOps with the software they need to build and deploy distributed, scalable, and reliable systems.


Ok, so maybe you are asking yourself, “How could Kubernetes help me?” Kubernetes helped us with one constant in the life of every application: change. The only applications that do not change are dead ones; as long as an application is alive, new requirements will come in, more code will be shipped, and it will be packaged and deployed. This is the normal lifecycle of all applications, and developers around the world have to take this reality into account when looking for solutions.
<h2>how Kubernetes is structured, </h2>

● The smallest unit is the node. A node is a worker machine, a VM or physical machine, depending on the cluster.

● A group of nodes is a cluster.

● A container image wraps an application and its dependencies.

● One or more containers are wrapped into a higher-level structure called a “pod.”

● Pods are usually managed by one more layer of abstraction: deployment.

● A group of pods working as a load balancer to direct traffic to running containers is called “services.”

● A framework responsible for ensuring that a specific number of pod replicas are scheduled and running at any given time is a “replication controller.”

● The key-value tags (i.e. the names) assigned to identify pods, services, and replication controllers are known as “labels.”


<h1>                                               ***Part 1***                                                   </h1>
in this part we are going to develop a docker image of  simple spring boot  application and then we will install it to  Kubernetes locally (Minikube).
According to official documentation (https://kubernetes.io/docs/setup/minikube/):

“Minikube is a tool that makes it easy to run Kubernetes locally. Minikube runs a single-node Kubernetes cluster inside a VM on your laptop for users looking to try out Kubernetes or develop with it day-to-day.”
 We’ll also need Kubectl, which  is a command line tool that allows us to manage and deploy applications on Kubernetes. It is also important to mention that Minikube works with Virtual Box by default, but if you want to use another VM driver, you can do so.Minikube is an open source tool that was developed to enable developers and system administrators to run a single cluster of Kubernetes on their local machine. Minikube starts a single node kubernetes cluster locally with small resource utilization. This is ideal for development tests and POC purposes,
In a nutshell, Minikube packages and configures a Linux VM, then installs Docker and all Kubernetes components into it.



<h2>Step 1   **Random Number Generator**</h2>

Create a Spring Starter Project for rng service

![https://github.com/fenago/microservices/blob/master/coin/mdimg/1.png](https://github.com/fenago/microservices/blob/master/coin/mdimg/1.png "Logo Title Text 1")
  
![https://github.com/fenago/microservices/blob/master/coin/mdimg/2.png](https://github.com/fenago/microservices/blob/master/coin/mdimg/2.png "Logo Title Text 1")  

Create a new controller for your Spring application:

  
  
  

src/main/java/com/learningvoyage/rngservice/HelloController.java (or wherever your Controller resides)

```

package com.learningvoyage.rngservice;


import org.springframework.web.bind.annotation.RestController;

import java.security.SecureRandom;
import java.util.Base64;

import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@RestController
public class HelloController {
    
   
        @RequestMapping("/rng")
	@ResponseBody
	String home() {
		return "RNG running on rng";
	}
	@RequestMapping("/rng/{howMany}")
	@ResponseBody
	public String getFRandomNumber(
			  @PathVariable("howMany") int howMany) {
		
		SecureRandom rnd = new SecureRandom();
		byte[] token = new byte[howMany];
		rnd.nextBytes(token);
			    return  Base64.getEncoder().encodeToString(token);
			}
	    
}



```

  *  `@RequestMapping` his annotation maps HTTP requests to handler methods of MVC and REST controllers with input coming from request (as you can see "rng/{how many}"   ).

*  `@ResponseBody` annotation tells a controller that the object returned is automatically serialized into JSON and passed back into the _HttpResponse_ object.
  


  

Although it is possible to package this service as a traditional [WAR](https://spring.io/understanding/WAR) file for deployment to an external application server, the simpler approach demonstrated below creates a standalone application. You package everything in a single, executable JAR file, driven by a good old Java `main()` method. Along the way, you use Spring’s support for embedding the [Tomcat](https://spring.io/understanding/Tomcat) servlet container as the HTTP runtime, instead of deploying to an external instance.

  

src/main/java/com/learningvoyage/rngservice/RngApplication.java

```

package com.learningvoyage.rngservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
@Configuration
@EnableAutoConfiguration
@ComponentScan
@SpringBootApplication
public class RngApplication {

	public static void main(String[] args) {
		SpringApplication.run(RngApplication.class, args);
	}
	
	

}


```

  
  

`@SpringBootApplication` is a convenience annotation that adds all of the following:

  
  
  

*  `@Configuration` tags the class as a source of bean definitions for the application context.

*  `@EnableAutoConfiguration` tells Spring Boot to start adding beans based on classpath settings, other beans, and various property settings.



*  `@ComponentScan` tells Spring to look for other components, configurations, and services in the `hello` package, allowing it to find the `HelloController`.

 
 
<h3>**Build an executable JAR**</h3>

  
  

 You can run the application from the command line with Gradle or Maven. Or you can build a single executable JAR file that contains all the necessary dependencies, classes, and resources, and run that. This makes it easy to ship, version, and deploy the service as an application throughout the development lifecycle, across different environments, and so forth.
 

 you can build the JAR file with `./mvn clean package`inside the folder. Then you can run the JAR file:

  
  

```

java -jar target/rng-0.0.1-SNAPSHOT.jar

``````

  

The procedure above will create a runnable JAR (microservice).

  
  

Logging output is displayed. The service should be up and running within a few seconds.

Open browser and go to link  https://localhost:8080/rng

 - you should see the output!   `
RNG running on rng
` 
- Now enter   https://localhost:8080/rng/32 in browser you will see the random number generated by our  rng microservice and response back to browser

![https://github.com/fenago/microservices/blob/master/coin/mdimg/3.png](https://github.com/fenago/microservices/blob/master/coin/mdimg/3.png "Logo Title Text 1") 

<h2>**Create Docker File**</h2>
Now create a simple text file and name it  `Dockerfile` inside your rng project folder  and add following lines



```
#pull base image
FROM openjdk:8-jdk-alpine

#maintainer add your email id here 
MAINTAINER xyz@email.com

#expose port 8080 for ouside world
EXPOSE 8080

#default command to run jar
CMD java -jar /target/rng-0.0.1-SNAPSHOT.jar

#copy hello world to docker image
ADD ./target/rng-0.0.1-SNAPSHOT.jar /target/rng-0.0.1-SNAPSHOT.jar

```
  
  Let’s now build the docker image by typing the following command -
    `
docker build -t rng-docker . 
`

That’s it. You can now see the list of all the docker images on your system using the following command
`
docker image ls
`

### 4. Running the docker image

Once you have a docker image, you can run it using `docker run` command like so -

`
 docker run -p 8080:8080 rng-docker
`
Now your random generator service container is up and running you can see again  by open browser and go to link [https://localhost:8080/rng](https://localhost:8080/rng)







<h3>installing minikube and kubectl on ubuntu</h3>

Step 1: Update system 




Run the following commands to update all system packages to the latest release:
```
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get upgrade
```
Step 2: Install  VirtualBox 

 install VirtualBox using:

```
sudo apt-get install virtualbox
```


Step 3: Download and install minikube

 

First, you will need to download the latest version of Minikube to your system. You can download it from their official websites with the following command:

```
wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
Once the download is completed, copy the downloaded file under /usr/local/bin with the following command:
```
sudo cp minikube-linux-amd64 /usr/local/bin/minikube
```
Next, give execution permission to the minikube with the following command:
```
sudo chmod 755 /usr/local/bin/minikube
```
Next, check the version of Minikube with the following command:
```
minikube version
```
You should see the following output:
```
minikube version: v1.2.0

```
Step 4: Install kubectl on Ubuntu 18.04

We need kubectl which is a command line tool used to deploy and manage applications on Kubernetes
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
Add Kubernetes apt repository:
```
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
Update apt index and install kubectl
```
sudo apt update
sudo apt -y install kubectl
```
Check version:
```
# kubectl version -o json 
{
  "clientVersion": {
    "major": "1",
    "minor": "10",
    "gitVersion": "v1.10.4",
    "gitCommit": "5ca598b4ba5abb89bb773071ce452e33fb66339d",
    "gitTreeState": "clean",
    "buildDate": "2018-06-06T08:13:03Z",
    "goVersion": "go1.9.3",
    "compiler": "gc",
    "platform": "linux/amd64"
  }
}
```
Step 5:  
Start the minikube cluster. This will take a while, expecting the output below. We are using the virtualbox driver as an example. But it should be similar to other drivers.

```
minikube config set vm-driver virtualbox
```
this will configure virtual machine for local instance of minikube 

Step 6:  
After installing Minikube and Kubectl, we should start the Minikube cluster with the following command:

```minikube start```

Minikube created a virtual machine, and inside it, a cluster is now running.

If we want to validate the state of Kubernetes resources in our cluster, we can use Kubernetes Dashboard; the command is `minikube dashboard` A web browser will be opened with the following dashboard:
![https://github.com/fenago/microservices/blob/master/scaling%20microservice/dash.png](https://github.com/fenago/microservices/blob/master/scaling%20microservice/dash.png "Logo Title Text 1")


and then after minikube start verify it by calling 

`minikube dahsboard`

if you get the folling error then  open browser manually and copy paste the link you see in the error


`pic`

in browser you can see dahshbaord as follow

`pic`

<h3>Deploy the app on Kubernetes</h3>
Step 7:  

First, we should check the information of our cluster:

`sudo kubectl cluster-info`

It is important to clarify that Minikube only has one cluster with its respective node. So, we could check our nodes using the command `sudo kubectl get nodes.` The output looks like this:

`pic`



Step 8:  

Minikube comes with its own docker daemon. Run the following so that we can work with that docker daemon on our machine.
for this close the terminal and then open again and run following command it will configure minikube to use docker-environment from inside 
`eval $(minikube docker-env)`

Step 9: 
<h3>create docker image of our spring bott application for kubernete</h3>
now we need to build docker image for minikube. from terminal go to rng folder and build docker image again with same command as you did previously 

 
# Build image
`docker build -t foo:0.0.1 .`
foo is the name of image

Step 10: 

For this post, we only need a master node, but obviously in production mode we would probably have to use at least three nodes: one for the master, and two nodes for all the things related to application redundancy.(we will further  disscuss in  part2 ) 
command for deployment is "sudo kubectl run {DEPLOYMENT_NAME} --image= {YOUR_IMAGE} --port=8080"

The command “kubectl run” only needs the {DEPLOYMENT_NAME} to work, but if you want to pull a Docker image inside this deployment, you should use the --image” option, with which you can specify the Docker image to be used.
`sudo kubectl run hello-fooo --image=foo:0.0.1 --image-pull-policy=Never`

now check if everything is running by executing following command
`sudo kubectl get pods`

you will see the output like 

NAME                         READY   STATUS              RESTARTS   AGE



hello-fooo-bf848b5d9-wlxmm   1/1     Running             0          17m



if status is "Running" everything is working fine else something went wrong during image pulling from docker



Step 11: 

Currently, our application isn’t accessible from outside the cluster; it is only running on the Kubernetes cluster.  We need to create a “bridge” between our application and the outside world, something that can be done by using a service. Let’s go ahead and create our service, because we want everyone to be able to use our application:

`sudo kubectl expose deployment/hello-fooo --type="NodePort" --port 8080`

The logic behind the above command is the following: we want to expose our deployment to the world through the NodePort (which will be assigned when the service is created). After you execute the command, a console message will appear:  “service ‘hello-fooo’ exposed,” which means that the application can be accessed from outside the cluster.

Step 12: 
in addition to obtaining more details about our service, we can use the following command:

`kubectl describe services/hello-fooo`
 you will see output like 
 
 Name:                     hello-fooo
Namespace:                default
Labels:                   run=hello-fooo
Annotations:              <none>
Selector:                 run=hello-fooo
Type:                     NodePort
IP:                       10.100.22.168
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  30360/TCP
Endpoints:                172.17.0.7:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
	
here line `NodePort:   <unset>  30360/TCP` showing that our service can access from port "30360"


Step 13: 
 Remember that the application lives inside a Pod, and we’ve created a service that allows us to access the application from outside the cluster. Use the following command to see which Minikube IP you have:

`minikube ip`
my ip is "192.168.99.100" check your and open the browser and 
In my case, the IP is http://192.168.99.100/ and the NodePort is 30961.


Step 14: 
Go to your favorite browser and type:  http://192.168.99.100:30961/rng.(change your ip to whatever  minikube ip command output is) You can see that the application is running and can be accessed from outside the Kubernetes cluster:

`img`

also you can generate random number here by `http://192.168.99.100:30961/rng/32`

<h3>Conclusion</h3>

We implemented and deployed our application on the Kubernetes cluster; in addition, we used Kubectl to execute commands on the cluster. We should now have a basic understanding of how Kubernetes works, so feel free to create more deployments and communicate with them by using services.

in next part we will create  some advanced cluster with daily life  example

In this session we are going to explain what Kubernetes is, we’re going to:
 - see what the official definition is;
 - ⁠what kubernetes does;
 - ⁠we will look at the problem-solution;
 - ⁠case study of kubernetes (basically why did kubernetes even come around or rose so faster, and what problem it does solve)
 - ⁠we will look at the basic architecture of kubernetes( what are the Master, the slave nodes, and the kubernetes processes that actually make up the platform mechanism));
 - ⁠basic concepts, and the components of kubernetes (which are Pods, containers, services, and what’s the role of each one of those components), and
 - finally we’ll look at a sample configuration that you as a Kubernetes cluster user would use to create those components and configure the cluster to your need.


𝐎𝐟𝐟𝐢𝐜𝐢𝐚𝐥 𝐝𝐞𝐟𝐢𝐧𝐢𝐭𝐢𝐨𝐧 𝐨𝐟 𝐤𝐮𝐛𝐞𝐫𝐧𝐞𝐭𝐞𝐬 
What is 𝐤𝐮𝐛𝐞𝐫𝐧𝐞𝐭𝐞𝐬?
Kubernetes is an open source container orchestration framework (tool). Kubernetes was originally developed by Google. Kubernetes manages containers being Docker or containers from other from other technologies.

This means that kubernetes helps you manage application which are made up of hundred or thousands of containers. It helps manage them in different environments like physical machines, virtual, cloud environment or environment hybrid deployment environment. 

𝐖𝐡𝐚𝐭 𝐩𝐫𝐨𝐛𝐥𝐞𝐦 𝐝𝐨𝐞𝐬 𝐊𝐮𝐛𝐞𝐫𝐧𝐞𝐭𝐞𝐬 𝐬𝐨𝐥𝐯𝐞?
So, what problem does Kubernetes solve?, and what are the tasks of a container orchestration tool actually? 
![image alt](the _prob_k8s_solve)

To go through chronologically:
The need for a container orchestration tool comes as a result of:
 - the rise from 𝐌𝐨𝐧𝐨𝐥𝐢𝐭𝐡 to 𝐦𝐢𝐜𝐫𝐨𝐬𝐞𝐫𝐯𝐢𝐜𝐞𝐬 which caused the increase usage of container technology because containers offer a perfect tools for small, independent applications like microservices. And the rise of containers and microservice technology actually resulted in applications that are now comprise of hundreds, and sometimes maybe thousands of containers. Managing those tons of containers across multiple environments using scripts, and self-made tools can be really complex, and sometimes even impossible. So, that specific scenario actually caused the need for having container orchestration technology. 

𝐖𝐡𝐚𝐭 𝐟𝐞𝐚𝐭𝐮𝐫𝐞𝐬 𝐝𝐨 𝐨𝐫𝐜𝐡𝐞𝐬𝐭𝐫𝐚𝐭𝐢𝐨𝐧 𝐭𝐨𝐨𝐥 𝐨𝐟𝐟𝐞𝐫? 
What those orchestration tools like kubernetes do is that it actually guarantees the following features:
![image alt](featur_of_orc_tools)
 - 𝐇𝐢𝐠𝐡 𝐚𝐯𝐚𝐢𝐥𝐚𝐛𝐢𝐥𝐢𝐭𝐲 or no downtime. In simple word, high availability means the application has no downtime. It’s always accessible by the users. 
 - ⁠𝐒𝐜𝐚𝐥𝐚𝐛𝐢𝐥𝐢𝐭𝐲 or high performance: This  means that application has high performance. It loads fast and users have a very high responsive rate from the application. 
 - ⁠𝐃𝐢𝐬𝐚𝐬𝐭𝐞𝐫 𝐫𝐞𝐜𝐨𝐯𝐞𝐫𝐲 (backup and restore). This basically means that if an infrastructure has some problems like data is lost or the server explode or something bad happens to the server center, the infrastructure has to have some kind of mechanism to backup data and restores it to the latest state so that application actually doesn’t lose any data. And a containerize application can run from the latest state after the recovery. All this are functionalities that container orchestration tool like kubernetes technology offer.

𝐊𝐮𝐛𝐞𝐫𝐧𝐞𝐭𝐞𝐬 𝐁𝐚𝐬𝐢𝐜 𝐀𝐫𝐜𝐡𝐢𝐭𝐞𝐜𝐭𝐮𝐫𝐞 
![image alt](k8s_arch)
How does kubernetes basic architecture actually look like? The kubernetes cluster is made up with at least one 𝐌𝐚𝐬𝐭𝐞𝐫 𝐍𝐨𝐝𝐞, and then connected to it are a couple of 𝐖𝐨𝐫𝐤𝐞𝐫 𝐍𝐨𝐝𝐞𝐬 where each node has at least a 𝐤𝐮𝐛𝐞𝐥𝐞𝐭 process working on it. A kubelet is a kubernetes process that makes it possible for the cluster to talk to each other, communicate to each other and actually execute some tasks on those nodes like running application processes. Each Worker node has docker containers of different application deployed on it. So, depending on how the workload is distributed, you’ll have different number of docker containers running on Worker nodes. And Worker nodes are where the actual work is happening. It’s where the applications run. 

![image alt](master_nodes_comp)
The question is what is runs on Master Node? The Master Node actually runs several kubernetes processes that are absolutely necessary to run and manage cluster properties. Such process are:
 - An 𝐀𝐏𝐈 server which is also a container. An API server is actually the entry point to the kubernetes cluster. So this is the process which different kubernetes clients wil talk to. Like the 𝐔𝐈 if you’re using the kubernetes dashboard, an API if you’re using some scripts and automating technologies, and 𝐊𝐮𝐛𝐞𝐜𝐭𝐥 if you are using a command line tool. All this will to talk to API server. 
 - Another process that runs on Master Node is a 𝐂𝐨𝐧𝐭𝐫𝐨𝐥𝐥𝐞𝐫 𝐦𝐚𝐧𝐚𝐠𝐞𝐫. It basically keeps the overview of what is happening in the cluster whether something needs to be repaired, or maybe if a container died and it needs to be restarted etc.

 - Another one is 𝐬𝐜𝐡𝐞𝐝𝐮𝐥𝐞𝐫. A scheduler is responsible for scheduling containers on different  nodes based on workloads and available server resources on each node. So it’s an intelligent process that decides on which Worker node next container should be scheduled based on the available resources on those Worker nodes and the load that the container needs. 

 - ⁠Another very important component of the whole cluster is actually an 𝐞𝐭𝐜𝐝 key value storage. An etcd holds at anytime the current state of the kubernetes cluster. So, it has all the configuration data inside and all the status data of each node, and each container inside of the node.  The backup and the restore we mentioned previously is actually made from the etcd snapshot. This is because you can recover the whole cluster state using the etcd snapshot. 

- ⁠Last but not the least, also a very important component of kubernetes which enables those nodes(Master nodes, Worker nodes) to talk to each other is the 𝐯𝐢𝐫𝐭𝐮𝐚𝐥 𝐧𝐞𝐭𝐰𝐨𝐫𝐤. It spans all the nodes that are part of the cluster. In a simple word, virtual network actually turns all the nodes inside of a cluster into one powerful *machine* that has the sum of all the resources of individual nodes. One thing to be noted here is that Worker nodes because they actually have much load, because there are running applications inside of it, usually are much bigger and have more resources because there are running hundreds of containers inside of them. Whereas Master Nodes will be running just a handful of Master processes like we’ve seen in the diagram. However, as you can imagine, Master node is much more important than the individual Worker nodes because, for example, if you lose a Master node, you’ll not be able to access the cluster anymore. That means that you  absolutely have to have a backup of your Master at anytime. In production environment usually, would have at least two masters inside a kubernetes cluster. In more cases of course you can have multiple masters where when one Master node is down, the kubernetes cluster continues to function smoothly because you have other masters available.

𝐊𝐮𝐛𝐞𝐫𝐧𝐞𝐭𝐞𝐬 𝐁𝐚𝐬𝐢𝐜 𝐂𝐨𝐧𝐜𝐞𝐩𝐭𝐬
![image alt](pod_container)
What are Pods and containers? In kubernetes a Pod is the smallest unit that you as the kubernetes user will configure and interact with. Pod is basically a repos of a container. On each worker node are found multiple Pods, and inside of a Pod, you can actually find multiple containers. Usually per application, you will have one Pod. The only time you would need more than one container inside of a Pod is when you have a main application that needs some helper containers. So, usually you would have one Pod per application, a database for example would be one Pod, a message breaker would be another Pod, a server would be another Pod, and your Nodejs application for example, or Java application will be on its own Pod. As we mentioned previously, there is a virtual network that spans the kubernetes cluster. So, what that does is that it assigns each Pod its own IP address. So each Pod is its own self-containing server with its own IP address. The Pods communicate with each other by using the internal IP addresses.  
![image alt](ip_addresses)

We don’t actually create or configure containers inside of a kubernetes cluster but we only work with a Pod which is an abstraction layer inside of a container. And Pod is a component of kubernetes that manages the container running inside itself without our intervention. For example, if a container stops or die inside of a Pod, it will be automatically restarted inside of the Pod. However Pods are ephemeral components, which means that Pods can also die very frequently and when a Pod dies, a new one gets created. Here is where the notion of a 𝐬𝐞𝐫𝐯𝐢𝐜𝐞 comes in to play. So, what happens is that whenever a Pod gets restarted, or recreated, a new Pod is created and it gets a new IP address. For example an application that talks to a database Pod does so using an IP address. When the Pod restarts, it gets a new IP address. Obviously it will be very inconvenient re-adjusting the IP address all the time. Because of that, another component of kubernetes called 𝐬𝐞𝐫𝐯𝐢𝐜𝐞 is used.



![image alt](service)
A service is basically an alternative or a substitute to those IP addresses.  So, instead of having these dynamic IP addresses, there are services sitting in front of each Pod that talks to each other. Now if a Pod behind the service dies and gets recreated, the service stays in place because their lifecycles are not tied to each other. A service has two functionalities. One is a permanent IP address which you can use to communicate between the Pods, at the same time, it is a load balancer.

𝐊𝐮𝐛𝐞𝐫𝐧𝐞𝐭𝐞𝐬 𝐂𝐨𝐧𝐠𝐫𝐚𝐭𝐮𝐥𝐚𝐭𝐢𝐨𝐧𝐬 
Now that we’ve seen the basic concepts of kubernetes, how do we actually create those components like Pod and services to configure the kubernetes cluster?
![image alt](config)

All the configurations in a kubernetes cluster actually go through a Master Node with the process called API server which we mentioned briefly earlier. So, kubernetes client which could be a UI (like kubernetes dashboard for example), or an API (which could be a script or automating technology, or curl command), or Kubectl( a command line tool) all talk to API server and they send their configuration request to the API server which is the main or the only entry point into the cluster . The request needs to be either in YML or JSON format.

This is how an example configuration file in yml format actually looks like. So with this, we are sending a request to kubernetes to configure a component called deployment which is basically a template or a blue print for creating Pods. And in this specific configuration example, we are telling kubernetes to create two replicas Pods for us called "*My-App*" with each Pod replica having a container based on “*my-image*” running inside. In addition to that, we configure what the environment variables and the *Port* configuration of this container inside the Pod should be. As we can see, the configuration requests in kubernetes are declarative format. So, we declare what is our desire outcome from kubernetes, and ubernetes tries to meet those requirements. Meaning, for example, since we want two replica Pods of “*My-App*” deployment to be running in the cluster and if one of those pods dies, the controller manager will see that “*there is*” and “*should*” state are different. The actual state is one Pod and our desired state is two. So, it goes to work to make sure that the desire state is recovered, automatically restarting the second replica of pod. This is how kubernetes configuration works with all of its components being the Pods, the services, or deployment, what have you.
𝐊𝐔𝐁𝐄𝐍𝐄𝐓𝐄𝐒 𝐀𝐑𝐂𝐇𝐈𝐓𝐄𝐂𝐓𝐔𝐑𝐄

In this tutorial, we'll be talking about basic architecture of kubernetes.
kubernetes is a complete framework which is very powerful but at the same time very complex and a lot of people when they read the documentation of kubernetes they actually get overwhelmed because they read the description of how kubernetes mechanism is built and how all the processes inside of that mechanism actually work to make up the cluster and make it possible to manage and orchestrate the containers

We going look at two types of Nodes that kubernetes operates on. One is Master in another one is slave. We are going t see what is the difference between those and which role each one of them has inside of the cluster. We will also go through the basic concepts of how kubernetes does what it does and how the cluster is self-managed and self-healing and automated and how you as a operator of the kubernetes cluster should end up having much less manual effort.

𝐍𝐨𝐝𝐞 𝐏𝐫𝐨𝐜𝐞𝐬𝐬𝐞𝐬
 - Worker Machine in Kubernetes cluster

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/632ed19fe23a91b5aa831ef5dc6821d8fcb86e41/03%20K8S%20Architecture/workernode.jpeg" width="400" height="250"/>
</p>
Let's with the basic setup of one Node with two application Pod running on it. One of the main components of kubernetes architecture are its worker servers or Nodes and each Node will have multiple application Pod with containers running on that Node. The way kunernetes does it is using three processes that must be installed on every Node that are used to schedule and manage those Pods. Nodes are the cluster servers that actually do the work that's why sometimes there are also called worker Nodes. The first process that needs to run on every Node is the container runtime in our example, have docker but it could be some other technology. 

The fact that application Pods have containers running inside a container runtime needs to be installed on every Node. The process that actually schedules those can those Pod and the containers then underneath is cubelet. Cubelet which is a process of kubernetes itself unlike container runtime that has interface with both container runtime and the machine(the Node itself). This is because at the end of the day cubelet is responsible for taking that configuration and actually running a Pod or starting a Pod with a container inside and then assigning resources from that Node to the container like CPU RAM and storage resources. Usually kubernetes cluster is made up of multiple Nodes which also must have container runtime and cubelet services installed. You can have hundreds of those worker Nodes which will run other Pods and containers and replicas of the existing Pods like the application and database Pod in this example. The way that communication between them works is using services which is sort of a load balancer that basically caches the requests directed to the Pod or the application like database for example and then forwards it to the respective Pod.

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/632ed19fe23a91b5aa831ef5dc6821d8fcb86e41/03%20K8S%20Architecture/kubelet.jpeg" width="400" height="250"/>
</p>
The third process that is responsible for forwarding requests from services to Pod is actually Kube proxy that also must be installed on every Node. Kube proxy has actually intelligent forwarding logic inside that makes sure that the communication also works in a performant way with low overhead. For example, if an application "my app" replica is making a request database instead of service just, randomly forwarding the request to any replica it will actually forward it to the replica that is running on the same Node as the Pod that initiated the request. Thusvavoiding the network overhead of sending the request to another machine.

To summarize, to kubernetes processes Kubelet and queue proxy must be installed on every kubernetes worker Node along with an independent container runtime in order for kubernetes cluster to function properly. The question is how do you interact with this cluster or do you decide on which Node a new application Pod or database Pod should be scheduled or if replica Pod dies what process actually monitors it and then reschedules it or restarts it again?  Or when we add another server how does he join the cluster to become another Node and get Pods and other components created on it? The answer is all these managing processes are done by Master Nodes.

𝐌𝐚𝐬𝐭𝐞𝐫 𝐍𝐨𝐝𝐞

Master servers or Master Nodes have completely different processes running inside and there are four processes that run on every Master Node that control the cluster state and the worker Nodes.

The first service is 𝐀𝐏𝐈 

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/632ed19fe23a91b5aa831ef5dc6821d8fcb86e41/03%20K8S%20Architecture/api.jpeg" width="400" height="250"/>
</p>
- 𝐀𝐏𝐈 server: When you as a user wants to deploy a new application in a kubernetes cluster you interact with the API server using some client. It could be a UI like kubernetes dashboard, it could be command line tool like Kubelet or a kubernetes this API. API server is like a cluster gateway which gets the initial requests of any updates into the cluster or even the queries from the cluster. It also acts as a gatekeeper for authentication to make sure that only authenticated and authorized requests get through to the cluster. This means whenever you want to schedule new Pods, deploy new applications, create new service or any other components, you have to talk to the API server on the Master Node. The API server then validate your request and if everything is fine, then it will forward your request to other processes in order to schedule the Pod or create this component that you requested. Also if you want to query the status of your deployment or the cluster health etc, you make a request of the API server and it gives you the response which is good for security because you just have one entry point into the cluster.

Another Master process is 𝐒𝐜𝐡𝐞𝐝𝐮𝐥𝐞𝐫

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/632ed19fe23a91b5aa831ef5dc6821d8fcb86e41/03%20K8S%20Architecture/scheduler.jpeg" width="400" height="250"/>
</p>
- 𝐒𝐜𝐡𝐞𝐝𝐮𝐥𝐞𝐫: As earliar mentioned, if you send an API server a request to schedule a new Pod, the API server after validating your request will actually hand it over to the scheduler in order to start the application Pod on one of the worker Nodes and of course instead of just randomly assigning to any Node,  scheduler has this whole intelligent way of deciding on which specific worker Node the next Pod will be scheduled or next component will be scheduled.

Firstly it will look at your request and see how much resources the application that you want to schedule will need. How much CPU, how much RAM, and then it's going to look at, go through the worker Nodes and see the available resources on each one of them and if it sees that one Node is the least busy or has the most resources available, it will schedule the new Pod on that Node. An important point to note here is that scheduler just decides on which Node a new Pod will be scheduled. The process that actually does the scheduling that actually starts that Pod with a container is the Kubelet. It gets the request from the scheduler and execute the request on that Node. For example, new Pods could be deployed on the Node which is 30% busy. 

The next Master process is 𝐂𝐨𝐧𝐭𝐫𝐨𝐥𝐥𝐞𝐫 𝐌𝐚𝐧𝐚𝐠𝐞𝐫
- 𝐂𝐨𝐧𝐭𝐫𝐨𝐥𝐥𝐞𝐫 𝐌𝐚𝐧𝐚𝐠𝐞𝐫: Controller Manager which is another crucial component because what happens when Pods die on any Node, there must be a way to detect that a Pod died and then reschedule those Pods as soon as possible. 

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/632ed19fe23a91b5aa831ef5dc6821d8fcb86e41/03%20K8S%20Architecture/controllermanager.jpeg" width="400" height="250"/>
</p>
What controller manager does is that it detects state changes like crashing of Pods, for example, when Pods die controller manager detects it and tries to recover the cluster state as soon as possible. For that reason, it makes a request to the scheduler to reschedule those dead Pods. In the same cycle, the scheduler decides based on the resource calculation which worker Nodes should restart those Pod again and makes requests so the corresponding Kubelets on t
hose worker Nodes to actually restart the Pods.

The Last Master process is 𝐞𝐭𝐜𝐝

- 𝐞𝐭𝐜𝐝: etcd  is a key value store of a cluster state. You can think of it as the cluster brain actually which means that every change in the cluster for example, when a new Pod gets scheduled, when a Pod dies all of these changes get saved or updated into this key value store of etcd. The reason why etcd store is a cluster brain is because all of these mechanism such as scheduler, controller manager, etc, work because of its data. For instance how does scheduler know what resources are available on each worker Node? How does controller manager know that a cluster state changed in some way? For example, How does controller manager know that a Pod died or that Kubelet restarted new Pod upon the request of a scheduler? When you make a query request to API server about the cluster health, for example, your application deployment state where does API server get all this state information from? All of this information is stored in etcd cluster. 

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/632ed19fe23a91b5aa831ef5dc6821d8fcb86e41/03%20K8S%20Architecture/etcd.jpeg" width="400" height="250"/>
</p>
What is not stored in the etcd key value store is the actual application data. For example, if you have a database application running inside of the cluster the data will be stored somewhere else, not in the etcd. etcd is just a cluster state information which is used for Master processes to communicate with the work processes and vice versa. So, now you probably already see that Master processes are absolutely crucial for the cluster operation, especially the etcd store which contains some data must be reliably stored or replicated. In n practice kubernetes cluster is usually made up of multiple Masters where each Master Node runs its Master processes. The Master Node is where the API server is load balanced and the etcd store forms a distributed storage across all the Master Nodes. 

Example of a Cluster Set-Up

Now that we've seen what processes run on worker Nodes and Master Nodes let's actually have a look at a realistic example of a cluster setup. So, in a very small cluster you'd probably have two Master Nodes and three worker Nodes. Also to note here, the hardware resources of Master and Node servers actually differ. Master processes are more important but they actually have less load of work so they need less resources like CPU, RAM, and storage. 

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/632ed19fe23a91b5aa831ef5dc6821d8fcb86e41/03%20K8S%20Architecture/clustersetup.jpeg" width="400" height="250"/>
</p>
Whereas the worker Nodes do the actual job of running those jobs with containers inside therefore they need more resources. AS your application complexity and its demand of resources increases, you may actually add more Master and Node servers to your cluster and thus forming a more powerful and robust cluster to meet your application resource requirements. In an existing kubernetes cluster you can actually add new Master or Node servers pretty easily. If you want to add a Master server you just get a new bare server, install all the Master processes on it, and add to the kubernetes cluster. Same way if you need two worker Nodes you get a bare servers, you install all the worker Node processes like container runtime, Kubelet and Kube proxy on it and add it to the Kubernetes is cluster. This way you can infinitely increase the power and resources of your kubernetes cluster, its replication complexity, and its resource demand increases.
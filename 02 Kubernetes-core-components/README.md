𝐊𝐔𝐁𝐄𝐑𝐍𝐄𝐓𝐄𝐒 𝐂𝐎𝐑𝐄 𝐂𝐎𝐌𝐏𝐎𝐍𝐄𝐍𝐓𝐒

Welcome to the next kubernetes tutorial. So, in this session, I will to give you an overview of the most basic components of kubernetes which are just enough to actually get you started using kubernetes in practice, either as a DevOps engineer or a software developer. kubernetes has tons of components but most of the time you are going to be working with just a handful of them. So, if you have you want to get started with kubernetes, then this writeup is actually a perfect fit for you.

𝐊𝐮𝐛𝐞𝐫𝐧𝐞𝐭𝐞𝐬 𝐂𝐨𝐦𝐩𝐨𝐧𝐞𝐧𝐭𝐬
We are going to use the case of a simple JavaScript application with a simple database and we are going to give you step by step description how each component of kubernetes actually helps you to deploy your application and what is the role of each of those components.

𝐍𝐨𝐝𝐞 𝐚𝐧𝐝 𝐏𝐨𝐝
A Worker Node or in kubernetes terms, a Node is a simple server which can be a physical or virtual machine. While a Pod is the basic component or the smallest unit of kubernetes. So, a Pod is basically an abstraction over a container. If you're familiar with docker containers or container images, what Pod does is that it creates this running environment or a layer on top of the container and the reason is because kubernetes wants to abstract away the container runtime or container technologies so that you can replace them if you want to and also because you don't have to directly work with docker or whatever the container technology you use in kubernetes. You only interact with the kubernetes layer. We have an application Pod which is our own application and that will maybe use a database Pod with its own container and this is also an important concept here.

𝐏𝐨𝐝
Pod is usually meant to run one application container inside of it. You can run multiple containers inside one Pod but usually it's only the case if you have one main application container and a helper container or some side service that has to run inside of that Pod. You can see this is nothing special you just have one server and two containers running on it with abstraction layer on top of it. 

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/404a4880eee71012d74ca1ccd61d242c764f1a1a/02%20Kubernetes-core-components/Pod.jpeg" width="400" height="250"/>
</p>
Let's see how they communicate with each other in kubernetes world. kubernetes offers out-of-the-box a virtual network which means that each Pod gets its own IP address not the container. The Pod gets the IP address and each Pod can communicate with each other using that IP address which is an internal IP address obviously it's not the public one so my application container can communicate with database using the IP address. However Pod components in kubernetes, also an important concept are ephemeral which means that they can die very easily. And when that happens for example if I lose a database container because the container crashed, because the application crashed inside or because the Node, or the server that we are running them on ran out of resources, the Pod will die and a new one will get created in its place. When that happens, it will get assigned a new IP address which obviously is inconvenient if you are communicating with the database using the IP address because you have to adjust it every time Pod restarts, and because of that another component of kubernetes called service is used.

𝐒𝐞𝐫𝐯𝐢𝐜𝐞 𝐚𝐧𝐝 𝐈𝐧𝐠𝐫𝐞𝐬𝐬

 - 𝐒𝐞𝐫𝐯𝐢𝐜𝐞
A service is basically a static IP address or permanent IP address that can be attached to each Pod. So, the application Pod will have its own service and database Pod will have its own service. The good thing here is that the life cycles of service and the Pod are not connected. Even if the Pod dies the service and its IP address will stay. You don't have to change that endpoints anymore. Obviously you would want your application to be accessible through a browser and for this happen you would have to create an external service.

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/404a4880eee71012d74ca1ccd61d242c764f1a1a/02%20Kubernetes-core-components/service.jpeg" width="400" height="250"/>
</p>
External service is a service that opens the communication from external sources. Obviously you wouldn't want your database to be open to the public requests and for that reason you would create something called an internal service. Internal service is a type of a service that you specify when creating one. However if you notice the URL of the external service is not very practical. What you have is an HTTP protocol with a Node IP address of the Node but not the service and the Port number of the service(e.g http:///172.201.3:8080). This good for test purposes if you want to test something very fast but not for the end product. Usually you will want your URL to look like this(eg. https://my-app.com) if you want to talk to your application with a secure protocol and a domain name and for that there is another component of kubernetes called ingress.

 - 𝐈𝐧𝐠𝐫𝐞𝐬𝐬 
So, instead of service, the request goes first to ingress and it does the forwarding to the service. Now you've seen some of the very basic components of kubernetes. This was a very simple set up we just have one server and a couple of containers running and some services.

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/404a4880eee71012d74ca1ccd61d242c764f1a1a/02%20Kubernetes-core-components/ingress.jpeg" width="400" height="250"/>
</p>
As we, said Pods communicate with each other using a service. An application will have a database endpoint let's say called "mongo-db-service" that it uses to communicate with the database. But where do you configure usually this database URL or endpoints? Usually you will do it in application properties file or as some kind of external environmental variable but usually it's inside of the built image of the application. 

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/404a4880eee71012d74ca1ccd61d242c764f1a1a/02%20Kubernetes-core-components/mongo-db-service.jpeg" width="400" height="250"/>
</p>
For instance, if the endpoint of the service or service name in this case changed to mongo-db, you will have to adjust that URL in the application. Usually you'd have to rebuild the application with a new version and you have to push it to the repository, and now you'll have to pull that new image in your Pod and restart the whole thing. 

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/404a4880eee71012d74ca1ccd61d242c764f1a1a/02%20Kubernetes-core-components/mongo-service.jpeg" width="400" height="250"/>
</p>
This is a little bit tedious for a small change like database URL. For that purpose kubernetes has a component called "configmap".

 - 𝐂𝐨𝐧𝐟𝐢𝐠𝐌𝐚𝐩
What ConfigMap does is it's basically your external configuration to your application. ConfigMap will usually contain configuration data like URLs of database, or some other services that you used. In kubernetes you just connect it to the Pod so that Pod actually gets the data that ConfigMap contains. Now, if you change the name of the service, the endpoint of the service you just adjust the ConfigMap and that's it. 

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/404a4880eee71012d74ca1ccd61d242c764f1a1a/02%20Kubernetes-core-components/configmap.jpeg" width="400" height="250"/>
</p>
You don't have to build a new image, you don't have to go through this whole cycle. Part of the external configuration can also be database username and password which may also change in the application deployment process. Putting the password or other credentials in a ConfigMap in a plain text format would be insecure even though it's an external configuration so for this purpose kubernetes has another component called secret.

 - 𝐒𝐞𝐜𝐫𝐞𝐭
Secret is just like ConfigMap but the difference is that it's used to store secret data credentials for example, and it's stored not in plain text format but in base64 encoded format. Secret will contain things like credentials. Database username could also be put in ConfigMap but what's important is the passwords, certificates, things that you don't want other people to have access to will go in the secret. 

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/bc37b7f289c017ccfd5333865b71f8aa661422c7/02%20Kubernetes-core-components/secret.jpeg" width="400" height="250"/>
</p>
Just like ConfigMap you just connect it to your Pod so that Pod can actually see those data and read from the secret you can actually use the data from ConfigMap or secret inside of your application Pod by using environmental variables.

We've actually looked at almost all mostly used kubernetes basic components. We've looked at the Pod, we've seen how services are used, what is ingress component useful for, and we've also seen external configuration using ConfigMap and secrets.

𝐃𝐚𝐭𝐚 𝐒𝐭𝐨𝐫𝐚𝐠𝐞
Let's see another very important concept generally which is data storage and how it works in kubernetes. We have this database Pod that our application uses and it has some data or it generates some data. Based on the current set up if the database container or the Pod gets restarted the data would be gone and that's problematic and inconvenient. Obviously will want your database data or log data to be persisted reliably long-term. The way you can do it in kubernetes is using another component of kubernetes called volumes.

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/404a4880eee71012d74ca1ccd61d242c764f1a1a/02%20Kubernetes-core-components/volume.jpeg" width="400" height="250"/>
</p>
How does volume work? How it works is that it basically attaches a physical storage on a hard drive to your Pod and that storage could be either on a local machine meaning on the same server Node where the Pod is running or it could be on a remote storage meaning outside of the kubernetes cluster. It could be a cloud storage or it could be your on-premise storage which is not part of the kubernetes cluster. You just have an external reference on it. When the database Pod or container gets restarted all the data will be there/persisted.

it's important to understand the distinction between the kubernetes cluster and all of its components and the storage regardless of whether it's a local or remote storage. Think of a storage as an external hard drive plug in into the kubernetes cluster because the point is Kubernetes cluster explicitly doesn't manage any data persistence which means that you as a community's user or an administrator are responsible for backing up the data, replicating and managing it, and making sure that it's kept on a proper Hardware etc.

𝐃𝐞𝐩𝐥𝐨𝐲𝐦𝐞𝐧𝐭 𝐚𝐧𝐝 𝐒𝐭𝐚𝐭𝐞𝐟𝐮𝐥𝐒𝐞𝐭  

Let's see everything is running perfectly and a user can access our application through a browser. With our set up what happens if your application Pod dies/crushes or I have to restart the Pod because I built a new container image? I would have a downtime where a user can't reach your application which is obviously a very bad thing if it happens in production. This is exactly the advantage of distributed systems and containers so instead of relying on just one application Pod and one database Pod etc, we are replicating everything on multiple servers. We will have another Node where a replica or clone of our application would run which will also be connected to the service so remember previously we said the service is like a persistent static IP address with a DNS name so that you don't have to constantly adjust the end point when Pod dies. The service is also a load balancer which means that the service will actually cache the request and forward it to whichever part is least busy. So it has both of these functionalities(static Ip, and Load balancer). In order to create the the second replica of the an application Pod you wouldn't create a second Pod but instead you would define a blueprint for in an application Pod and specify how many replicas of that Pod you would like to run and that component or that blueprint is called deployment, and which is another component of kubernetes. In practice you would not be working with Pods or you will not be creating Pods, instead, you will be creating deployments because there you can specify how many replicas and you can also scale up or scale the down number of replicas of Pods that you need. With Pod we said that Pod is a layer of abstraction on top of containers and deployment is another abstraction on top of Pods which makes it more convenient to interact with the Pods, replicate them and do some other configuration. In practice you will mostly work with deployments and not with Pods. If one of the replicas of your replication Pod die, the service will forward the requests to another one so your application would still be accessible for the user.

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/404a4880eee71012d74ca1ccd61d242c764f1a1a/02%20Kubernetes-core-components/staefullset.jpeg" width="400" height="250"/>
</p>
You may probably be wondering what about the database Pod because if the database Pod dies your application also wouldn't be accessible so we need database replicas as well. However we can't replicate database using a deployment and the reason for that is because database has a state which is its data. This means that if we have clones of replicas of the database they would all need to access the same shared data storage and there you would need some kind of mechanism that manages which parts are currently writing to that storage or which parts are reading from that storage in order to avoid data inconsistencies. That mechanism in addition to replicating feature is offered by another kubernetes component called StatefulSet. 

𝐒𝐭𝐚𝐭𝐞𝐟𝐮𝐥𝐒𝐞𝐭

StatefulSet is kubernetes component is meant specifically for applications like databases. MYSQL MongoDB, elasticsearch or any other statefull applications or databases should be created using StatefulSets and not deployments. This is a very important distinction. StatefulSet just like deployment will take care of replicating the Pods and scaling them up or scaling them down but making sure that database `reads` and `writes` are synchronized so that no database inconsistencies are offered. However, I must mention here that deploying database applications using StatefulSets in kubernetes cluster can be somewhat tedious. So, it's definitely more difficult than working with deployments where you don't have all these challenges. That's why it's also a common practice to host database applications outside of the kubernetes cluster and just have the deployments or stateless applications that replicate and scale with no problem inside of the kubernetes cluster and communicate with the external database.

<p align="center">
  <img src="https://github.com/wisdom2608/Learn-Kubernetes/blob/404a4880eee71012d74ca1ccd61d242c764f1a1a/02%20Kubernetes-core-components/crashed.jpeg" width="400" height="250"/>
</p>
Now that we have two replicas of our application Pod, and two replicas of the database and they're both load balanced. Our setup is more robust which means that even if Node 1,  the whole Node server was actually rebooted or crashed and nothing could run on it, we will still have a second Node with application and database Pods running on it and the application would still be accessible by the user until these two replicas get recreated so you can avoid downtime.

To summarize, we have looked at the most used kubernetes components. We started with the Pods and the services in order to communicate between the Pods and the ingress component which is used to route traffic into the cluster. We've also looked at external configuration using ConfigMaps and secrets and data persistence using volumes. And finally we've looked at Pod blueprints with replicating mechanisms like deployments and StatefulSets where StatefulSet is used specifically for stateful applications like databases. Of course there are a lot more components that communities offers but these are really the core, the basic ones just using these core components you can actually build pretty powerful kubernetes clusters.


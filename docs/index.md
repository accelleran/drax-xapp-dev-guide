<table>
  <tr>
   <td>
<h1><strong>dRAX RIC xApps Development Guide</strong></h1>


   </td>
  </tr>
  <tr>
   <td>

<table>
  <tr>
   <td>Doc Ref
   </td>
   <td>dRAX-xApps-01
   </td>
  </tr>
  <tr>
   <td>Version
   </td>
   <td>0.4.0
   </td>
  </tr>
  <tr>
   <td>xApp Framework Version
   </td>
   <td>drax-3.0.0
   </td>
  </tr>
  <tr>
   <td>dRAX Version Support
   </td>
   <td>3.0.0
   </td>
  </tr>
  <tr>
   <td>Date
   </td>
   <td>14.04.2021.
   </td>
  </tr>
</table>


   </td>
  </tr>
  <tr>
   <td>
   </td>
  </tr>
</table>




**About This Document**

This document is intended as a guide for developers creating xApps for the dRAX platform. 

**Copyright Notice **

Accelleran copyrights this dRAX xApps Development Guide. No part of this document may be reproduced in any form or means, without the prior written consent of Accelleran. 

**Disclaimer **

This specification is preliminary and is subject to change at any time without notice. Accelleran assumes no responsibility for any errors contained herein. For more information, please consult our customer support. 

**Contact Us** 

Accelleran NV, Quellinstraat 49, 2018, Antwerpen, Belgium, EU

E-mail: customer.support@accelleran.com

Phone: +32 489 602 684

Website: http://www.accelleran.com/ 




# 1. Version history


<table>
  <tr>
   <td><strong>Author</strong>
   </td>
   <td><strong>Note</strong>
   </td>
   <td><strong>Date</strong>
   </td>
   <td><strong>Version</strong>
   </td>
  </tr>
  <tr>
   <td>Ensar Zeljkovic
   </td>
   <td>First draft with table of contents
   </td>
   <td>06.05.2020.
   </td>
   <td>0.0.1
   </td>
  </tr>
  <tr>
   <td>Ensar Zeljkovic
   </td>
   <td>Restructuring to new xApp Framework Package
   </td>
   <td>24.09.2020.
   </td>
   <td>0.0.2
   </td>
  </tr>
  <tr>
   <td>Ensar Zeljkovic
   </td>
   <td>Pre-Release ready for review
   </td>
   <td>07.10.2020.
   </td>
   <td>0.1.0
   </td>
  </tr>
  <tr>
   <td>Antoine Dugast
   </td>
   <td>Adding Extendable API overview
   </td>
   <td>25.03.2021.
   </td>
   <td>0.2.0
   </td>
  </tr>
  <tr>
   <td>Antoine Dugast
   </td>
   <td>Adding README documentation
   </td>
   <td>09.04.2021.
   </td>
   <td>0.3.0
   </td>
  </tr>
  <tr>
   <td>Ensar Zeljkovic
   </td>
   <td>Incorporating API and README documentation
   </td>
   <td>13.04.2021.
   </td>
   <td>0.3.1
   </td>
  </tr>
  <tr>
   <td>Ensar Zeljkovic
   </td>
   <td>New Development workflow
   </td>
   <td>14.04.2021
   </td>
   <td>0.4.0
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
</table>





# 2. Table of Contents


[TOC]





# 3. Introduction

dRAX is Accelleran’s O-RAN aligned Cloud Native RAN platform. dRAX is cloud native by design and encompasses the O-RAN defined CU function as well as a near real-time RAN Intelligent Controller (RIC) platform. It features a micro-service oriented architecture with containerized functions orchestrated via Kubernetes and supports both 4G and 5G technologies.

 

The dRAX RIC platform is a Near Real-Time platform. It allows developers to implement xApps that can leverage dRAX near real-time open RAN data and communicate with other xApps  to bring new services and functionalities. It offers seamless integration with other orchestration platforms and lowers the technology barrier to implement Enterprise cellular networks by implementing artificial intelligence techniques for the configuration and operation of private cellular networks. This document serves as the developers guide in creating and deploying xApps on the Accelleran dRAX. 

To make the development and integration of xApps into dRAX as smooth as possible, we have created the dRAX xApps Framework. This framework consists of a number of predefined and pre-developed resources that can communicate with the rest of the entities within the dRAX RIC. This will allow the developers to only focus on the functionality of their xApp, making the integration into dRAX seamless. The dRAX xApp Framework is currently written in Python which the developers can use immediately. The framework can also be written in another language of the developers choice, but requires recreating some elements. 

The dRAX xApps Framework consists of the following components, which are shown on _Figure 2_:



1. The xApp REST API
2. The xApp Database 
3. The xApp Core

<table>
  <tr>
   <td><em>Figure 2: dRAX xApp Framework</em>
<p>
The xApp REST API is used for communication with the dRAX RIC. It is a prebuilt block by Accelleran, so that the developer does not have to worry about it. This REST API can be used to configure the xApp. This configuration is then saved in the xApp’s local database. The database is also already prebuilt, in particular we use the Redis database. Finally, the main part of the xApp Framework is the xApp Core. Even this block is almost entirely prebuilt, with helper functions that abstract the different possibilities within the dRAX RIC. The developer can just integrate their code within the xApp Core code and make use of the already existing code.
   </td>
  </tr>
</table>





# 4. Quick guide


## 4.1. Fork or download the xApps Framework Package

We have prepared the xApps Framework Package which contains an xApp template which a developer can use. The package is located on GitHub at:

[https://github.com/accelleran/xapp-framework-package.git](https://github.com/accelleran/xapp-framework-package.git) 

First, fork this GitHub or download its content.


## 4.2. Build the development environment Docker image of the xApp

After downloading the xApp Framework Package, go to the _/xapp-core_ folder. Run the Docker build command to build the development environment for the xApp:

sudo docker build &lt;xapp-name>:&lt;xapp-version> .

where &lt;xapp-name> is the name of your xapp, and &lt;xapp-version> is the version tag. For example, if the name of the xapp is "myxapp-dev" and the version is 0.1.0:

sudo docker build myxapp-dev:0.1.0 .


## 4.3. Prepare Helm chart

In the other folder, _/helm_chart/xapp_, you can find the xApp Helm chart. Here, edit the values.yaml file to specify the details of your xApp. First change the global.kubeIp field to the IP address of your Kubernetes. Next, edit the details of the development Docker image of the xApp Core that were given in the previous step.

…

global:

  kubeIp: "&lt;kube-ip>"

...

# Image settings for the xApp Core

image:

	repository: &lt;xapp-name>

	pullPolicy: IfNotPresent

	tag: "&lt;xapp-version>"

…


## 4.4. Deploy the xApp development environment

We can now deploy the xApp development environment. This is done using helm and by setting the developerMode:

helm install &lt;dev-env-name> . --set developerMode.enabled=true --set developerMode.hostPath=/path/to/xapp-framework-package/xapp_core/

Note, you can give the deployment any generic name you want by changing the &lt;dev-env-name>. The developerMode.hostPath value has to be set to the absolute path to the xApp Core folder of the xApp Framework Package. 


## 4.5. Accessing the development environment

You can now access the xApp development environment by going into the container in Kubernetes. When the xApp is deployed you will have 3 pods in Kubernetes:



* &lt;dev-env-name>
* &lt;dev-env-name>-xapp-redis
* &lt;dev-env-name>-xapp-api

 

The xApp Core is located under the &lt;dev-env-name> pod, so to access the development environment you can:

kubectl exec -it &lt;dev-env-name> -- /bin/bash


## 4.6. Start the xApp in the development environment

Finally you can start your xApp by issuing the following command inside the container:

python3 xapp_main.py


## 4.7. How to uninstall the xApp

You can simple delete the xApp development environment by using the helm delete command:

helm delete &lt;dev-env-name>




# 5. Workflows


## 5.1. Development workflow

When first developing the xApp for dRAX, one can follow this Development workflow for quick development and fixes. The idea of the development workflow is to create an xApp development environment in a Kubernetes pod and mount the xApp Core code inside that pod. This allows for a number of advantages:



* You can start and stop the xApp manually, allowing for quick development and debugging
* Since the xApp Core files are mounted inside the pod, you can edit them either inside the pod or outside of the pod. This allows developers to use advanced code editors on the files outside the container. The changes to those files will be visible inside the development environment since the files are mounted. 
1. **Build xApp development environment Docker image**

Similarly as the quick guide, download the xApp Framework package and build the Docker image from the /xapp-core folder. Give your xApp development environment Docker image and name &lt;xapp-name> and version &lt;xapp-version>:

sudo docker build &lt;xapp-name>:&lt;xapp-version> /path/to/xapp-core



2. **Preparing the values.yaml of the Helm chart for the xApp development environment**

While in the phase of developing an xApp, we have created a development environment which a developer can use for quick development and testing. The development environment is deployed using Helm.

We first have to edit the values.yaml file of the Helm chart. The file is located in the xApp Framework Package under the helm-chart/xapp/ folder. 

First change the global.kubeIp field to the IP address of your Kubernetes. Next, edit the details of the development Docker image of the xApp Core that were given in the previous step.

…

global:

  kubeIp: "&lt;kube-ip>"

...

# Image settings for the xApp Core

image:

	repository: &lt;xapp-name>

	pullPolicy: IfNotPresent

	tag: "&lt;xapp-version>"

…



3. **Deploying the xApp development environment**

We can now deploy the xApp development environment. This is done using helm and by setting the developerMode:

helm install &lt;dev-env-name> . --set developerMode.enabled=true --set developerMode.hostPath=/path/to/xapp-framework-package/xapp_core/

Note, you can give the deployment any generic name you want by changing the &lt;dev-env-name>. The developerMode.hostPath value has to be set to the absolute path to the xApp Core folder of the xApp Framework Package. This path is used to mount the files of the xApp Core inside the xApp development environment.



4. **Accessing the development environment**

You can now access the xApp development environment by going into the container in Kubernetes. When the xApp is deployed you will have 3 pods in Kubernetes:



* &lt;dev-env-name>
* &lt;dev-env-name>-xapp-redis
* &lt;dev-env-name>-xapp-api

 

The xApp Core is located under the &lt;dev-env-name> pod, so to access the development environment you can:

kubectl exec -it &lt;dev-env-name> -- /bin/bash



5. **Start the xApp in the development environment**

You can start your xApp by issuing the following command inside the container:

python3 xapp_main.py



6. **Developing stage**

From this point on, the developers can either edit the xApp Core files inside of the xApp Development Environment, or can take advantage of the fact that the xApp Core files are mounted inside of it, so they can be edited even outside the environment. So if you edit the files in the xapp-framework-package/xapp-core folder, the changes will take effect in the xApp Development environment as well. This allows developers to edit the code in a code editor as well. 

The developer can stop the xApp inside the development environment, edit the xApp Core code (inside the development environment or outside) and restart the xApp again in the development environment to see the changes taking effect. The developer also has access to the logs of the xApp right in the development environment for quick debugging.


## 5.2. Production workflow

When the developers are happy with a version of the xApp, they can prepare it for production mode. Here we explain the details.



1. **Build the Docker image**

Same as before, build you Docker image with an &lt;xapp-name> and &lt;xapp-version>:

sudo docker build &lt;xapp-name>:&lt;xapp-version> .



2. **Push to Docker repository**

Push your xApp Docker image to a docker image repository of your choice. 



3. **Edit Helm Chart**

Edit the values.yaml file in the /helm_chart/xapp folder to specify the remote Docker image repository:

…

# Image settings for the xApp Core

image:

	repository: dockerRepoUrl/&lt;xapp-name>

	pullPolicy: IfNotPresent

...

Also don’t forget to set the xappFrameworkConfig.flushOnDeployment field back to ‘false’.

Edit the Helm chart Chart.yaml to specify the version:

…

version: &lt;xapp-version>

	…

appVersion: &lt;xapp-version>

	...



4. **Package the Helm chart**

Package the Helm chart in a .tgz file using:

helm package /helm_chart/xapp

This will create the .tgz xApp Helm chart package, which will also have the version of the xApp.



5. **Use xApp**

You can now give this xApp Helm chart packaged .tgz file to a user, that can deploy it via the dRAX dashboard. 


# 6. xApp Framework In-depth

As mentioned, the dRAX xApps Framework consists of the following components, which are also shown on _Figure 2_:



1. The xApp REST API
2. The xApp Database 
3. The xApp Core


## 6.1. xApp REST API

The xApp REST API is part of every dRAX xApp. It has been developed using Flask, NGINX and uWSGI. This API is used for communication between the xApp and the dRAX RIC platform. It has three significant functionalities:



1. To enable the dRAX RIC to discover the xApp and make it visible through the dRAX Dashboard
2. To enable the real-time configuration of the xApp
3. To enable the developer to expose functionalities of the xApp to the rest of the dRAX as well as to external systems

Within the xApp Framework, the API interacts directly with the xApp database where the configuration of the xApp is stored. Other dRAX xApps, as well as external entities, can talk directly with the specified xApp through the REST API and configure it. The exact URL and PORT of the API for a specific xApp can be discovered through the dRAX Service Monitor.

The xApp API exposes the following endpoints:


<table>
  <tr>
   <td><strong>Endpoint</strong>
   </td>
   <td><strong>HTTP Methods</strong>
   </td>
  </tr>
  <tr>
   <td>/config 	
   </td>
   <td>GET, PUT, DELETE, POST
   </td>
  </tr>
  <tr>
   <td>/health/alive	
   </td>
   <td>GET
   </td>
  </tr>
  <tr>
   <td>/health/ready		
   </td>
   <td>GET
   </td>
  </tr>
  <tr>
   <td>/readme
   </td>
   <td>GET
   </td>
  </tr>
  <tr>
   <td>/api
   </td>
   <td>ANY
   </td>
  </tr>
</table>


The /config endpoint is used for the configuration of the xApp. You can GET the current configuration of the xApp. Using POST you can create the configuration, or edit it using the PUT method. There is also the DELETE method, which can temporarily stop the xApp without deleting its resources.  The full API documentation that includes the JSON requests and responses can be found in the dRAX Swagger. The /health endpoint can be used to check the status of the xApp. The /health/alive endpoint can be used for checking the aliveness of the xApp and the /health/ready exposes the xApp readiness. It is up to the xApp developer to implement the xApp specific readiness checks and integrate them into the corresponding Flask view. The /readme endpoint exposes the README.md file that contains the documentation of the xApp. The README.md is located in the xApp Core. The /api endpoint on the xApp API forwards requests to the xApp Core, specifically to the xApp Core REST API block. This block allows the developers to implement their own custom API endpoints which will be exposed under the /api endpoint.


## 6.2. xApp Database

Each xApp comes with its own database. We specifically use REDIS, which is an in-memory key-value database. This is where the configuration and the metadata of the xApp are stored. These stored data can be set, changed or deleted using the xApp REST API. The xApp Core reads or writes to the configuration in  real-time. The mechanism behind these real-time updates is based on the REDIS Keyspace notifications. The developer is also free to use the included REDIS database for their own purposes.


## 6.3. xApp Core

The xApp Core is where the developers implement their own new functionalities and/or services. In regards to the xApps Framework, the xApp Core listens to the notifications from the xApp database, and updates the real-time configuration parameters as needed. The xApp developer can also expose functionalities/services of the xApp Core via the REST API block, which will be exposed under the /api endpoint of the xApp API. 




# 7. xApp Core In-depth


## 7.1. xApp core Overview

The xApp Core consists of a number of functional blocks, as shown in _Figure 3. _Most of these blocks are predefined and pre programmed for the developers’ convenience, such as:



1. README.md
2. REST API
3. Redis listener
4. Settings
5. Kafka listener
6. In-Queue
7. Kafka producer
8. Out-queue
9. Action taker
10. Processor

<table>
  <tr>
   <td>


<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


<img src="images/image3.png" width="" alt="alt_text" title="image_tooltip">

   </td>
  </tr>
  <tr>
   <td>Figure 3: xApp Core
   </td>
  </tr>
</table>



### 7.1.1. Ready to go blocks

The blocks shown in orange in the xApp Core figure are predefined blocks used for the interaction with the dRAX RIC. The developer is free to also explore them and make changes, however, that is not required. 

The **Redis listener** is used to get notifications about changes to the xApp Database of the xApp framework. When a change in the configuration is detected, the Redis listener updates the configuration of the xApp in the **Settings** block. This block is used throughout the xApp to check the configuration parameters, some of which can also be real-time.

The **Kafka listener** is used to communicate with the dRAX Databus and retrieve data from it. This data is preprocessed and sent to the** In-Queue**. 

The **Kafka producer** is a block that can create new Kafka messages and send them out to the dRAX Databus. It reads any new messages found in the **Out-Queue** and sends it to the dRAX Databus. 

The** Action taker** is a block that can be called on an event-based manner. It contains the logic of sending commands to the dRAX system. There are two types of commands currently supported:



1. Handover command - You can manually issue a handover command to move a specific UE from Cell_A to Cell_B.
2. Sub-band masking - You can issue a command on a per-cell basis to mask certain subbands of a cell, making them unavailable for use. 


### 7.1.2. Blocks to edit

The **Processor** block is where the developers actually input their code. This block reads each message from the In-Queue and makes them available to the developer. Following the processing step, the developer can send out their own messages that should be published on the dRAX Databus. This is done by simply putting the messages on the Out-Queue. Also, the developer can call on the Action taker to issue a handover or sub-band masking command. The processor is the place where the developer can implement their logic or algorithms to process the data available on the dRAX Databus. 

The **README.md** block shows the readme file that is present in the xApp core. This file is used for documentation purposes of the xApp. The xApp Framework Package comes with a temple of the README.md file and what it should contain. The developers are encouraged to follow the template and input the required documentation as a minimum. Of course, each developer can freely add additional documentation text to the readme file. 

The **REST API** block inside the xApp Core allows the developers to define custom API endpoints for their xApp. This REST API is connected to the main xApp API module. The xApp API module forwards requests of the /api endpoint to the REST API that is residing in the xApp Core. This way the developer can add API endpoints right in the xApp Core code, without the need to change the xApp API module. This also allows us to have one single entrypoint to all the API endpoints of the xApp through its xApp API module (both for the API endpoints defined in the xApp API module and the endpoints in the REST API block of the xApp Core). The REST API is implemented using the Tornado framework and is easily extendable. 

Using such architecture, we enable the developer to focus only on their own functionality in the processor block and the optional REST API, while the other blocks required for the communication and integration with dRAX are already pre-built.


## 7.2. Overview of the xApp interactions with the RIC

Once an xApp and its framework is deployed into dRAX, then it can interact with all the components present in the system. Currently, there are two main ways to achieve this: via the dRAX data bus or via the REST APIs. Depending on the use case, a developer can choose which communication method fits his needs best. The use of the data bus is encouraged in scenarios where xApps need to exchange real-time data or issue commands via the Action Taker. The use of the REST APIs is best for non-real-time information gathering or for utilizing the Netconf functionalities exposed by the dRAX REST API Gateway. 

The xAPP interaction with the cells can happen either via the REST API or via direct Netconf sessions to the corresponding Netconf servers.




# 8. The xApp Documentation README.me


## 8.1. The README.md file

The README.md is a text file used to provide a reliable way to explain, describe and illustrate an xApp. Its content is fully text-based and allows special syntax using Markdown. The Markdown language is a lightweight markup language that allows the creation of formatted text using only a plain-text editor. It allows to control the display of the document; formatting words as bold or italic, adding images, creating lists and much more. By filling the README.md file with xApp information, the Accelleran dRAX RIC will  display its content automatically in the dRAX dashboard of the specific xApp. Thus, a full description of the xApp and its context can be provided with a special style for the illustration.

Here are some links describing the Markdown features:



* Wikipedia Markdown: [https://en.wikipedia.org/wiki/Markdown](https://en.wikipedia.org/wiki/Markdown)
* Markdown CheatSheet: [https://www.markdownguide.org/cheat-sheet/](https://www.markdownguide.org/cheat-sheet/)
* StackEdit: In-browser Markdown Editor
* Markdown guide: [https://guides.github.com/features/mastering-markdown/](https://guides.github.com/features/mastering-markdown/) 


## 8.2. What we expect inside the README.md file

The README.md file is used to explain, describe and illustrate xApp behavior. It is the place where the xApp developer can give information about the xApp and its context.

The xApp README file generally contains the following elements:



* A description of the xApp and its functionality
* If the xApp is sharing data on the dRAX RIC Databus or not
* If the xApp is sharing data, what is the format of the data
* If the xApp is sharing data, which topic on the dRAX RIC Databus is the data shared on
* If the xApp has additional custom API endpoints, and an explanation of what they are and what they do

Then, it is the responsibility of the xApp developer to feed the information correctly and give the best description possible for its xApp. The Markdown style formatting is also available to help xApps developers to structure the document but it’s not mandatory.


## 8.3. How the README.md is made available via /readme endpoint

As with the /config and /api endpoint on the xApp API, (consequently used for configuration purposes and used to forward request to the xApp core), the /readme endpoint retrieves the README.md file from the xApp core and make it available for other modules. Only the GET method is accepted for this endpoint.

To suit this behavior, the README.md must be a text file co-located with the xApp core files. An example of the README.md file is already provided in the xAppFramework Package to demonstrate the /readme endpoint functionality.

Thus, the README.md content must be available triggering its address on the xApp API port:

_http://&lt;kubernetes_ip>:&lt;xapp_api_port>/readme_

Just substitute &lt;kubernetes_ip> with the advertised address of your Kubernetes cluster and the &lt;xapp_api_port> by the exposed port of your xApp API.


# 9. xApp Core Processor In-depth

As stated before, most of the xApp Core has been pre developed so that the developer can only focus on their contribution. We have specified the place to put your code in the Processor block of the xApp Core. Specifically, if you open the processor.py file, you can look for the 'run' function. Here we have the xApp loop implemented. Inside of it, we grab the latest data coming in from the 'in_queue'. From this point on, you may start developing.

In the processor.py file, we have already included a number of functions that abstract the dRAX specific code. In the xApp loop, we have included a number of examples that show you what you can do in an xApp. In this section we will explain the different abstracted functions and interactions an xApp can have within the dRAX RIC.


## 9.1. dRAX RIC Databus

The data from the dRAX RIC Databus comes in the ‘in_queue’; variable inside the xApp Core Processor. We used the Python Queue library to implement the queue, which is defined as a First In First Out Queue. We retrieve the data from the queue with the following line:


```
data = in_queue.get()
```


A simple way to check what messages are received is to simply log everything coming in from the ‘in_queue’. This is shown in Example 1 in the xApp Core Processor.


```
### Example 1: Just logging all the messages received from the dRAX Databus
logging.info("Received message from dRAX Databus!")
logging.debug("dRAX Databus message: {data}".format(data=data))
```


By checking the logs of the xApp, you will be able to see that the messages received are in the JSON format. Therefore, the ‘data’ variable we have in the xApp Core Processor is a Python dictionary. There are a number of different messages currently available on the dRAX RIC Databus. Please check the “Data on the dRAX RIC Databus In-depth” section for a detailed description of which data is available.


## 9.2. dRAX Commands

From the xApp you can currently issue two types of commands towards the dRAX:

1. The handover command

2. The subband masking command


### 9.2.1. The handover command

dRAX enables you to issue a handover command of a specific UE, from its currently serving cell to a certain target cell, assuming of course the target cell is in UEs range. In the processor, we show this in Example 4:


```
### Example 4: Send handover command
handover_list = [
    {'ueIdx': 'ueRicId_to_handover_1', 'targetCell': 'Cell_1', 'sourceCell': 'Cell_2'},
    {'ueIdx': 'ueRicId_to_handover_2', 'targetCell': 'Cell_2', 'sourceCell': 'Cell_1'}
]
send_handover_command(settings, handover_list)
```


First, we have to create the handover_list. This is a list of dictionaries in Python. Each dictionary, as shown above, contains a handover request, where we have the following elements:



* ‘ueIdx’: This is the UE RIC ID of the UE you want handed over;
* ‘targetCell’: The target cell to which the UE should get handed over to;
* ‘servingCell’: The UEs’ current serving cell. 

All that information can be retrieved from the messages on the dRAX RIC Databus. Once the handover_list is created, you simply call the 'send_handover_command' function passing along the settings variable and the handover_list. This function abstracts the lower-level details on how a handover command message is generated and sent towards the dRAX and cells. Also, the different configuration options, such as where to send the message etc., are stored in the settings variable. As described before, this settings variable is populated by the configuration options of the xApp.


### 9.2.2. Subband-masking command

dRAX enables you to also mask certain subbands of a cell. This way, these subbands become unusable by the cell. Similarly to the handover command, we have abstracted the lower-level details, and created a function you can call to mask the subbands of a certain cell. This is described in Example 5 in the processor.py:


```
### Example 5: Send subband masking command
subband_mask = [
    {'cell': 'Cell_1', 'num_of_bands': '13', 'mask': [1,1,1,1,1,1,1,1,1,1,1,1,1]},
    {'cell': 'Cell_2', 'num_of_bands': '9', 'mask': [1,1,1,1,1,1,1,1,1]}
]
send_subband_masking_command(settings, subband_mask)
```


First we create the subband_mask variable, which is a list of dictionaries in Python. Each dictionary contains the following elements:

- 'cell': This is the cell name of the cell we want to apply subband masking on;

- 'num_of_bands': Number of bands, should be 13 for 20 Mhz or 9 for 15MHz;

- 'mask': This is a list containing bools where each bool corresponds to a subband of the cell, if it's set to true the subband is used, else if it's set to false the subband is masked and not used.


## 9.3. Publish to dRAX RIC Databus

From your xApp, you can publish data on the dRAX RIC Databus for other xApps to use as well. There are two types of publishings:



1. Event-based
2. Periodic 


### 9.3.1. Event-based publishing 

You can publish certain event-based data to the dRAX RIC Databus. This is shown in Example 2 in the processor.py of the xApp Core:


```
### Example 2: Publishing one time or event-based data on the dRAX Databus
###                      We will publish on topic "my_test_topic", and just republish the "data" data
publish_on_kafka(out_queue, "my_test_topic", data)
```


We have created an abstraction function called ‘publish_on_kafka’, which takes the following parameters:



1. out_queue: this is the out_queue of the xApp Core
2. "my_test_topic": This is the topic on the dRAX RIC Databus to which the data should be published
3. data: the actual data that needs to be published, it should be of a type that can be serialized.


### 9.3.2. Periodic publishing

The second option is to create periodic publishing of data. We have prepared a variable called ‘data_store’ that is available to use in the xApp loop in the processor.py. Any data that the developer wants to publish periodically should be saved in this variable. It is by default defined as a Python dictionary. once the data is stored in this variable, It is taken by the ‘periodic_publisher’ block of the xApp Core and published in periodic intervals. This has already been developed, so the developer only needs to save the data and update it in the ‘data_store” variable in the processor block.

Also, to enable periodic publishing, one must configure it for the xApp in the xApp configuration. There are two fields of interest:



1. periodic_publish - This is a bool, true means periodic publishing is enabled, else if set to false its disabled;
2. publish_interval - The periodic interval at which to publish periodically.

Check the xApp Configuration In-Depth section for more details on the xApp Configuration.


## 9.4. Use the dRAX RIC API Gateway

The dRAX RIC exposes a number of APIs through the dRAX RIC API Gateway. The full documentation on all of the endpoints is available on the dRAX RIC API Gateway Swagerhub, which is exposed on the following URL: [http://10.20.20.20:31315/api/v1/docs/](http://10.20.20.20:31315/api/v1/docs/)

Here 10.20.20.20 is the Kubernetes cluster advertisement address, so you should change it accordingly. 

Inside the Processor block of the xApp Core, we have already implemented the code for making API calls, and abstracted the details on how to connect to the API. Basically, that information is stored in the xApp Database under API_GATEWAY_URL and API_GATEWAY_PORT. We created 3 examples of how this works in the Processor, while the SwagerHub contained detailed information and description of all the endpoints.

In summary, through the dRAX RIC API Gateway you can:



* xApps
    * Get a list of deployed xApps
    * Get/Modify the configuration of another xApp
    * Check if the xApp is healthy
    * Deploy or delete other xApps
* 4G Radio Controller
    * Check the status of the 4GRC
    * Configure the 4GRC
* Cells
    * Configure basic parameters of a cell using a JSON
    * Send a full NetConf RPC through the API to configure the cell
    * Upload a pre-existing configuration
    * Auto-configure cells
    * Reboot cells 


### 9.4.1. How to use the dRAX RIC API Gateway in general

Example 6 shows how to call the API gateway on the following endpoint:


```
"/xappconfiguration/discover/services/netconf" 
```


This endpoint, as described in the SwagerHub, returns the list of netconf services deployed on dRAX. Since the Accelleran small cells have NetConf servers running in them to be able to use NetConf to configure them, this way we can get that information.


```
### Example 6: How to use the dRAX RIC API Gateway
endpoint = '/xappconfiguration/discover/services/netconf'
api_response = requests.get(create_api_url(settings, endpoint))

if api_response.status_code == 200:
  try:
    logging.info(api_response.json())
  except:
    logging.info('Failed to load JSON, showing raw content of API response:')
    logging.info(api_response.text)
```


We use the “create_api_url” function to create the URL for a specific endpoint on the dRAX RIC API Gateway. 


### 9.4.2. How to pick only the port at which NetConf is exposed

Example 7 shows how to parse the retrieved information from the dRAX RIC API Gateway. Because the information retrieved can be parsed as JSON, in python we can simply use it as a Python dictionary:


```
### Example 7: Get the ports where the netconf servers of cells are exposed
        endpoint = '/xappconfiguration/discover/services/netconf'
        api_response = requests.get(create_api_url(settings, endpoint))

        for cell, cell_info in api_response.json().items():
            for port in cell_info['spec']['ports']:
                if port['name'] == 'netconf-port':
                    logging.info('Cell [{cell}] has NETCONF exposed on port [{port}]'.format(
                        cell=cell,
                        port=port['node_port'])
                    )
```


This would log the following example data:


```
[netconf-dageraadplats] has NETCONF exposed on port [30023]
[netconf-parking] has NETCONF exposed on port [32634]
```





# 10. 4G RAN Data on the dRAX RIC Databus In-depth


## 10.1. Beacons

**Description**

Both the Layer 2 and Layer3 of each cell send out beacons to report that they are alive. 

**Format**

The Layer 2 beacons has the following format:


<table>
  <tr>
   <td><code>{</code>
<p>
<code>  "beaconInfo": {</code>
<p>
<code>    "componentId": "Generic_cell_name",</code>
<p>
<code>    "interval": 2,</code>
<p>
<code>    "serviceInfo": "DUPLEX=TDD",</code>
<p>
<code>    "serviceType": "SERVICETYPE_LAYER2"</code>
<p>
<code>  },</code>
<p>
<code>  "fromChannelId": 36,</code>
<p>
<code>  "messageType": 1,</code>
<p>
<code>  "timestamp": 1601798237277110800,</code>
<p>
<code>  "topic":"TOPIC_BEACONS"</code>
<p>
<code>}</code>
   </td>
   <td><code># Beacon info block</code>
<p>
<code># Cell name </code>
<p>
<code># Interval of sending beacons</code>
<p>
<code># Duplex mode of the cell</code>
<p>
<code># SERVICETYPE_LAYER2 indicates layer 2</code>
<p>
<code># Timestamp in nano epochs</code>
<p>
<code># dRAX Topic Information</code>
   </td>
  </tr>
</table>


The Layer 3 beacons have a similar format, and just differ in the serviceType field:


<table>
  <tr>
   <td><code>{</code>
<p>
<code>  "beaconInfo": {</code>
<p>
<code>    "componentId": "Generic_cell_name",</code>
<p>
<code>    "interval": 2,</code>
<p>
<code>    "serviceInfo": "",</code>
<p>
<code>    "serviceType": "SERVICETYPE_LAYER3"</code>
<p>
<code>  },</code>
<p>
<code>  "fromChannelId": 36,</code>
<p>
<code>  "messageType": 1,</code>
<p>
<code>  "timestamp": 1601798237277110800,</code>
<p>
<code>  "topic":"TOPIC_BEACONS"</code>
<p>
<code>}</code>
   </td>
   <td><code># Beacon info block</code>
<p>
<code># Cell name </code>
<p>
<code># Interval of sending beacons</code>
<p>
<code># SERVICETYPE_LAYER3 indicates layer 3</code>
<p>
<code># Timestamp in nano epochs</code>
<p>
<code># dRAX Topic Information</code>
   </td>
  </tr>
</table>





## 10.2. UE Measurements

**Description**

If periodic reporting is enabled in the 4G Radio Controller, the UEs will be instructed to send periodic measurements. These measurements contain the RSRP and RSRQ value from the UE to its serving cell, as well as a maximum of 8 neighboring cells. Each measurement to a specific cell is sent as a separate message. 

**Format**


<table>
  <tr>
   <td><code>{</code>
<p>
<code>  'timestamp': '1590609372749960768',</code>
<p>
<code>  'topic':'Topic_OPENRAN_DATA',</code>
<p>
<code>  'type': 'UE_MEASUREMENT',</code>
<p>
<code>  'ueMeasurement': {</code>
<p>
<code>    'cellId': 'Dageraadplaats', </code>
<p>
<code>    </code>
<p>
<code>    'rsrp': 94, </code>
<p>
<code>    'rsrq': 27, </code>
<p>
<code>    'ueCellId': 'Dageraadplaats',</code>
<p>
<code>    'ueDraxId': '0d567a4a-86d8-4a38-b0c2-dbbcf8117c53',</code>
<p>
<code>    'ueRicId': 'UE_2'</code>
<p>
<code>  }</code>
<p>
<code>}</code>
   </td>
   <td><code># Timestamp in nano epochs</code>
<p>
<code># dRAX Topic Information</code>
<p>
<code># Indicating the type as UE_MEASUREMENT</code>
<p>
<code># UE Measurement block</code>
<p>
<code># Cell name from which the measurement is reported</code>
<p>
<code># The RSRP value</code>
<p>
<code># The RSRQ value</code>
<p>
<code># UEs' serving cell name</code>
<p>
<code># UE ID in dRAX (also used for dRAX Commands)</code>
<p>
<code># UE RIC ID used as a simpler, more visual UE ID (used in the dashboard, Grafana, etc.)</code>
   </td>
  </tr>
</table>


If the cellId and ueCellId fields are the same, it means the report is from the UEs’ serving cell. Otherwise, if the cellId is different, the report is from that neighboring cell.


## 10.3. UE Throughput

**Description**

The dRAX RIC also reports the downlink and uplink throughputs of the UEs in the network. 

**Format**


<table>
  <tr>
   <td><code>{</code>
<p>
<code>  "throughputReport": {</code>
<p>
<code>    "cellId": "Bt1Cell",</code>
<p>
<code>    "dlThroughput": 1,</code>
<p>
<code>    "ueDraxId": "0d567a4a-86d8-4a38-b0c2-dbbcf8117c53",</code>
<p>
<code>    "ueRicId": "UE_2",</code>
<p>
<code>    "ulThroughput": 0</code>
<p>
<code>  },</code>
<p>
<code>  "timestamp": 1598974491926513000,</code>
<p>
<code>  "topic":"Topic_OPENRAN_DATA"</code>
<p>
<code>  "type": "THROUGHPUT_REPORT"</code>
<p>
<code>}</code>
   </td>
   <td><code># Throughput report block</code>
<p>
<code># Serving cell of the UE</code>
<p>
<code># Downlink throughput</code>
<p>
<code># UE ID in dRAX (also used for dRAX Commands)</code>
<p>
<code># UE RIC ID used as a simpler, more visual UE ID (used in the dashboard, Grafana, etc.)</code>
<p>
<code># Uplink throughput</code>
<p>
<code># Timestamp in nano epochs</code>
<p>
<code># dRAX Topic Information</code>
<p>
<code># Indicating the type</code>
   </td>
  </tr>
</table>



## 10.4. CQI

**Description**

The dRAX RIC also supports retrieving the CQI report of the UE to its serving cell. You can get the CQI value from each subband as well as the wideband CQI value.

**Format**


<table>
  <tr>
   <td><code>{</code>
<p>
<code>  "cqiReport": {</code>
<p>
<code>    "cellId": "Bt2Cell",</code>
<p>
<code>    "cqiList": {</code>
<p>
<code>      "0": 12,</code>
<p>
<code>      "1": 12,</code>
<p>
<code>      "2": 12,</code>
<p>
<code>      "3": 12,</code>
<p>
<code>      "4": 12,</code>
<p>
<code>      "5": 13,</code>
<p>
<code>      "6": 12,</code>
<p>
<code>      "7": 13,</code>
<p>
<code>      "8": 13,</code>
<p>
<code>      "9": 13,</code>
<p>
<code>      "10": 14,</code>
<p>
<code>      "11": 14,</code>
<p>
<code>      "12": 12</code>
<p>
<code>    },</code>
<p>
<code>    "cqiListSize": 13,</code>
<p>
<code>    "ueDraxId": "0d567a4a-86d8-4a38-b0c2-dbbcf8117c53",</code>
<p>
<code>    "ueRicId": "UE_2",</code>
<p>
<code>    </code>
<p>
<code>    "widebandCqi": 13</code>
<p>
<code>  },</code>
<p>
<code>  "timestamp": 1601464410244201200,</code>
<p>
<code>  "topic":"Topic_OPENRAN_DATA",</code>
<p>
<code>  "type": "CQI_REPORT"</code>
<p>
<code>}</code>
   </td>
   <td><code># CQI Report block</code>
<p>
<code># UEs' serving cell</code>
<p>
<code># The CQI values list per subband</code>
<p>
<code># The size of the CQI values list</code>
<p>
<code># UE ID in dRAX (also used for dRAX Commands)</code>
<p>
<code># UE RIC ID used as a simpler, more visual UE ID (used in the dashboard, Grafana, etc.)</code>
<p>
<code># The value of the wideband CQI</code>
<p>
<code># Timestamp in nano epochs</code>
<p>
<code># dRAX Topic Information</code>
<p>
<code># Indicates the type of message</code>
   </td>
  </tr>
</table>



## 10.5. BLER

**Description**

The dRAX RIC receives the BLock Error Rate (BLER) of the communication between the UE and the serving cell.

**Format**


<table>
  <tr>
   <td><code>{</code>
<p>
<code>  "blerReport": {</code>
<p>
<code>    "cellId": "Bt2Cell",</code>
<p>
<code>    "dlBler": 0,</code>
<p>
<code>    "ueDraxId": "0d567a4a-86d8-4a38-b0c2-dbbcf8117c53",</code>
<p>
<code>    "ueRicId": "UE_2",</code>
<p>
<code>    </code>
<p>
<code>    "ulBler": 0</code>
<p>
<code>  },</code>
<p>
<code>  "timestamp": 1601467816295917800,</code>
<p>
<code>  "topic":"Topic_OPENRAN_DATA",</code>
<p>
<code>  "type": "BLER_REPORT"</code>
<p>
<code>}</code>
   </td>
   <td><code># BLER Report block</code>
<p>
<code># UEs' serving cell</code>
<p>
<code># Downlink BLER after HARQ in % * 1000 000 (fixed decimal point notation)</code>
<p>
<code># UE ID in dRAX (also used for dRAX Commands)</code>
<p>
<code># UE RIC ID used as a simpler, more visual UE ID (used in the dashboard, Grafana, etc.)</code>
<p>
<code># Uplink BLER after HARQ in % * 1000 000 (fixed decimal point notation)</code>
<p>
<code># Timestamp in nano epochs</code>
<p>
<code># dRAX Topic Information</code>
<p>
<code># Indicates the type of message</code>
   </td>
  </tr>
</table>





# 11. xApp Configuration In-depth

The configuration of the xApp is stored inside the xApp Database. There are multiple ways of creating and editing the configuration options. First, we enable creating and editing the configuration options on deployment time through the xApp Helm Chart. Second, we also enable changing the configuration options during runtime of the xApp. This can be done either through the dRAX Dashboard or by sending the so called xApp Configuration JSON to the xApp API.

The xApp Core then reads the configuration options from the xApp Database and uses them throughout. The xApp Core consists of the redis_listener block which talks to the xApp Database. This block knows how to parse the information from the xApp Database, and is also notified if a change in configuration occurs in the xApp Database. 

It is important to understand that the first time you deploy your xApp, the deployment time configuration options from the xApp Helm Chart will be saved in the xApp database. When you uninstall the xApp, the xApp Database has persistence enabled, which means that your configuration is saved. When you re-deploy the xApp, the configuration will be restored from the persistent storage. You can, however, on deployment time choose to delete the configuration in the persistent storage and save the deployment time configuration options instead. 


## 11.1. The default xApp configuration Options

Below is a list of the default configuration options that come predefined in the xApp Framework Package.


<table>
  <tr>
   <td>Configuration option
   </td>
   <td>Type
   </td>
   <td>Description
   </td>
  </tr>
  <tr>
   <td>metadata.name
   </td>
   <td>Uneditable 
   </td>
   <td>AUTO-GENERATED DON’T EDIT. The name of the xApp
   </td>
  </tr>
  <tr>
   <td>metadata.configName
   </td>
   <td>Optional
   </td>
   <td>A generic name of the xApp Configuration JSON 
   </td>
  </tr>
  <tr>
   <td>metadata.namespace
   </td>
   <td>Uneditable 
   </td>
   <td>AUTO-GENERATED DON’T EDIT. The namespace where the xApp is deployed. This is the same namespace as dRAX.
   </td>
  </tr>
  <tr>
   <td>description
   </td>
   <td>Optional
   </td>
   <td>A description of the xApp
   </td>
  </tr>
  <tr>
   <td>last_modified
   </td>
   <td>Uneditable 
   </td>
   <td>AUTO-GENERATED DON’T EDIT. A timestamp when the configuration was last modified. In format: 
<p>
"d/m/Y H:M:S"
   </td>
  </tr>
  <tr>
   <td>config.apiGatewayUrl
   </td>
   <td>Uneditable 
   </td>
   <td>AUTO-GENERATED DON’T EDIT. The URL to the dRAX API Gateway
   </td>
  </tr>
  <tr>
   <td>config.apiGatewayPort
   </td>
   <td>Uneditable 
   </td>
   <td>AUTO-GENERATED DON’T EDIT. The PORT to the dRAX API Gateway
   </td>
  </tr>
  <tr>
   <td>config.REDIS_URL
   </td>
   <td>Uneditable 
   </td>
   <td>AUTO-GENERATED DON’T EDIT. This is the URL to the xApp Database
   </td>
  </tr>
  <tr>
   <td>config.REDIS_PORT
   </td>
   <td>Uneditable 
   </td>
   <td>AUTO-GENERATED DON’T EDIT. The PORT of the xApp Database
   </td>
  </tr>
  <tr>
   <td>config.LOG_LEVEL
   </td>
   <td>Required
   </td>
   <td>The log level of the xApp. Can be changed during runtime. 
   </td>
  </tr>
  <tr>
   <td>config.KAFKA_URL
   </td>
   <td>Uneditable 
   </td>
   <td>AUTO-GENERATED DON’T EDIT. The URL of the dRAX RIC Databus
   </td>
  </tr>
  <tr>
   <td>config.KAFKA_PORT
   </td>
   <td>Uneditable 
   </td>
   <td>AUTO-GENERATED DON’T EDIT. The PORT of the dRAX RIC Databus
   </td>
  </tr>
  <tr>
   <td>config.KAFKA_LISTEN_TOPICS
   </td>
   <td>Required
   </td>
   <td>The Topics to subscribe to on the dRAX RIC Databus.
   </td>
  </tr>
  <tr>
   <td>config.kafka_producer_topic
   </td>
   <td>Optional
   </td>
   <td>The Topic on the dRAX RIC Databus where the xApp will publish its data, if the xApp is publishing data.
   </td>
  </tr>
  <tr>
   <td>config.periodic_publish
   </td>
   <td>Required
   </td>
   <td>Boolean to either enable or disable periodic publishing of data from the xApp.
   </td>
  </tr>
  <tr>
   <td>config.publish_interval
   </td>
   <td>Optional
   </td>
   <td>The interval at which the periodic publishing occurs in seconds.
   </td>
  </tr>
  <tr>
   <td>jsonSchemaOptions
   </td>
   <td>Optional
   </td>
   <td>Defines the visual look of the configuration options on the dRAX Dashboard.
   </td>
  </tr>
  <tr>
   <td>uiSchemaOptions
   </td>
   <td>Optional
   </td>
   <td>Defines the visual look of the configuration options on the dRAX Dashboard.
   </td>
  </tr>
</table>



## 11.2. Setting xApp Configuration during xApp deployment 

When deploying an xApp, we have exposed a way to set the different configuration parameters via the xApp Helm Chart. This makes it easy to change the existing configuration options, but also add additional configuration options.

The configuration options can be found in the values.yaml file under the /helm_chart/xapp folder of the xApp Framework Package. Below we see how that looks like in the values.yaml file.


```
xappFrameworkConfig:
  # Enter the description of the xApp
  description: "An example xApp"

  # If you want to flush the existing config, and use the config on helm install, enable the following
  flushOnDeployment: false

draxConfig:
  - name: 'API_GATEWAY_URL'
    type: string
    value: '{{ .Values.global.kubeIp }}'
  - name: 'API_GATEWAY_PORT'
    type: string
    value: '31315'
  - name: 'KAFKA_URL'
    type: string
    value: '{{ .Values.global.kubeIp }}'
  - name: 'KAFKA_PORT'
    type: string
    value: '31090'
  - name: 'DRAX_COMMAND_TOPIC'
    type: string
    value: 'Topic_OPENRAN_COMMANDS.OranController'
  - name: 'NATS_URL'
    type: string
    value: 'nats://{{ .Values.global.kubeIp }}:31000'

xappConfig:
  - name: 'LOG_LEVEL'
    type: int
    value: 20
  - name: 'KAFKA_LISTEN_TOPICS'
    type: list
    value: 'test2, Topic_State'
  - name: 'periodic_publish'
    type: string
    value: 'True'
  - name: 'publish_interval'
    type: int
    value: 2
  - name: 'kafka_producer_topic'
    type: string
    value: 'none'
```


The configuration options are divided into three categories:



1. xApp Framework Configuration - These are the general configuration options across the whole xApp Framework.
2. dRAX Configuration Options - These are the configuration options related to the dRAX elements that the xApp can use. They are mostly auto-generated so the developer does not need to edit them.
3. xApp Configuration - Here are the xApp Core specific configuration options which the developer can edit or even add custom ones.


### 11.2.1. xApp Framework Configuration

In the xApp Framework configuration there are 2 interesting options. The first one is the ‘description’. This field is the ‘description’ field of the xApp Configuration. The developers can add the description of their xApp here. It is a convention to add a few details here like: i) the name of the xapp, ii) what it does in a short description, iii) if the xApp is publishing data on the dRAX Databus, it is useful to add here the format of the data being published so other developers can know how to parse the data. The second option is the flushOnDeployment field which can be set to ‘true’ to reset the configuration in the xApp Database on deployment time.


### 11.2.2. xApp configuration

We have created the xApp Helm Chart so that it can automatically generate the xApp configuration options inside the xApp Database on deployment time based on their definition in the value.yaml file. The xApp Configuration options are defined by three fields in the values.yaml file: 



* name
* type
* value

The ‘name’ field is the name of the configuration option, and the name that will be used inside the xApp Core to access this configuration option. Therefore, the naming convention is to name it a variable. 

The ’type’ field indicates whether the type is an “int”, “string” or “list“. An “int” type can have a number as the value, while a “string” type has a string. The “list” type is a string with comma separated values, so for example, like the 'KAFKA_LISTEN_TOPICS' xApp Config option.

Finally, the ‘value’ field is the actual value of the configuration option. So for example, the log level is defined with the name ‘LOG_LEVEL’, takes as input a number so the type is ‘int’, and the current ‘value’ is set to 20, which is the ‘info’ level logging. If we decide to change the log level to ‘debug’ we can change the ‘value’ of this configuration option to ‘10’.


## 11.3. Resetting the configuration upon redeployment of the xApp

As stated before, the xApp Configuration is saved in the xApp Database with persistence. This means that on redeployment, your old configuration will be restored. You can, however, choose to delete the old configuration and redeploy with the default deployment time configuration options. To do so, in the values.yaml file in the /helm_chart/xapp folder of the xApp Framework Package, change the value of the xappFrameworkConfig.flushOnDeployment field to ‘true’. 


```
flushOnDeployment: true
```



## 11.4. Using the dRAX Dashboard to edit the xApp configuration during runtime

Going to the xApp tab on the left of the dRAX Dashboard, we can reach the xApps list. Here each xApp has a ‘Configure’ button that a user can click. This will take us to a specific xApps’ configuration page. Here, a user can view all the xApp Configuration options as described so far. Each xApp configuration option can be edited, and by clicking the ‘Submit’ button the configuration is sent to the xApp API to take effect.


## 11.5. Using the xApp Configuration JSON to edit the xApp configuration during runtime

The configuration of an xApp can also be defined inside the xApp Configuration JSON. This JSON can be sent to the xApp API which knows how to correctly parse it and save the configuration options in the xApp Database. The configuration options inside the xApp Database are also saved according to this xApp configuration JSON format. The xApp Configuration JSON format is shown below.


```
{
    "metadata": {
        "name": "AUTO-GENERATED", # DON'T EDIT
        "configName": "GenericConfigName",
        "namespace": "AUTO-GENERATED" # DON'T EDIT
    },
    "description": "xApp Description...",
    "last_modified": "AUTO-GENERATED", # DON'T EDIT
    'config': {
        ''API_GATEWAY_URL': '10.20.20.20', # AUTO-GENERATED, DON'T EDIT
        ''API_GATEWAY_PORT': '31315', # AUTO-GENERATED, DON'T EDIT
        'REDIS_URL': '10.20.20.20', # AUTO-GENERATED, DON'T EDIT
        'REDIS_PORT': 32000, # AUTO-GENERATED, DON'T EDIT
        'DRAX_COMMAND_TOPIC': 'Topic_OPENRAN_COMMANDS.OranController', # AUTO-GENERATED, DON'T EDIT
        'NATS_URL': 'nats://10.20.20.20:31000', # AUTO-GENERATED, DON'T EDIT
        'LOG_LEVEL': 20,  
        'KAFKA_URL': '10.20.20.20', # AUTO-GENERATED, DON'T EDIT
        'KAFKA_PORT': '31090', # AUTO-GENERATED, DON'T EDIT
        'kafka_producer_topic': 'xapp_specific_topic',
        'KAFKA_LISTEN_TOPIC': 'test2, Topic_State',
        'periodic_publish': True, 
        'publish_interval': 1  # in seconds
    },
    "jsonSchemaOptions": {},
    "uiSchemaOptions": {}
}
```


By creating such a JSON and sending it to the xApp API, the configuration options can be edited during runtime. The xApp API can be reached via the dRAX RIC API Gateway. Each xApp has their API endpoints exposed through the dRAX RIC API Gateway. You can use the:

/xappconfiguration/config/{xAppName}

API endpoint to reach the configuration of each xApp. You can get the xApp configuration from this endpoint and edit it. Details on the dRAX RIC API Gateway endpoints for the xApps can be seen on Swagger which can be reached at:

http://&lt;kubernets_ip>:31315/api/v1/docs

Just substitute &lt;kubernetes_ip> with the advertise address of your Kubernetes cluster. 


## 11.6. How to add a configuration option

We add a custom configuration option to an xApp using the values.yaml file. This new field will be created upon xApp deployment. Therefore, for the new configuration option to take effect, currently we also have to delete the previous configuration. Therefore, in the values.yaml file we first have to set the flushOnDeployment field to true.

Next, under the xappConfig field in the value.yaml file, we add a custom configuration option by creating the three fields of a configuration option: name, type and value.


```
xappFrameworkConfig:
  flushOnDeployment: true

xappConfig:
  - name: 'LOG_LEVEL'
    type: int
    value: 20
  - name: 'KAFKA_LISTEN_TOPIC'
    type: string
    value: 'test2'
  - name: 'KAFKA_STATE_TOPIC'
    type: string
    value: 'Topic_State'
  - name: 'periodic_publish'
    type: string
    value: 'True'
  - name: 'publish_interval'
    type: int
    value: 2
  - name: 'kafka_producer_topic'
    type: string
    value: 'none'
  - name: 'custom_config_1'
    type: string
    value: 'some_value'
```


In the example above, we have added a ‘custom_config_1’ configuration option, with the type of ‘string’ and a value ‘some_value’.


## 11.7. How are the xApp Configuration options used in the xApp core

The xApp Configuration options are retrieved from the xApp Database via the redis_listener block of the xApp Core. They are then stored in the settings variable and follow the xApp Configuration JSON format. The settings variable also has a Lock to avoid conflicting read/writes of the configuration options. 

You can access the different configuration options in the xApp Core like this:


```
with settings.lock:
periodic_publish = settings.configuration['config']['periodic_publish']
last_modified = settings.configuration['last_modified']
config_name = settings.configuration['metadata']['configName']
```



## 11.8. How to create configuration option widgets on the dRAX Dashboard

The dashboard front-end makes use of the react-jsonschema-form library to visualize the xApp configuration form. Changing the  "jsonSchemaOptions" and "uiSchemaOptions" objects within the xApp configuration, will change the way the form appears on the browser. e.g.


```
uiSchemaOptions:{"API_GATEWAY_PORT": {"ui:disabled": true}} 
```


will disable the corresponding text box in the form on the frontend. 

For more information and how to modify these options check :

[https://react-jsonschema-form.readthedocs.io/en/latest/usage/widgets/](https://react-jsonschema-form.readthedocs.io/en/latest/usage/widgets/)

The visualisation library used can be found here:

[https://github.com/rjsf-team/react-jsonschema-form](https://github.com/rjsf-team/react-jsonschema-form)    




# 12. Extending the API of the xApp

The dRAX RIC platform has been, in its own roots, designed to use an adaptable approach. Through its genuinely defined architecture, an xApp can be developed and extended depending on needs to provide infinite tastes and flavors to the RIC.

To fit this challenge, the xApp framework has been specifically implemented to allow developers to create their own homemade API endpoints and thus extend the xApp features. The xApp API has a couple of endpoints predefined:



* the /config – which is an exposed endpoint used for configuration purposes and forwards requests to the pre-built xApp API Module
* and /api – which is an exposed endpoint that forwards requests to the xApp Core

The /config endpoint is pre-built in order to support seamless integration with the dRAX RIC. 

The /api on the other hand is the extendable and optional endpoint that we have left up to developers to implement the functionalities. The code for handling the /api endpoint is located in the xApp Core. This way we keep the xApp Core the single point of focus for the developers.

Thanks to this behavior, one xApp developer can create and add customs and unlimited numbers of endpoints behind /api at needs with ease.

Consequently, during the development phase, one xApp developer will just have to take care about the endpoints he wants to create and the actions triggered when these endpoints are reached while all the tools needed for easy development and the whole integration in the RIC will be automatically handled.

The API endpoints in the xApp Core are created using the Tornado framework which grants easy building of web and asynchronous networking applications. For more information about the tornado framework, you can reach the following link :[ https://www.tornadoweb.org/en/stable/](https://www.tornadoweb.org/en/stable/) 

It allows xApp developers to create handlers for the API endpoint requests (POST, GET, DELETE, PUT, etc…), to create different actions per endpoint depending on the requests received and add support for event-based actions.

The code for the /api endpoint is located in the xApp Core under the restapi.py module.


## 12.1. Adding an endpoint behind the /api

To create a new entry point for your xApp under /api, one xApp developer simply needs to edit the xApp Core restapi module.

For each endpoint defined in the list, a callback function is associated. These callbacks will be called each time the endpoint is triggered and can also be overloaded at needs to handle every REST request (GET, PUT, POST, DELETE, …).

It is very important to note that the added API endpoints will be created under the /api/ root.

In the following example /api/actions and /api/request has been defined as endpoints with corresponding callbacks handlers. The third argument is used for callback context. 


```
app = tornado.web.Application([
    	("/api/", MainApiHandler, {"in_queue":in_queue}),
    	("/api/actions", ActionsHandler),
    	("/api/request", RequestHandler),
	], debug=True)
```


Then, the endpoint handler can be declared to ensure defined behaviors per REST request.

In the following example, the RequestHandler class is defined for the /api/request enpoint. It handles:



* A first initialize function used to set the callbacks context (see 11.2).
* A get function that handles the REST request GET for this specific endpoint
* A post function that handles the REST request POST for this specific endpoint.

    ```
class RequestHandler(tornado.web.RequestHandler):
	def initialize(self):
    	self.write("init request handler\n")
	def get(self):
    	self.write("request handler get\n")
	def post(self):
        self.set_header("Content-Type", "text/plain")
    	self.write("You wrote " + self.get_body_argument("message"))
```



Hence, every endpoint is bound to a specific class handler that will define and organize all the expected management and behaviors when it will be triggered.


## 12.2. How to extend the context of the newly added API endpoint

While deploying a new endpoint, xApp developers also might want to extend its functionalities to have the hand on much wider components. To have access to the functionalities of these components, an application context will need to be provided to the handlers.

This context will be forwarded automatically to the user callback for further use inside it. For this purpose, a third argument is used during the declaration of the endpoint.

It allows xApps developers to declare a dictionary of variables and information that will be made automatically available in the initialize function of the handler definition.

The following example introduce the notion of context inside the the callback with the in_queue object passed to the callback function MainApiHandler:


```
class MainApiHandler(tornado.web.RequestHandler):
    def initialize(self, in_queue):
        self.in_queue = in_queue
    def get(self):
        self.write("Write a message into the in_queue...\n")
        self.in_queue.put("Message from restapi main handler")
        self.write("Message written.\n")
```


The in_queue object is recreated and set in the initialize function of the MainApiHandler and can be used to add a message into the in_queue.

This way a certain message from the API can be forwarded to the in_queue and used in the processor. Hence, xApp developers can extend the possibilities of the xApps at needs in a fully configurable environment.


## 12.3. Reaching the newly created /api endpoint

Once a new endpoint has been defined, it is automatically made available in the dRAX xApp framework through the xApp API pod.

Thus, xApp developer can trigger it while reaching: \
http://&lt;kubernetes_ip>:&lt;xapp_api_port>/api/&lt;new_endpoint>

Just substitute &lt;kubernetes_ip> with the advertised address of your Kubernetes cluster and the &lt;xapp_api_port> by the exposed port of your xApp API.


# 13. Helm Chart In-Depth

The way we package and deploy our xApp is using Helm. Therefore, we created a Helm Chart to accompany our xApp. Editing the Helm chart is done as any other Helm chart. Most of the fields in the values.yaml file are auto-generated and prebuilt. 


## 13.1. Version control

The version of the xApp is configured in the Chart.yaml file in the /helm_chart/xapp folder. Here we have two version fields: i) version and ii) appVersion. The first one corresponds to the version of the xApp Helm Chart, while the second one is the version of the xApp Core Docker image. We tend to use the same version tag for both of these fields to make it simple and easy to read. However, the developer can choose to do differently.


## 13.2. xApp Core Docker image to use

Which Docker image of the xApp Core is used when deploying the xApp Helm Chart is defined in the values.yaml file under the ‘_image.repository’_ field. You can specify a local Docker image or a remote Docker repository.


## 13.3. How to add another microservice helm chart to the xApp

Adding another microservice helm chart to your xApp requires some Helm knowledge. We try to create a simple guide here as a starting point. We will add InfluxDB to our xApp.



1. Add the InfluxDB Helm Chart to the xApp

    This is done by editing the Chart.yaml file in the /helm_chart/xapp folder. Under the ‘dependencies’ field. add the following:


    ```
dependencies:
  - name: xapp-redis
    condition: xapp-redis.enabled
    version: 0.1.4
    repository: https://accelleran.github.io/helm-charts/
  - name: xapp-api
    condition: xapp-api.enabled
    version: 0.3.6
    repository: https://accelleran.github.io/helm-charts/
  - name: influxdb
    condition: influxdb.enabled
    version: 4.8.5
    repository: https://github.com/influxdata/influxdb
```



    Give the Helm Chart a ‘name’, set the conditions for it to be installed under ‘condition’, choose the ‘version’ of the helm chart to be installed and finally give the Helm Chart repository where the Helm Chart is located.

2. Next, edit the values.yaml file to enable editing the new Helm Charts specific configuration:

    ```
...
influxdb:
  # Enable/disable installation of InfluxDB
  enabled: true

  setDefaultUser:
    enabled: true
    user:
      username: admin
      password: admin
...
```



    Here, make sure to use the ‘name’ used in the Chart.yaml file. Note that here we create the ‘enabled’ option, which is the ;condition; statement in the Chart.yaml. The rest of the fields are InfluxDB specific fields which come from its own Helm Chart. Therefore, you can pick any config option from the InfluxDB Helm charts’ values.yaml and incorporate it in the xApp values.yaml file this way.

3. Optionally add some of the InfluxDB specific configuration options as xApp Configuration so that you can access them in the xApp Core. To do so, just add custom xApp configuration options and set them properly:

    ```
xappConfig:
  - name: 'influxdb_username'
    type: string
    value: 'admin'
```






# 14. xApps Lifecycle In-depth

In this section we will explain the lifecycle of the xApps in the RIC.


<table>
  <tr>
   <td>

<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


<img src="images/image4.png" width="" alt="alt_text" title="image_tooltip">

   </td>
  </tr>
  <tr>
   <td>Figure 1: dRAX RIC and xApp interactions
   </td>
  </tr>
</table>



## 14.1. Service Monitor and Service Orchestrator roles

Part of the dRAX RIC are the Service Monitor and Service Orchestrator which play a key role in the xApp lifecycle management. 


### 14.1.1. Service Orchestrator

The Service Orchestrator is a service of the dRAX RIC which has a number of tools at its disposal. It contains a Kubernetes client as well as a Helm client, enabling it to deploy Kubernetes resources. It is exposed through the dRAX RIC API Gateway on 2 endpoints:



* /deploy/xapp
* /deploy/delete

Details on the endpoint can be found on the dRAX RIC API Gateway Swagger page: 

http://&lt;kubernets_ip>:31315/api/v1/docs

Just substitute &lt;kubernetes_ip> with the advertise address of your Kubernetes cluster. 

The Service Orchestrator supports multiple deployment options:



* Using a local Helm chart package
* Using a remote Helm repository

In both cases, you can also send the optional values.yaml file with specific xApp configuration options.


### 14.1.2. Service Monitor

The Service Monitor is equipped with the Kubernetes client, so it's able to monitor all the deployed resources. Therefore the Service Monitor is used for:



* xApp discovery
* Retrieving relevant configuration options of dRAX RIC resources, such as services, ports, etc.


## 14.2. Onboarding

There are 2 ways of onboarding an xApp:



* Through the dRAX Dashboard
* By using the Service Orchestrator endpoint via the dRAX RIC API Gateway


### 14.2.1. Using the dRAX Dashboard

Go to the dRAX dashboard, which by default is on port 31315:

http://&lt;kubernetes_ip>:31315

On the left side, click the xApps tab. On the next page you should see a “Deploy new xApp” button. After clicking the button you will land on the xApp deployment page. Here you need to specify the xApp metadata, such as:



* xApp name
* Organization that made the xApp
* Team inside the organization
* Version of the xApp
* Owner of the xApp

Next, you specify the “Deploy Method”. Here you can choose:



* Upload Helm Chart - To upload a packaged Helm chart .tgz file
* Remote Helm Repository - To specify a Helm repository from which to deploy the xApp

If you select “Upload Helm Chart”, you will get the option to:



* Upload the Helm Chart package .tgz file
* Upload the optional values.yaml file for the Helm Chart

As seen before, the values.yaml file contains for example the Kubernetes advertise IP address, which is different from Kubernetes to Kubernetes. So, each deployment of an xApp can use their version of the values.yaml file specifying the correct configuration options. 

If you select “Remote Helm Repository”, you will need to fill in a few more fields:



* Repository name - A generic name of your choice
* Repository URL - The URL where the Helm Chart repository is located
* Chart name - the name of the Helm Chart in the repository where the xApp is located
* An option to also upload a values.yaml file

Once the details are filled in, click “Submit”, and your xApp will be deployed.


### 14.2.2. Using the Service Orchestrator endpoint via the dRAX RIC API Gateway

As described before, we use the Service Orchestrator to deploy the dRAX RIC xApps. The method described before, using the dashboard, also in the background uses the Service Orchestrator. Therefore, you may also choose to deploy an xApp directly using the Service Orchestrator by using its API endpoints via the dRAX RIC API Gateway. Details on the contents of the API are on the dRAX RIC API Gateway Swagger:

http://&lt;kubernets_ip>:31315/api/v1/docs

Just substitute &lt;kubernetes_ip> with the advertise address of your Kubernetes cluster. 

You again have the possibility to either:



* deploy using a helm chart package .tgz file
* Deploy from a remote Helm Chart repository

Deploying an xApp using a local Helm chart tgz package file can be done using the curl command like this:


```
curl -X POST "http://10.55.1.2:31315/api/deploy/xapp" -H "accept: application/json" -H "Content-Type: multipart/form-data" -F 'xAppForm={"xAppName":"example-xapp","organization":"Accelleran","team":"dRAX","version":"0.1.0","owner":"Owner","method":"Upload Helm Chart","namespace":"default"}' -F "file=@/home/ad/xapp-training-course/xapp-0.1.0.tgz;type=application/x-compressed-tar" -F "values=@/home/ad/xapp-training-course/values.yaml;type=application/x-yaml"
```


Deploying an xApp using a remote Helm chart repository can be done using the curl command as an example:


```
curl -X POST "http://10.55.1.2:31315/api/deploy/xapp" -H "accept: application/json, text/plain, */*" -H "Content-Type: multipart/form-data" -F 'xAppForm={"xAppName":"example-xapp","organization":"Accelleran","team":"dRAX","version":"0.1.0","owner":"Owner","method":"Remote Helm Repository","chartName":"xapp-hello-world","url":"https://accelleran.github.io/xapp-hello-world/","repoName":"example-repo-name","namespace":"default"}' -F "values=@/home/dimitris/Desktop/5g_logs/values.yaml;type=application/x-yaml"
```


The following information is of interest:



* **10.55.1.2** - Is the Kubernetes IP
* **example-xapp **- is the xApp name
* **Accelleran **- is the organisation 
* **dRAX **- is the team
* **0.1.0 **- is the version of the xApp
* **Owner **- the owner of the xApp
* **Upload Helm Chart/Remote Helm Repository **- specifies whether we are uploading a helm .tgz package or using a remote Helm repository
* If a .tgz helm package file is used, then **@/home/dimitris/Desktop/5g_logs/xapp-hello-world-0.1.0.tgz **specifies the location of the .tgz helm package
* If a remote helm repository is used, then **https://accelleran.github.io/xapp-hello-world/ **is the URL to the remote repository
* If a remote helm repository is used, then **xapp-hello-world **is the name of the helm chart in the remote repository
* If a remote helm repository is used, then **example-repo-name **is the generic name used to save the repository URL in helm
* Finally, **@/home/dimitris/Desktop/5g_logs/values.yaml **specifies where the optional values.yaml file is located


## 14.3. Discovery

Once the xApp is deployed, the dRAX Service Monitor automatically registers the xApp as deployed using predefined labels in the xApp Kubernetes objects. This information is gathered by the dRAX RIC API Gateway and is used by the dRAX Dashboard where the new xApp appears in the xApps tab. From here, the user can control and configure the xApp. Using the dRAX RIC API Gateway, a user can: 



* Get the xApps list with all the deployed xApps
* Get the configuration of an xApp
* Check the health of an xApp

Details are available on the dRAX RIC API Gateway Swagger:

http://&lt;kubernets_ip>:31315/api/v1/docs

Just substitute &lt;kubernetes_ip> with the advertise address of your Kubernetes cluster. 


## 14.4. Configuration

Details on how the xApps are configured can be found in the “xApp Configuration In-depth” section. Here we summarize that the xApp can be configured:



* On deployment time using the xApp Helm Chart values.yaml file
* During runtime through the dRAX Dashboard
* During runtime through the xApps’ API, which can be reached through the dRAX RIC API Gateway

The configuration is defined by the xApp configuration JSON, which is described in detail in the “xApp Configuration In-depth” section.


## 14.5. Uninstallation

Uninstalling an xApp in the dRAX RIC can be done in 2 ways:



* Using the dRAX Dashboard
* Using the service Orchestrator API via the dRAX RIC API Gateway

If you are using the dashboard, you can simply go to the xApp tab on the left side of the dashboard. From here you can see the xApp list of currently deployed xApp. For each xApp, you can see an “Uninstall” button, which if you click will uninstall the xApp.

On the other hand, you can also reach the /deploy/delete endpoint on the dRAX RIC API Gateway, which is used to delete an xApp. details for the endpoint are available on the dRAX RIC API Gateway Swagger:

http://&lt;kubernets_ip>:31315/api/v1/docs

Just substitute &lt;kubernetes_ip> with the advertise address of your Kubernetes cluster. 

An example of how to use delete an xApp using the API Gateway can be seen below:


```
curl -X PUT -H "accept: application/json" -H "Content-Type: application/json" -d '{"name":"accelleran-drax-example-xapp-010", "namespace":"default"}' http://10.55.1.2:31315/api/deploy/delete
```


Here we use the curl command to reach the API endpoint, and use the following parameters:



* **accelleran-drax-example-xapp-010 **- This is the name of the xApp
* **default **- This is the namespace where the xApp is deployed
* **10.55.1.2 **- This is an example Kubernetes advertise IP




# 15. Writing the xApp Core in a different language

The xApp consists as mentioned before of 3 parts:



* The xApp API
* The xApp Database
* The xApp Core


## 15.1. xApp Core 

If you wish to write the xApp Core in another programming language, that is of course possible. You only need to create the same hooks as we have done in the xApp Core template. Basically, you will need:



* A Kafka consumer client - which will be able to talks to the dRAX RIC Databus
* A Kafka producer client - which can publish data on the dRAX RIC Databus
* A Redis client - which is able to get and set values in a REDIS database, which is used as the xApp Database
* A NATS client - which is able to send commands to the dRAX Databus
* A protobuf client - To be able to generate and parse protobuf messages
* An API client - To be able to send API requests to the dRAX RIC API Gateway
* [OPTIONAL] A NetConf client - To be able to talks NetConf to the cells

The internal workings of the xApp Core can then be implemented as the developer wishes.

The developer has to also make sure to package the xApp Core into a Docker Image.


## 15.2. Helm Chart

The xApp Helm Chart stays practically the same, with just modifying as usual the Docker image that is used for the xApp Core and the version number.




# 16. Appendix: Pushing to Dockerhub



1. One example remote Docker image repository is Dockerhub. Create an account and log in on [https://hub.docker.com/](https://hub.docker.com/). Click on Create Repository and name it &lt;xapp-name>. 
2. Log into the Docker Hub from the command line using:

    _docker login --username=_yourdockerhubusername_ --email=[name@company.com](mailto:name@company.com)_


    Enter your password when prompted. 

3. Check the IMAGE ID of your local xApp Docker image:

    docker images

4. Tag your xApp Docker image by using its IMAGE ID:

    docker tag &lt;image-id> yourdockerhubusername/&lt;xapp-name>:&lt;xapp-version>

5. Push your xApp Docker image to the repository you created:

    docker push yourdockerhubusername/&lt;xapp-name>


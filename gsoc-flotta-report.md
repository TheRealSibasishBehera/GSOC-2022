# Google Summer of Code 2022: Work Product Submission

![image](https://drive.google.com/uc?export=view&id=1f2_C00KcCQ8SjJ6dQCrbmdY2mDzv09Jg)

## Table of contents

 - [Student Developer Info](#student-developer-info)
 - [Project Overview](#project-overview)
 - [Communication and Work Management](#communication-and-work-management)
 - [Primary features/components I worked on](#primary-featurescomponents-i-worked-on)
 - [Issues Faced](#issues-faced)
 - [Future Scope of the project](#future-scope-of-the-project)
 - [Things I learned while contributing](#things-i-learned-while-contributing)
 - [Acknowledgements](#acknowledgements)
 - [Important Links](#important-links)


## Student Developer Info

**Name**: Sibasish Behera ([@TheRealSibasishBehera](https://github.com/TheRealSibasishBehera))  
**Organisation**: [JBoss Community](https://www.jboss.org/)  
**Project**: [Flotta - Reduce energy consumption](https://summerofcode.withgoogle.com/projects/_)

## Project Overview

- Project Flotta is designed to manage containerized workload on small-footprint devices found on the edges of networks by managing the lifecycle of 
  edge-focused operating systems and securing connectivity between K8s/OCP and the edge

- Flotta-device agent is handling all the workloads on small factor SBC which can be located anywhere, where the power cannot be reliable ,Some devices are running on moving devices or powered by solar panels, where power consumption is a key factor.

- For this reason, monitoring the power and performance is important. For this we used diffrent approaches go get a view of things


- Project Flotta is designed to manage containerized workload on small-footprint devices found on the edges of networks by managing the lifecycle of 
  edge-focused operating systems and securing connectivity between K8s/OCP and the edge

- Flotta-device agent is handling all the workloads on small factor SBC(raspberryPi, Nvidia Jetson, etc) which can be located anywhere, where the power cannot be reliable, Some devices are running on moving devices or powered by solar panels, where power consumption is a key factor.

- For this reason, monitoring the power and performance is important. For this, we used different approaches to get a view of power consumption in edge devices


### Components of Power Management

- #### Kepler Monitoring:

	- Kepler (Kubernetes Efficient Power Level Exporter) uses eBPF to probe energy-related system stats and exports as Prometheus metrics 
	- eBPF is a kernel technology (fully available since Linux 4.4). It lets programs run without needing to add additional modules or modify the kernel source code
    You can conceive of it as a lightweight, sandboxed virtual machine (VM) within the Linux kernel
  - It was primarily designed for cluster setup using the cluster API, like k8s 
  - Support for only container setup was added. As project Flotta uses, Podman for deploying edge workloads, a Podman and Cluster interface was set up for that 
  
- #### Powertop Tunings:

	- PowerTOP is a terminal-based diagnosis tool developed by Intel using RAPL protocol that helps you to monitor the power usage by programs running on a Linux system when it 
   is not plugged onto a power source, which makes it suitable for unreliable power sources
	- For integrating it with flotta a Go-based implementation was used, which was later containerized with the help of Docker, and can be deployed as a 
    EdgeWorkload in flotta operated devices 

- #### External PowerMeter Service:
	- Using OS- tools can more or less give accurate information about the energy consumption by the operating system and the CPU.but in cases of             SBCs like Raspberry Pi, and Nvidia Jetson Boards this is not sufficient as there are other factors as well which consume energy comparable to that of CPU computation. Eg. Energy consumption by networking components like Ethernet, GPIO, UART, etc.

         - To overcome this problem and to get the complete power consumption, an external power meter needs to be integrated into the SBC

         -  For this, we would be using Tasmota EU plug V2 by Athom. This is based on tasmota-HLW8032, providing control using MQTT, Web UI, and HTTP.
         
	 - A Go Based Implementation is done for calling the API and can convert the data into Prometheus metrics, Further, it is containerized by which  the Go application can be easily deployed as an EdgeWorkload

- #### Thanos and Graphana Set Up for Observation and Visualisation of stats [On the way ....]:

	- Thanos provides a global query view, high availability, data backup with historical, cheap data access as its core features in a single binary           which is a great way to access the metrics in workloads
	- We are trying to deploy the Thanos receiver and querier outside the edge device , which can be the data center hosting the operator and somewhere else 
	- currently, the testing is done with help of flotta-dev-cli, there are some issues that we found on the way, due to metrics cant getting exposed outside the workload
	- I am trying to work with the team to fix it and have an alternative for that


## Communication and Work Management

- The entire project is hosted on the repository under Project FLotta on Github.
- The procedure for implementation comprised of discussion with mentors over Slack and Google Meet calls
- There were bi-weekly meetings with the Flotta team, where we discussed the objective over implementations, it was a great way to demonstrate our     work as well as see other contributors and project member's works
- There were regular two short meets for unblocking and some code demonstration by mentors, 
- The code review was done mostly on GitHub

## Primary features/components I worked on

 ### Power Monitoring in Flotta
  
  - I addressed the primary need for power consumption monitoring in flotta edgedevices at various level
  - All the features Kepler , Powertop and Powermeter can now be deployed as edgeworkloads , which makes it much easier for user to deploy in the device worker depending on their use case
  - The metrics can be easily exposed both as form of logs as well as stored in the device workers internal TSDB  


### Workloads Metric Management

- Accesing the workloads outside the workloads through metrics is important for monitoring and obseravation as a user
- For this Thanos receiver and querier can be easily set up by user using the configuration provided in the respective repositories of the projects 


### Issues Faced


- **Raspberry Pi**   

     - The idea was to test the workloads developed throughout the GSoC period, in an environment where the power consumption of the CPU components              and other components were comparable
     -   But there was a delay in the delivery due to shortages reasons.
          Further, there was a problem in booting up the desired environment



- **Workload deployment**   

     - the configuration for deploying the PowerMeter and powertop workloads with  Thanos and Graphana was ready but we faced some issues related to            podman which were unresolved previously.
       So we intend to address those issues first, which would make it possible to run workloads with metrics 
  



## Future Scope of the project
- Fixing the workload metric exposure  
- Implementing the power management by turning kernel modules in form of PowerSaving Modes

## Things I learned while contributing

- **New Technology**: In the past, I would solely work on technologies that I was previously familiar with and base all of my projects on them. During GSoC, I learned that we should select technologies/languages/dependencies that properly match our use case, not just those that we're familiar with like Golang, BPF and prometheus metrics query DSL.
- **Large Codebase**: I wasn't used to working with large codebases before, and it took me a while to get used to it.
- Debugging large codes, identifying the cause, and fixing it. Also got to learn about some debugging tools. At the same stage, Kepler was based on BPF, so learning how to debug BPF was a hard thing in this case
- **Importance of Ease in Deploying Applications**:Got to know  a lot about how good documentation articles and blog posts can make the user as well as     developer's experience of using the application so much easier
   Understood using Yaml, Makefiles, and Dockerfiles for making things easier  
- **Writing clean and well-documented code**: Before GSoC, I had never given much thought to code quality. But after working with my mentors, I realized that it is the most significant and vital characteristic of any excellent codebase. 
-  **Importance of Communication**: Before Gsoc I had rarely collaborated with developers from over the world. During GSoC, I understood the importance of communication which is important in expressing as well as acknowledging an idea. I also had a good idea of how developers even from different timezones collaborate to turn an idea into an implementation .

## Acknowledgements
I would like to express my heartfelt gratitude to:

-   All my mentors: Eloy Coto ([@eloycoto](https://github.com/eloycoto)),  Piotr Kliczewski ([@pkliczewski](https://github.com/pkliczewski)),  Gloria Ciavarrini ([@gciavarrini](https://github.com/gciavarrini)), Moti Asayag ([@masayag](https://github.com/masayag))
-   Deepandra Singh([@dpshekhawat](https://github.com/dpshekhawat)) and Ahmad Ateya ([@ahmadateya](https://github.com/ahmadateya)) for being fantastic collaborators throughout this journey.
-   The entire JBoss Community for their welcoming support and assistance.

## Important Links
 
 - [GSoC Project](https://summerofcode.withgoogle.com/projects/#)
 - [GitHub Repository](https://github.com/project-flotta)
-  [PowerMeter Repository](https://github.com/project-flotta/powermeter_integration/)
-  [PowerTop Repository](https://github.com/project-flotta/powertop_monitoring)
-  [Kepler Repository](https://github.com/project-flotta/kepler/pull/1)

THe documentaions are attached at the end of the readme files

     
     

# Google Summer of Code 2022: Work Product Submission

![image](https://drive.google.com/uc?export=view&id=1f2_C00KcCQ8SjJ6dQCrbmdY2mDzv09Jg)

## Table of contents

 - [Student Developer Info](#student-developer-info)
 - [Project Overview](#project-overview)
	 - [Components of Charmil](#components-of-charmil)
 - [Communication and Work Management](#communication-and-work-management)
 - [Primary features/components I worked on](#primary-featurescomponents-i-worked-on)
 - [Other Contributions](#other-contributions)
 - [Future Scope of the project](#future-scope-of-the-project)
 - [Things I learned while contributing](#things-i-learned-while-contributing)
 - [Acknowledgements](#acknowledgements)
 - [Important Links](#important-links)


## Student Developer Info

**Name**: Sibasish Behera ([@TheRealSibasishBehera](https://github.com/TheRealSibasishBehera))  
**Organisation**: [JBoss Community](https://www.jboss.org/)  
**Project**: [Flotta - Reduce energy consumption](https://summerofcode.withgoogle.com/projects/_)

## Project Overview
<div align="center">
	<a href="https://github.com/aerogear/charmil/"><img src="https://drive.google.com/uc?export=view&id=1MYi9drQW2mi3TUdjBqyOM5KPAw90_LVU" alt="Charmil logo"></a>
</div>
<br />

- Project Flotta is designed to manage containerized workload on small-footprint devices found on the edges of networks by managing the lifecycle of 
  edge-focused operating systems and securing connectivity between K8s/OCP and the edge

- Flotta-device agent is handling all the workloads on small factor SBC which can be located anywhere, where the power cannot be reliable ,Some devices are running on moving devices or powered by solar panels, where power consumption is a key factor.

- For this reason, monitoring the power and performance is important. For this we used diffrent approaches go get a view of things


### Components of Power Management

- #### Kepler Monitoring:

	- Kepler (Kubernetes Efficient Power Level Exporter) uses eBPF to probe energy related system stats and exports as Prometheus metrics 
	- eBPF is a kernel technology (fully available since Linux 4.4). It lets programs run without needing to add additional modules or modify the kernel source code
    You can conceive of it as a lightweight, sandboxed virtual machine (VM) within the Linux kernel
  - It was primarily designed for cluster setup using the cluster API, like k8s 
  - Support for only container setup was added . As project Flotta uses , Podman for deploying edge workloads ,a Podman and Cluster interface was setup for that 
  
- #### Powertop Tunings:

	- PowerTOP is a terminal-based diagnosis tool developed by Intel that helps you to monitor power usage by programs running on a Linux system when it 
   is not plugged on to a power source , which makes it suitable for unreliable power sources
	- For integrating it with flotta a Go - based implementation was used , which was later containerised with help of Docker , and can be deployed as a 
    EdgeWorkload in flotta operated devices 

- #### External PowerMeter Service:
	- Charmil offers its own command-line interface (CLI) that allows developers to construct their new Charmil project with additional support for other templates, allowing them to focus on more essential aspects of their project.

	- This is the easiest way to incorporate Charmil into a project. 

	- As of now, the Charmil Generator CLI offers 3 commands:
		- **init**: Initializes a project boilerplate by using the Charmil Starter Template.
		- **add**: Adds a new command into the CLI.
		- **crud**: Helps developers generate CRUD commands for their CLI.

- #### Thanos and Graphana Set Up for Observation and Visualisation of stats [On the way]:

	- 


## Communication and Work Management

- The entire project is hosted on the [Charmil](https://github.com/aerogear/charmil/) repository under the Aerogear organization on Github.
- Before implementing any new feature, we used [Github Discussions](https://docs.github.com/en/discussions) to discuss our approach with the mentors and other contributors.
- In the Aerogear discord server, a dedicated channel was created for this project to settle any doubts, ideas, and comments for speedier coordination.
- Some live coding/discussion sessions were conducted on the Aerogear discord server with the mentors and other contributors.

## Primary features/components I worked on

 ### Charmil Generator CLI
- **Charmil CRUD generator command**:
	- With the help of the  `charmil crud`  command, developers can eliminate a lot of boilerplate in CLIs containing multiple services that perform standard CRUD operations.
	- Using a set of pre-defined templates, this command generates CRUD command packages in the directory specified by the  `crudpath`  flag as well as its corresponding language file in the directory specified by the  `localepath`  flag.  
	- These generated files can then be modified by developers to fit their own needs.

### Charmil Core
- **Config Management Package**:
	- The Charmil Config package offers a convenient mechanism for both host and plugin developers to manage configurations in their CLI applications.
	-   Helps in maintaining all available configurations in a single, centralized local config file.
	-   Provides the host CLI developers with a set of methods to read/write configurations from/to a local config file.
	-   Provides the plugin developers with functionality to load/save their CLI configurations from/to the host CLI local config file with ease.


## Other Contributions

- Some refactors on Starter templates
- Feature Flags (Idea discontinued)
- Making the RHOAS CLI repo use Charmil Validator
- Provide POC on how Charmil can be used to integrate case study repositories

## Future Scope of the project
- Implementing the Charmil Command Registry component
- Investigating shared authentication between host and plugin CLIs

## Things I learned while contributing

- **Learning new technologies on a regular basis**: In the past, I would solely work on technologies that I was previously familiar with and base all of my projects on them. During GSoC, I learned that we should select technologies/languages/dependencies that properly match our use case, not just those that we're familiar with. 
- **Working with huge codebases**: I wasn't used to working with large codebases before, and it took me a while to get used to it.
- Debugging large codes, identifying the cause, and fixing it. Also got to learn about some debugging tools.
- **Developer Workflow**: Got familiar with the iterative process of first `Picking an issue` ⮕ `Having discussions around it` ⮕ `Implementing it` ⮕ `Creating a PR` ⮕ `Asking for Reviews` ⮕ `Applying suggestions based on reviews` ⮕ `Getting it merged`.
- Adapting with contributors across different timezones.
- Understanding, reviewing, and modifying others contributors' code.
- Implementing error handling wherever possible in code, to assist users in determining the source of any issue.
- **Writing clean and well-documented code**: Prior to GSoC, I had never given much thought to code quality. But after working with my mentors, I realized that it is the most significant and vital characteristic of any excellent codebase. 

## Acknowledgements
I would like to express my heartfelt gratitude to:

-   All my mentors: Wojciech Trocki ([@wtrocki](https://github.com/wtrocki)),  Dimitri Saridakis ([@dimakis](https://github.com/dimakis)),  Jack Delahunt ([@jackdelahunt](https://github.com/jackdelahunt)), JJ Kiely ([@jjkiely](https://github.com/jjkiely)), Ramakrishna Pattnaik ([@rkpattnaik780](https://github.com/rkpattnaik780)) and Maksym Vavilov ([@makslion](https://github.com/makslion)), for always believing in me and devoting time to provide impeccable guidance and motivation while contributing.
-   Ankit Hans ([@ankithans](https://github.com/ankithans)) for being an amazing collaborator throughout this journey.
-   The entire JBoss Community for their welcoming support and assistance.

## Important Links
 
 - [GSoC Project](https://summerofcode.withgoogle.com/projects/#6059989371715584)
 - [GitHub Repository](https://github.com/aerogear/charmil/)
 - [Documentation](https://aerogear.github.io/charmil/docs/)
 - Links to Work Done in the project:
	 - [aerogear/charmil](https://github.com/aerogear/charmil)
		 - Commits: [Link](https://github.com/aerogear/charmil/commits?author=namit-chandwani)
		 - Pull Requests: [Link](https://github.com/aerogear/charmil/pulls?q=is%3Apr+author%3Anamit-chandwani)
		 - Discussions: [Link](https://github.com/aerogear/charmil/discussions?discussions_q=author%3Anamit-chandwani)
	 - [aerogear/charmil-starter](https://github.com/aerogear/charmil-starter)
		 - Commits: [Link](https://github.com/aerogear/charmil-starter/commits?author=namit-chandwani)
		 - Pull Requests: [Link](https://github.com/aerogear/charmil-starter/pulls?q=is%3Apr+author%3Anamit-chandwani)
	 - [aerogear/charmil-host-example](https://github.com/aerogear/charmil-host-example)
		 - Commits: [Link](https://github.com/aerogear/charmil-host-example/commits?author=namit-chandwani)
		 - Pull Requests: [Link](https://github.com/aerogear/charmil-host-example/pulls?q=is%3Apr+author%3Anamit-chandwani)
	 - [aerogear/charmil-plugin-example](https://github.com/aerogear/charmil-plugin-example)
		 - Commits: [Link](https://github.com/aerogear/charmil-plugin-example/commits?author=namit-chandwani)
		 - Pull Requests: [Link](https://github.com/aerogear/charmil-plugin-example/pulls?q=is%3Apr+author%3Anamit-chandwani)
	 - [redhat-developer/app-services-cli](https://github.com/redhat-developer/app-services-cli)
		 - Commits: [Link](https://github.com/redhat-developer/app-services-cli/commits?author=namit-chandwani)
		 - Pull Requests: [Link](https://github.com/redhat-developer/app-services-cli/pulls?q=is%3Apr+author%3Anamit-chandwani)
     
     
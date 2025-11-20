# OSS - Japan

## 08/12/25

### Intro talks
#### Intro keynote
A general talk about the state of the Linux foundation and why open source was important to AI. The speaker made comparisons to how the performance of the linux kernel has improved significantly due to everyone having a common interest in improving it due to its wide use. Using primarily open source software in the AI infrastructure stack would have a similar effect. He also added some other interesting Ai related points such as how he thought we were not in an AI bubble so much but potentially an LLM bubble which seems to be a common sentiment. He also pointed out that the main bottle neck in scaling out all this AI infrastructure is actually the power requirements which is pretty dystopian if you ask me. 

<!-- - Infastructure bottleneck in scaling AI due to power and hardware
- Not in Ai bubble but more likely in LM bubble
- talked about how opensource important in Ai due to the software infrasturce stack, e.g pytorch and down
    - companies can work together to improve model performance -->
#### Agones
A short talk about Agones, a GCP kubernetes tool optimised for running video game servers. In summary, kubernetes treats pods as stateless microservices which is not a great model for a video game server. Agones extends the kubernetes API for keeping state better. The speaker didnt go into detail as to how exactly this differs from regualr kubernetes. It seems like having persistant volumes for the pods would work fine for this purpose? He went on to talk about Arc Raiders as an example of a video game running on this new service.

<!-- - google project for optimising kubernetes for running game servers
- k8s treats services as stateless microservices, agones extends k8s api for keeping state better. Used Arc raiders as a case study
    - arc raiders world just k8s pods using agones
- agones donated to cncf
- didnt go into details as to how agones is different from regualr k8s. Can state not be managed in persistant volumes? -->

#### AGL SoDev
An overview and update on the recent improvements of the AGL SoDev platform. This is a Linux based OS and development platform designed for use in automotive.
The speaker started off by talking about the transition in automotive from a hardware to a software based product. This was compared to the mobile phone industry where phones orignally had very custom hardware and software desgined specifically for them but they have transitioned to a generic platform where that developers can make applications for wihtout having to customise them to specific hardware.

He then went on to go over the definition of an SDV the main points of this being:
- The many ECUs of the vehicle have been replaced with software running on a single processor
- Hardware and software is decoupled e.g possible to develop software without knowledge of the hardware.
- Virtualised devices now replace physical ECUs.
- The introduction of Unifed HMI. From my understanding a kind a Unified API/data format to allow data from any system in the car to be easily diplayed anywhere else.
- Continuous delivery via over the air updates.
- Decoupling of the software and hardware development lifecycles.

Next he then gave an overview of the AGL SoDev platform which is a linux based OS/development platform that fills the above criteria. He finished by talking a litle bit about the EU cyber resiliance act and how AGL SoDev would satisfy it.


<!-- - talking about the move from hardware to software defined
- compared to mobile phone industry
    - started very hardware based, moved to decoupling of hardware and software
- SDV:
    - moviing from many ECUS to software running on a single processor
    - decoupling of hardware and software
    - virtualised devices
    - unifed HMI: system to display info from any system
    - continuous delivery via OTA updates.
    - decouple software and hardware lifecycles
- AGL SoDev 
    - single platform to allow software development for the car
        - diferent domains for different parts of the car e.g zephr for saftey critical, android for IVI
- talked about EU Cyber Resilance Act and why AGL SoDev
    - SBOM and tracablitiy 
    - easy to patch systems -->

#### Accelerating SDV through open source virtualisation
This talk covered similar ponts to the last, namely highlighting the benefits of using viirtualised hardware all running on a single OS as opposed to using physical ECUs. They were specifically talking about the VirtIO technology and how it allows software to be moved from real hardware to a virtualised system in a binary compatible manner. 
The second key point was about corporate OSS adoption and how companies should contribute to themselves or better launch there own OSS projects.

<!-- - talked about speed of product development. SDVs decrease development time
- hardware approach to software
    - hardware:
        - customized hardware
        - software developed to make it work
        - long lead times
    - software:
        - choose latestest hardware, can start developing software straight away
        - shorter dvelopment times
- VirtIO key to software first development
    - eliminate reliance on specific hardware by always developing software for virtualised hardware
    - software can be moved from running on real hardwar to virtualised system in binary compatable manner
- talked about car UX
    - moving from multiple displays to single display. 
    - Can do agile development with unified HMI,
    - cockit UX design is constrained by the hardware in non SDVs
- talked about model for oss adoption
    - companies should contribute to OSS themselves or launch their own projects  -->

#### What is learning in AI
This was a talk discussing how AI is changing the way we learn, escpecially for the younger generations. The speaker started off talking about the 46% drop in entry level tech hiring in the UK and how this is due to AI automating entry level tasks (or at least that is what the employers beleive...). This is quite worrying as it is effecitivly removing he apprenticeship pipeline which will only lead to a lack of experienced developers in the years to come. She went on to say how education needs to shift from teaching statc knowledge to having more of a focus on dynamic thinking. 
The next point raised was that as well as changing education we should try to create what she called Socratic AI which tries to ask people more questions to aid the thinking process rather than just spewing out answers with varying degrees of accuracy. Finally, she finished by talking about the use of socratic AI in the OSS pipline, specifically how it could be used to onboard new maintainers to a project.
Although I dislike the hype around AI I think disscussions like this are very important as the technology is likely here to stay and so we need to be able to use it effectivly.

<!-- - tech professionals need to learn annualy to stay competetive
- 46% drop in entry level tech hiring in uk
- AI automating entry level tasks and removing the apprenticeship pipeline
- AI overwellming OSS maintainers
- CS deparmens moving from teaching static knowldege to dynamic thinking
- AI good at boilerplate and code completion but strgulles with security and context
    - struggles for languages or systems where there isnt alot of data
    - Ai is jagged intelligence - very unpredictable
- loss of people knowing why things work
- Instead we should choose AI systems that ask us questions rather and just provide answers
- talked about programs to help tech programming to people without access to resources
    - people benefitted from the programs went on to run their own in other countries

- education needs improving, Promoted change to scratch to train AI. Helps to teach how Ai works
    - kids who trained their own AI were more skeptical on its intelligence
- there was a demo for a copilot additon to this which asks more questions to the user rather than just providing answers
    - this seems to defeat the purpose in my oppinion
- Speaker conveyed that AI used in the correct way is helpful for learning

- Socratic AI in the OSS pipeline
    - mentor new comers
    - could be an automated maintainer
- We need not just Open source models but open data -->

### Running CFS on Zephyr: A Modern RTOS for Space Missions
This gave an overview of spacecraft architecture as well as the Core Flight System [CFS](https://github.com/nasa/cFS), an open source libary developed by NASA to create a reusable code base for use in spacecraft. They described the spacecraft as an ultra remote distributed system with real time islands and briefly talked about some of the communication standards CFS supports such as SPP (space packet protocol), COP-1 and CCSDS file delivery protocol. In particular the speaker talked about the OSAL component of the CFS and the work done to add support for Zephyr. OSAL is a platform abstraction layer that provides an API to abstract away OS specific logic. It uses CMake for build configuration which made it easy to intergrate with Zephyr and uses C defines for build time variables (however the speaker claimed only a little work was needed to get it working wth Kconfig). He briefly disscussed some of the work that still needs doing to make OSAL work with Zephyr. For example OSAL has fixed task priorties where as Zephyr allows for alot more customisation.
Overall I was quite suprised at the amount of abstraction and standardisation in software for spacecraft. I guess I just assumed that due to the presicion needed everything would be alot more customised specifically for the hardware.

<!-- - space cubics: create sbc for spacecraft
- cfs - core flight system: reusable for running different software
    - developed by nasa
    - public on github
    - used on lots of spacecrafts
    - multiple layers
         - CFE - top level provides core services, task maangemnt etc
         - application layer - example:DSU application:
                                - controls things like Data store and filesystem
                                - interestigly only the file manager application can acess files
         - Platform abstraction (OSAL):
            - os abstracion API - supports RTEMS VxWorks Linux Zephr (not upstreamed
            - uses CMake for build configuration (easy to intergrate with Zephr)
            - uses C defines for build time configuration variables (not Kconfig but can be easily ported)
            - OSAL is an API to abstract away the OS
            - zephr has configurable task priorities however in OSAL its fixed so may need extra thougt when using wih Zephr
             have just started adding zphr support to OSAL so still needs work
             - showed how OSAL build for zephr 
            - gave a demo of builidng some application code for zephyr and OSAL using west (Zephyr build system)
- went over aritechure of a spacecraft
    - xpdr (transponder) -> command and data handling -> other systems
    - need to carefully cmeasure temperature

- there is a standard for spacecraft communication standards CCSDS
    - CFS supports CCSDS standards
        - SPP space packet protocol
        - COP-1
        - CFDP - CCSDS file delivery protocol

- spacecraft is an ultra remote distributed system with real time isalnds

- there was a question about if the filesizes were still small using zepyhr but they could didnt have the figures
- I was suprosed there was so much abstraction in space craft development -->

### Applying Zephyr RTOS in Spacecraft Development
This talked about a robotics project for JAXA to deploy a small robot used for streaming video around the inside of the ISS. The Robot looking like the Jedi training orb and was propelled by fans with video IO. The speaker went on to give an overview of the hardware which consisted of a Rasberry Pi for running the multi media service, an stm32g4 for the main control board and an stm32f4 for the power management system. He went on to talk about the software architecture of the control system. This was running zephyr and the main command handler ran the following loop: pre-process -> state evaluation -> communication -> idle -> feedback control. He also briefly went over their develpopment setup which was OpenOCD for debugging and west (the zephyr development tool) for flashing. Finally, he showed a short video of the robot in action on the ground and said they hope Zephyr will become a standard OS for space devices. 
Speaking as someone who doesnt know much about making robots for space, it looked pretty much like a consumer electronics project. The robot operates inside the ISS so I suspect they didnt really need to worry about radiation hardening and from what the speaker was saying it doesnt even look like they made a custom PCB for the project just attached together some dev boards you can buy easily. Im sure there is alot more to the project e.g flight stablity of the robot and things like that, but the talk did make me wonder if Codethink could do an equally good job on a project like this.

<!-- - gave some examples of robotics in space (namely uses on the ISS)
- specificaly gave an example of a robot the company is working on on for streaming video around the inside of the iss
    - propelled by fans
    - has audio and video IO with real time trnasmission cabailties
    - looks like the jedi training droid
 - 
 - hardware overview:
    - multi media service - rasberyy pi
    - control board - stm32g4 (32kb)
    - power managgement - stm32f4re
- sofwtare architecture
    - control system running zephyr:
        - command handler has following loop:
        -   pre-process -> state evaluation -> communication -> idle -> feedback control
- went over their development setup
    - ubuntu, vscode, openocd, west to build 
- it was reassuring to see that the setup wasnt to dissimialr to home projects
- showed some videos of the robot working (on the ground)
- they hope zephyr will be a standard OS for space devices

- even though it is tecnically in space the robot showcased is designed to operate inside the iss so could ignore alot of the standard spacecraft development considerations, was bassicaly a standard electronics project. -->

### Enhancing Your Gaming Experience on Linux With Sched_ext
A talk about improving gaming performance on linux by using a custom scheduler. A very important topic. The talk began with an overview of gaming on linux stating how there is a signifcicantly less performance compared to Windows due to un-optimised scheduling. They next went over the role of the scheduler and what needs to happen to improve gaming performance e.g can the scheduler make descions to help gaming specific tasks. Three ways raised were,
- latency critcal tasks need to be scheduled earlier
- a task should run long enough for warmer cache but not hog all the  the CPU time.
- not all tasks are power hungry. Some can run on power efficient cores (useful on handheld systems like the steam deck)

To explain the speaker said that most gaming specific workloads have the following characteristics,
- They are communication intensive: multiple tasks collaborate to do finish a single task.
- Short running: most tasks run for less than 1ms.
- Semi periodic: a task does similar work again and again making it very predictable.

As an example of multiple tasks collaborating, the speaker showed an example task tree from the game CyberPunk 2077 making it clear that the tasks are tightly linked. They then went on to talk about how you can create your own scheduler in linux with sched_ext which enables custom scheduling policies to be implemented in a BPF program. The custom scheduler then consists of three parts, the sched_ext core in the kernel which provides hooks to redefine scheduling policy, the BPF part which defines the custom scheduling policy (sched_ext core calls a call back written in BPF when a custom policy is needed) and finally a user space part of the scheduler which loads the BPF code and is ussualy quite thin.
Next, the speaker introduced their own gaming optimised scheduler titled LAVD (I cant remember what it stands for) which satisfied the above above condiions for optimising gaming specific workloads. They defined a task as latency critical if the wakeup frequency and wait frequency of the task is highand therefor the scheduler will assign these tasks a tighter deadline. The scheduler also aims to scheduler everything in a fixed interval. The talk finished with a demo of Space Marine 2 running on the steam deck using both the standard scheduler and the new one. The performance difference was very noticable.


<!-- - schedular framework - write a custom schedular
- talked about steam os and the platforms it runs on
- steam os can run x86 windows games on linux
- averge 41 fps on linux (okay but not great for pc gaming)
- however low 1 percent can be 5 fps which is very bad
- energy consumption is importatn since alot of steam os platforms are battery powered
- somedevices use hybrid processors
    - little core - energy efficent but slow
    - big core - high performance
 - scheduler does 3 things
    - which task should run first
    - for how long
    - where should task run (whch cores)
- for gaming performance
    - latency critcal tasks need to be scheduled earlier
    - a task should run long enough for warmer cache but not hog the cpu
    - not all tasks are power hungry some can run on power efficient cores
- gaming optimised scheduler
    - workload: can the scheduler make a descion to help gaming specific tasks
    - hardware: hybrid proseccors are popular so a scheduler should make better use of hybrid schedulers
- gaming workloads:
    - communication intensive: multiple tasks collaborate to do finish a single task
    - short running: most tasks run for < 1ms
    - semi periodic: a task does similar work again and again (very predictable)
- tasks tightly linkedin graph
    - showed example task tree from  cyber punk

- sched_ext is now merged into linux kernel - enables scheduling policies to be implemented in a BPF program
    - similar to kernel module programming but bpf guarentees safety (e.g no memory bugs)
    - 3 parts:
        - sched_ext core in kernel - provides hooks to redefine scheduling policy
        - BPF part - defines custom scheduling policy, shed_ext core calls a call back written in bpf when a custom policy is needed
        - user space part of a custom scheduler - load and set up the bpf code, ussualy thin and written in rust

- speakers solution for a gaming optimised scheduler:
    - LAVD (new scheduling algorytm for gaming workloads)
    - tasks scheduling is represented as a task deadline 
        - deadline is virtual, represeting relate urgency

- - decde virtual deadline:
    - schedule latency critical task first
        - define a task as latency critical if wakeup frequency and wait frequency of tasks is high
            - high wake up freq -> important producer
            - high wait freq -> important consumer
        - if both are high then assign tighter deadline

- deciding tasks time slice:
    - want everything to run in a fixed interval
    - time slice = f(number of runnable tasks)
        - more tasks -> shorter time slice for each

- deciding which core to schedule on
    - - use a table of cpus suggested for different work loads
        - e.g for a particular qualcomm snapdragon low load -> use little cores, medium -> use perfromance cores first then little cores, very high -> use prime core
- showed a demo of spacemarine 2 on steam deck with and without the custom scheduler and you could easily see the performance difference

- interesting question about if theres a difference with the custom scheduler when running games on wine vs natively: speaker said there wasnt much difference apart froma small issue about mutexes not being released when tasks got scheduled out
- question asked about how energy efficency was measured, info just comes from the kernel -->

### hkml: Mailing Tool for Simple Linux Kernel Development
This was a talk showing off a new tool for making it easier to submit linux kernel patches. The talk first went over the current process for submitting a change and highlighted why this can be confusing for newcomers. He then proceeded to demo the [new tool](https://github.com/sjp38/hackermail) which is a CLI tool for managing changes via a mailing list. The tool requires minimal setup and generally makes submitting changes via a mailing list alot less painful although you do still need to setup `git send email` which isnt mentioned in the docs. As someone unfamiliar with the patch submission process for the kernel this looked very appealing.

<!-- - kernel developent process:
    - contributer -> mm/damon/maintanier -> mm/Maintainer -> linus torvaldsnew tool
    - changes passed up the maitainer tree until it gets merged
    - process:
        - git commit -> git request-pull
        - find relvant maintainer and send to them with `git send email`
    - issues:
        - need to use own mailing tool as gmail doesnt always send
        - need to subscibe to whole mailing list - gets very busy so hard to find your reply
- hkml: tool to reduce pain 
    - minimum setup and resources
    - used by DAMON maintainer
    - github hacker mail
- demo of the tool - just generlly makes it alot easier to interact with the mailing list
- mentioned at the end you still need to setup `git send email` which isnt mentioned in the docs -->

### TinyML at the Edge: Deploying and Optimizing AI Workloads on Zephyr RTOS
The talk covered why Zephyr is a good platform for deploying AI models on embedded systems. The main takeaways were that it abstracts away hardware specifics and has alot of support for AI runtimes such as Tensorflow lite, MicroTVM, EMlearn and Kenning Zephyr runtime which allows developers to focus on getting their AI models working rather than having to deal with embedded software issues. Zephyr also has useful features such as
- LLEXT (linkable loadable extensions) which enables hot swapping of application logic or AI without reflashing the device
- OTA updates
- Hot swapping: update certain parts of software while still running (application of LLEXT)

He finished off the talk by going through the steps to get Tensorflow lite running on a system with Zepyhr and finally talked a bit about Zephelelin, an AI profiling libary useful for finding performance bottle necks.

<!-- - zephyr - rtos for resoucre constrianed devices with standardized drivers 
- goodfor ai:
    - deterministic
    - long battery life
    - easy itegration
    - dynamic extensions
    - low-power operation
- features:
    - modern build system (Cmake) easy to build for the developer
    - device tree based hardware description
    - secure boot
    - has testing libaries
    - module and dpendacy managment (west)
    - easy to use HAL API
- can use the same models across boards
- core runtimes available:
    - Tensorflow lite
    - MicroTVM
    - EMlearn
        - converts models to sci kit or pure c for use in zepher, very fast, good for small models
    - Kenning Zephyr runtime
- showed table comparison of the different ai runtimes
- showed how to use TFLM in zephyr. Steps:
    - configure zephyr with some config varbles and use `west config`
    - add model data
    - implement inference
    - flash board with west

- kenning is an open source auto-ML module to automate the process of AI optimization and deployment
- Zephyr allows for
    - LLEXT (linkable loadable extensions) enables hot swapping of application logic or AI without reflashing the device
    - OTA updates
    - Hot swapping:
        - direct application of LLEXT
        - allwows for upadting certain parts of sofware while its still running, needs good state management

- Zephelelin
    - Zepyhr AI profiling libary to find bottle necks
    - uses Tracing subsystem
    - thread tracing API
    - memory manegment -->

### OF-nodes, Fwnodes, Swnodes, Devlinks, Properties - Understanding How Devices Are Modeled in Linux
This was a very interesting talk going over the structure of devices in the linux kernel and the do and donts when adding a new driver. The speaker started with some history of the kernel and how devices were orignally modeled with board files which was eventully switched to the device tree model. This seperated out te hardware description from the logic and allowed for more resuable structs as previously each driver would define its own custom structure that board files would fill. The talk then covered the various different structs for modeling devices in the kernel e.g OF-nodes, ACPI nodes and Software nodes as well as how the relationship between the bus and devices are described in the code. He finally gave a quick programming guide on which structs to use when creating a new driver. The key points were,
- for `struct device` use `device_property_read_xyz()` to fetch device information if you dont need to parse other nodes
- use fwnode APIs when the device is associated with a firmware node that has children with important information
- use low level APIs when you need to iterate over all properties of an OF-node or an ACPI device node 
- try and use this order when choosing a device struct  device_ -> fwnode -> of_ -> swnode_ -> acpi_node

<!-- - devices
    - struct device
    
- drivers
    - struct device_driver
- bus
    - bus_type
    - match driver to device
    - bus can be a deivice as well

- each bus has a list of drivers and devices
- bus tries to match a device from the list of drivers `bus.match`
- once matched call `bus.probe` , if succesful then device is attached to driver, can also return ERPOBE_DEFER - could be missing resources
- two categories of devices:
    - some can be matched dynamically some cant

- Platform Bus
    - platfrom devices are non discoverable
    - platform devices can be described in firm ware in different ways
    - linux needs the map of registers and clock but need to make driver portable so need to pass configuration to the kernel to configure generic driver
    
- before device trees they used board files
    - strict ordering with no parralization
    - initialisation in c code, mixing hardware description with logic
    - hand coded dependecies
-   - `struct resource`
    - each platfrom driver would define its own custom structure that board files would fill

- now have devicetree
    - data structre describing hardware
    - defines both text and binary format
    - dtc tranlates between the two
    - tree is parsed and represetned in memory as a directed graph of `struct device_node`
    - linuxhas set of fuctions to read/traverse the device tree nodes

- can generalise device nodes to firmware nodes
    - OF-nodes
    - ACPI nodes
    - Software nodes - defined statically and registered at runtime, have parent child relationship and can reference other nodes
    - these all implement struct fw_node_handle
    - allows device drivers to use the same fucntions to access all kinds

- a device can have properties of both OF/ACPI-node and software node
- programming guide
    - for `struct device` use `device_property_read_xyz()` if you dont need to parse other nodes
    - use fwnode apis when device is associated with firmware node =that has children with important information
    - use low level apis when you need to itaerate over all properties of an OF-node or an ACPI device node or 
    in short try and use in this order: device_ ->fwnode -> of_ -> swnode_ -> acpi_node -->


## 09/12/25

### Day 2 Intro talks

<!-- #### How FINOS is delivering value to financial services
- Hightlighted how financial services are coming round to using open source
- contributing gives faster time to market and is cheaper todevelop since not all the work is on one company
    - improved resiliance as open source hashigh security standards
- talked about FINOS - group encourging the use of open technologies in financial services

#### OpenSearch: From code to community
- benefits off joining linux foundation

#### Hondas OSPO Challenge
- talked about the aritecture honda want to have in an SDV
- stack:
    - Eclipse SDV -> AGL -> communication and application layers
- talked about how they started wth the IVI system and then expanded to encompassing ECUs
- highlighted how honda is driving open source development in the company

#### Fujitsus
- several projects
    - CDI Dynamic hardware scalingby k8s - scaling up/down number of attached GPUS in an AI infrastrucure platform
    - Fujitsa-monaka arm processor  - enables  confidential computing , lots of security features good for AI
    - Pushing open source uses -->

#### Keynote: Linus Torvalds, Creator of Linux & Git, in Conversation with Dirk Hohndel, Head of the Open Source Program Office, Verizon
The talk alot of people were waiting for, a short Q&A with the big man himself. He was first asked about kernel 6.18 highlights to which he said its mainly just fixes and general clean ups as the kernel is really just a bunch of drivers for hardware and that reliability is always the priority. He was asked to explain what he actually does in a merge window. On average he said he gets about 120000 commits to review and fix merge conflicts for. He was asked what some of his pet peeves were with kernel submissions to which he replied,
- PRs on a saturday before the merge window closes
- He runs the latest kernel on all his machines and always finds bugs which shouldnt happen, sends angry emails to maintainers
- He doesnt like when people dont ackowledge bugs they have introduced, escpecially when a bug is introduced as a consequence of fixing a different one. He emphasised that dealing with a known bug is much better than introducing new ones.

After this he apologised to everyone in the room for hurting anyones feelings on the mailing list which I thought was nice.
He was finally asked about his thoughts on AI to which I thought he had a very balanced approach to. he said he hates the hype around AI but thinks its very useful as a tool for reviewing code which was a suprising take. He gave an example of a tool that found a kernel bug in a PR that he also raised as well as adding another suggetsion which he was very impressed by. Finally, he compared AI now to the state compilers were in when he first started programming but thought that the productivty increases from AI are going to be no where near the gains that compilers have provided for developers.

<!-- - kernel 6.18 highlights
    - more of the same linus likes boring just patches and clean ups
    - kenrel is just drivers for hardware and needs to be reliable
- merge window
    - linus takaes all the new code from the maintainers, theres no rule they just decide that its ready when its ready
    - asked what does linus do, - he said he doesn tdo any coding he just reviews the prs from the maintainers, he gets about 12000 commits in one merge window (a few hundred prs) he does all the merging and fixes all the merge conflicts himself (god damn)
- pet peeves
    - prs on a saturday before the merge window - sometimes tells them to 
    - he runs the latest kernel on all his machines and always finds bugs which shouldnt happen, sends angry emails to maintainers
    - doesnt like when people dont ackowledge bugs they have introduced, wouldrather have predictable bugs, cant fix on ebug and introduce another
- said sorry to people for hirting there feelings
- hates the AI hype
- thinks Ai is much better as a tool for reviewing code rather than writing code
- he gave an example of a tool that found a bug that he also raised and it added another suggestion
- comapred ai now to compilers when he first started programming however said that the real 1000x increase in programming was the work on compilers

- very balanced take approach to AI

- why are no regressions in the kernel so hard
    - dont want applications to rely on bugs that break others
    - have to remebr a change isnt jus fixing your own bug it could be breaking things other applcations rely on -->
    - 
### Zephyr: Learnings From Working on Safety Certification in the Open
A talk on trying to get the Zephyr kernel certified with IEC 61508-3. The talk covered what was nessecary to obtain this certification for Zephyr. Some of the key requirements stated were,
- Need to say how Zephyr is going to be used
- provide a definition of the safety claims e.g need to make the documentaion good enough
- tracability of code
- provide evidence that Zephyr handles critical failures
- need to create a safety manual

It was noted that some of these are difficult since Zephyr is an open source project. Specificaly it was brought up that the certification expects the requirements of the project to be clearly written down however, as this is an open source project, this hasnt really happened. She went on to talk about some of the lessons they had learned from a first attempt at getting certified. Some of these were,
- all enhancements need to go upstream
- need general agreement from maintainers on methodology and processes
- need open community participating ton a larger scale
- need to make tooling more developer friendly
- should use a static analysis tool for checking and enforcing coding guidelinesadd

The talk concluded with the statement that the work needed for certification is almost done and there is alot of work on going to document requiements for the project.
Ben Dooks asked whether the certification process itself needs to change to be more appropriate for OSS projects. The response was yes and Trustable was mentioned as an option.

<!-- - rtos built with security in mind
- saftey - freedom fromunacceptable risk of physical injury or of damage to health of people
    - should it fail predictably
- opensource is sometimeslacking in requirements
 - zephyr kernel aiming for IEC 61508-3
    - need to say how zephyr is going to be used
    - definition of saftey claims -> need to make the documentaion good enough
    - tracability of code
    - try to use the existing tests as safety evidence
    - need evidence that zephyr handles critical failures -> can do through the traceability
    - need to create a safety manual
- have made the development processauditable
- first attempt lessons:
    - all enhancments nede to go upstream
    - need buy in from maintainers on methodology and prcesses
    - need open community participating to scale
    - want to use develpoer friendly tooling
    - have static analysis tool for checking coding guidelines

- theres now a saftey committee for platinum members anda safety group fro anyone
- zephyr has publicly documented coing standards similar to misra c rules

- difficult part: requirements and tracability
    - open soruce so didnt start with requirements
    - why dowe ned this
        - can reason as to where exactly an issue arose e.g coding or requirements

- summary: its almost there
    - requiremnts are in progress
    - need to do the aboce steps -->

### Zephyr‑Based VIRTIO Backend on Xen: Toward Open Source Functional Safety
This talk went over the open source hypervisor Xen and how it could be used along side Zephyr in automotive. The speaker started with some history on Xen, specifically how it was used in early versions of AWS EC2 and then went on to talk about the use of Zephyr in a car. The main points beng that many ECUs in the car run an RTOS escpecially if they are safety critical and the current trend is moving individual ECUs to run on a single hardware platform via virtualisation. He talked about how Zephyr running on Xen with VirtIO would be a perfect system for this and how they would like to replace a single Linux instance with multiple Zephyr instances using the virtio backend to provide direct access to the hardware. The talk went into some detail about the work the speaker had done to Xen to better support running Zephyr and highlighted some of the design issues with the new change. Some of these were,
- Xen can assign memory to the VM by page however if multiple peripheral registers are mapped to the same page it gets complicated
- Zephyr drivers set pin and clock control when initializing which I assume now needs to be done by Xen (I missed the detail on this part)
- difficulty in trying to use chip specific security modules e.g arm trustzone

The speaker finished by talking about some future improvements they would like to add to Xen. The main one was that Arm interrupt controllers support interrupt injection into VMs. Adding support for this to Xen would be a big performance improvement as interrupt handling via Xen has alot of overhead.


<!-- - xen is an opn source hyervisor
- used in early verisons of ec2 but now used in ebmedded systems
- virtio
    - paravirtualised device spec 
- uses two ring buffers , frontned (guest), backedn (host) taht pass data between each other
- Automotove
    - many ecus use an rtos
    - especially ecus bound by saftey constraints
    - want to change arch from individual ecus to all run on single ecu and speration using virtualization
- zephyr stills needs saftey cert - tryning to catch up with AUTOSAR which has saftey certification
    - can use xen as a virtio hypervisor
- went over SoDev arch 
    - lowest level operatig system running xen as hypervisor wih all the different ecus virtualised on top, however virtio allows virtualisation to use the underlyng hardware directly
- want to replace underlying linux instance with multiple zephyr instances using the virtio backend
- new chnage
    - shared guest memory via xen grant tables
    - xen hypercall handles writes to MMIO registers and an interrupt is raised to acknoledge the request
- still needs wkr for 
    - support for vrious vritio devices
    - supporting different hypervisors
    - support virtio-gpu
    - better performance

- design problems with new change
    - difficult to split hardware functions across multiple domds
    - xen can assign memory tovm by page, if multiple peripheral registers are in the same page it gets complicated
    - zephyr driver sets pin and clock control when initializing
    - difficultto trying to use chip specific security modules e.g arm trustzone

- furute plans for xen:
    - arms interrupt controller supprts interrupt injection into vms (performance improvment). Could add support for this in xen -->

<!-- ### Linux User Namespaces: A Blessing and a Curse (14:00 B1 4F)
- linux name spaces
    - mount namespace
        - files systems mountde under root under different dir
            - can create namespace with copy of mount tree 
            -  /chroot, useful for snaboxing filesystems, 
            - containers instead use pivot-root to make copy of ount tree and unmount the orginal 
    - networt namespaces
        - isoltae nwtrok resources e.g os config, firewall, routing
        - can clone all these resources and make changes without changing te orginal
    - process namespace
        - when can clone whihc creates new process in first namespace but is pid 1 in process namespace
    - users
        - calling clone maps users to othre users in the namespace
- unprivaledged user namepsaces
    - dont need to be an admin to create a new user nsamepsace. can even assume root 
    - after create new user cna create a new mount namespace, chrome uses a a container for new tabs
    - hardened securtiy paratises reccomned to disbale unprivalidged user name spaces 
- demoed creating mount and network namespaces

- showed how you can crash the kernel by creating an unprevalidged namepsace without sudo
- soltuion - allow some applications to create user namespaces and displable for others
    - at cloud fare used bpf to add  in the process check into userns_create hook

### Zephyr Porting Efforts in High Performance SoCs (14:50 B2 4F)
- company works on providing high performenac socs to customers
- why port zephyr
    - keep real time tasks fast and safe
    - seperate software with differetn roledto idooate problems
    - want to establish common appraohing for high peeformace SoC support working with micro controllers

- mixed systems are hard to build - so first pprted zephyr to just arm64
- still need to improve driver support for high perf SoCs as mainly for MCUs
- talked about steps they took to boot zepyhr
-   - they first  port to a wdiely used soC supported by Linux
    - went through preperation steps as well as confguration an build files -->

## Conclusion
Overall it was a good conference. I attended a few other talks however the jet lag always seemed to hit hard at about 3 O'Clock in the afternoon so I struggled to really take much in aftyer that each day. It was interesting to learn about the SDV model as (despite working for codethink) I hadnt really been exposed to it in that much detail. Although interesting, the shift in the Automotive industry to start treating cars as software platforms concerns me due to the added complications this brings. For example introducing this huge stack of virtualisation seems to add alot of uneeded layers between the software and hardware which I imagine is going to make debugging hardware level issues very difficult. There was also an analogy made to the mobile phone when talking about the ability to provide over the air updates. This of course has alot of benefits for the software developer but also makes it very easy for the software to outgrow the hardware running it which is fine when the hardware is a phone, not so much when its your brakes.
I ended up attending alot of Zephyr talks which is a project I hope I can get involved with in the future.


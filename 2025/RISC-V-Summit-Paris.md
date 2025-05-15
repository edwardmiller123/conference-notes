# RISC-V Summit Paris

This was my first ever conference so I wasnt sure exactly what to expect. I went to the conference as someone who knows very little about RISC-V with the intention of learning as much as I could about the area. With this in mind, some of the talks were hard to follow as someone new the field of RISC-V and processor micro-architecture in general and so there were definitely parts of alot of the talks that I didnt understand. Nevertheless, I have tried to summarise most of the ones I attended as best as I could.

I had planned to go to specific talks but didnt realise that they were all back to back in the main venue hall so ended up attending all of them on the first day. These notes summarise most of the talks I went to however I have ommitted some that I either didnt find paritcularily interesting or just didnt understand enough of to include in the write up. I have also left most of my notes from the conference commented out in the raw mark down so they are still available to see out of preview for future refernce.

## 13/05/25

### From ISA to Industry: Accelerating Technical Progress and RISC-V adoption in 2025

This was a general talk about the progress in general adoption RISC-V had already made and plans for accelerating it further. The general gist of it was that RISC-V is steadily gaining traction with more commercially available chips appearing all the time. Some of the other key points covered were,
- the ratification of RVA23. This is the core 64 bit instruction set that will support the numerous extensions.
- an interesting mention for pushing for RISC-V chips in space and other hazardous environments.

There was some words from the RISE foundation about further improving the software ecosystem for RISC-V. An example was the work to port pytorch. In addition to this
there was a short announcment from the Yocto project saying there would be much better support for RISC-V going forward, namely in the form of offical yocto images and support for all RISC-V profiles. To quote, "if your compiler works then yocto can build it". With the sheer number of custom extensions ot the ISA I feel there will be some exceptions to this.


<!-- - general talk about riscv ganining tracttion
- interesting mention of riscv in space applications due to security architecture
- ratified RVA23 (vector extensions)

- words from RISE
    - porting pytorch etc..

- better riscv support from yocto
    - offical images
    - support for all riscv profiles
        - if your compiler works then yocto can build it -->


### High performance RISC-V (Ventana)
This was mainly an advert for Venatana showing off their Veyron V2 cores and announcing the Veyron V3. The company focus is mainly on getting RISC-V cores into data centres. The talk highlighted the benefits of RISC-V with AI work loads mainly due to the vector and matrix multiplcation extensions that chip designers can tailor for specific use cases. In the announcement of the Veyron V3 they claimed a clock frequency of 4.2GHz which is starting to sound much more competitive with the likes of Intel and AMD as well as a [SPECint2017](https://www.spec.org/cpu2017/) score of 11.

The most interesting part of this talk was the micro architecture of the V3 core. To maintain speed and power efficiency the core executes what they called "Macro ops" which can be sequences of 1 to 5 RISC-V instructions. Individual instructions can be cached/encoded/decoded in batches which much improves efficiency. All of this is invisible to the programmer who can write regular assembly which is then fused into macro ops by the core's "Advanced fusion engine". 


<!-- - trying to get riscv chips into data centres
- promoting ai with riscv 
    - single architecure across cpu and accelerator
- look up verron v2 8.4SPECint2017
    - annaounced veyron v3 11SPECint2017
        - high single thread perfomance with power efficiency
        - up to 4.2 GHz
        - micor architecture
            - use macro ops (1-5 instruciions) so can cache/encode/decode sequences of insructions
            - micro arch works in macro ops rather thna idividual instructions -> more power efficent
            - 16 execution shcedulrs and pipelines
            - "Advanced fusion engine" fuses instructions into macro ops -->


### ESWIN 
This was another general talk about the state of RISC-V adoption as well as an advert for ESWIN's products. The talk first covered the challenges facing RISC-V as a whole, the biggest of these being market awareness and general software support. They also talked about the AI space and how its providing good opportunities for RISC-V, mainly due to the ISA's modular nature with the custom vector extensions. Finally the talk covered the various products on offer from the company ranging from gaming monitors to various embedded devices that utilise RISC-V cores.

<!-- - challenges of riscv
    - makret awareness, general adoption, available software etc...
- AI providing good oppurtunities for riscv
    - suitable due to modular design

- General info about ESWIN
    - software and riscv cores
    - showed exmaple products using riscv
        - monitors, camera systems, automotive motor controls -->


### TLB poster conversation
As well as the main talks I had some very interesting conversations with people at the stands throughout the conference. One of these was with someone talking about their research into improving the efficiency of TLB shootdowns, the process by which all cores in a CPU are forced to flush their translation Lookaside Buffers (TLB's) when access to a page is denied by one of the cores. The standard way of triggering this is to interrupt all the cores however this has a alot of overhead which starts to become more noticable on systems with a large number of cores. A better way (supported by ARM) is to send a non interrupting TLB flush request only to the cores that would be affected. This removes the issue of having to interrupt every core (potentially unnecessarily) but still has some overhead. RISC-V currently only supports the interrupt method which isnt ideal. Although the presenter had no exact solution one idea he gave was that the hardware could expose a specific control register conataning the nessecary infomation as to which cores needed to flush their TLBs.


<!-- - interrsting poster about TLB shootdowns
- using interrupts isnt very efficeint on systems with lots of cores
- sending TLB flush requests is much better (ARM feature) but still has issues
- RISCV currently doesnt support non intterrupt method.
- poster man suggested that the hardware should expose the nessecary info in a register but this relies on the vendors -->


### State of the Union
Another general talk about the state of RISC-V from one of the original creators. The talk highlighted the increasing adoption in industry and how the ISA is solidifying itself as a standard base for new Ai accelerators. There was a quick history of RISC-V starting all the way back from its creation in 2010 moving to the initial standardization RV64GC from 2015 to 2019 and finally the future of RISC-V in which we our seeing hardware implemntations moving into focused areas e.g AI. The presenter went through the various profiles that have been ratified and mentioned that the next major release (RVA30) will be seen in the next 2 to 3 years. It was also highlighted how more work was still needed in porting existing software to RISC-V (A sentiment held by many at the conference).

The talk went on to cover some notable extensions that are in progress. Some of these were,
- SPMP: provide S-mode supervisors protection from U mode tasks
- RISCV Worlds: secure partitioning of devices on SoCs
- Supervisor Domains
- Support for long instructions (greater than 32 bits)
- (Even) more support for vector instructions

AI was of again highlighted as a great use case for RISC-V due to its flexability. This is due to its balance of scalar, vector and matrix capabilities which can all be done on the same core. It was also noted that the specific balance of scalar vector matrix performance in an implementation of a core can be changed in the micro architecture without needing to mak any changes to software that runs on the platform. This allows RISC-V to maintain flexability while also being a reliable platform for developing software. The talk went a little deeper into the implementation of 4 types of vector operations. These were,
- Dot Product: No addtional matrix state needed on top of vector registers however doesnt scale well
- Matrix in Vector: no additional state, allows larger throughput
- Vector Matrix: Adds aditional state to processors with existing vector registers, maintains vector ISA and memory model
- Seperate Matrix: Custom for very large matricies. Doesnt use standard vector registers

<!-- changing software
 - standard base for new ai accelerators
 - increasing industry realisation
 - riscv 15 years old woo!
 - quick history
    - 2010: initial development
    - 2015 - 2019: RV64GC initisal standardization
    - 2020 onwards: commercal adoption
    - 2025: moving into more focused areas
        - more work needed porting existing software to riscv
- various profiles ratified 
    - next major release will be RVA30 but not for the next 2 - 3 years
- goal to put all specification content into a single repo
- Security extensions in progress:
    - SPMP: provide smode supervisors protection form U mode tasks
    - RISCV Worlds: secure partitioning of devices on SoCs
    - Supervisor Domains
    - CHERI: new base ISAs (RV32Y)
- DSP extensions
    - P extension: 100 new instructions
    - support for vector instructions

- Long instructions
    - compressed instructions
    - support >32b instructions

- ay eye:
    - needs large memory fotprint, mmory bandwidth, numerical compute
    - will need more complex control and shceduling
    - riscv has balance of scalar, vector and matrix capabilities
        - same core can do all
    -   - can change balance of scalar vector matrix in micro architecture without changing software
- matrix extensions
    - great for ai
    - showed 4 supproted methods for matrix multipications
         - dot products, matrix-in0vector, vector-matrix, sperate-matrix
         - first 3 use the RVV vector registers
            dp: no addtional matrix state, doesnt scale well
            MiV: no addiotnal state, allows larger throughput
            VM: add state to processors with existing vector registers, maintains vector ISA and memory model
            SM: custom for very large matricies. doesnt use standard vector registers
- take away: riscv well suited for AI with matrix extensions -->


### RISC-V ISA Extensions with Hardware Acceleration for Hyperdimensional Computing
A talk from a PHD student going over their research into RISC-V's suitability for Hyper Dimensional Computing (HDC). The talk explained how HDC is a way of encoding information through the use of vectors with a large number of dimensions. The group created a new extension to RISC-V implementing this explaining how this particular model is very useful for AI workloads as it is scalable, resiliant against noise and allows for extreme parallelism. Although there are some current offerings for this type of model they are task specific and not suited to general purpose hardware. Specifically, current extensions only work with hyper vectors of fixed length and type. The custom extension desgined by the team mapped well to high level C functions. Testing was done by creating a hardware accelerator which was intergrated into an RV32IM Klessydra T03 core. To free up main memory all HDC operations were executed directly on dedicated memory. The results from the research were a consistent speed up of HDC tasks showing that RISC-V is a perfect base for these types of computations. A good question was asked about the comparison of the calculations when done via standard vector operations however the asnwer was that the task in question was very specific so it was not easily comparable.

<!-- - new isa extension for hyperdimensional computing
- HDC: encode information through high dimensional vectors
    - space chacrecterized by operations:
        - binding, bundling, permutation, clipping
    - ideal for ai 
        - scabale, reselinat against noise, extreme parallelism

- current archs are task specific 
    - no general purpose hardware
    - only work wth fixed length and type HV (hyper vectors)
- riscv perfect for a general puprose HDP extension

- team designed a hardware accelerator and intergrated into RV32IM Klessydra T03 core
    - can be configured fo HDC at synthesis time
- team designed a custom extension which maps to a high level functions in C
- all hdp operations executed directly on dedicated memory

- results
    - consistent speed up across key HDC tasks
    - question asked about comparison with standard vector operations
        - task ran was very specific so not easily comparable -->




<!-- ### Real-Time Extension to the RISC-V Advanced Interrupt Architecture (12:15 S2) T1.3
- RISCv AIA doesnt satisfy real time interrupts
- proposing a new extension
- avoid stakcless execution in interrutrupt handler

- new extension adds new nested vectored interrupt handling mode
    - external interrurpt verctor table
    - new interrupt delivery mode
    - mode added to all privilige levels



### Atomic IO Enqueue Extension
- general cpus cant handle hetrogeneous loads so have to offload to gpu etc...
- linux jernel issues
    - context switch overhead
    - weak isolation for security
    -synchronisation primatives overhead
- hardware features bypass krnal to fix these issues
    
- AIOE:
    - UENQ instruction single 64 byte atomic write
        - uses 8 registers to store data
    - AIOE PMA defines SNQ and UENQ phyical memory attributes
    - CSR_SUENQ: designated register

- GIPC extension
    - process id distinguishes VMs
    - no nested page table walk

- AIOE Extension elimeates sync primatives overhead

- GIPC extension enables AIOE to function across processes of different VMS


- look up heterogeneous programming -->


### Accelerating AI Models - Andes Matrix Multiplication
This was a very interesting talk about a new extension improving matrix multiplication on RISC-V cores using the standard RVV vector registers. The main idea is that the matrix is broken into tiles like such `M x N tile = AMM(MxK tile, KxN tile)`. The extension treats multiple vector registers as a single vector allowing for larger matricies to be processed as well as adding 2D load and store instructions to handle the new format. The talk also talked about dealing with some edge cases and also how high level code, from example pytorch, is compiled to the new instructions. In conclusion the presenter claimed a 2 - 6 times speed up using the new extension when compared to regular vector operations.

<!-- - AMM designed to optimise matrix multiplication while reusing the RVV registers
-   - tiles stored in RVV registers
    - M x N tile = AMM(MxK tile, KxN tile)
    - tiles are broken down in size
    -  vector legnth depends on VPU implementaion
    - can treat multiple vector registers as one long vector
    - dimension k is longer so can process more in parallel
    - offers 2d load and store instructions to store new data format
- wjat if input shapes are not multipes of AMM tiles 
    - bouandary conditions set in custom regsiter

- example compilation workflwo:
    - pytorch -. layer -> layer -> convert to AMM tiles -> LLVM

- AMM had a 2 - 6x speedup over regular vector operations -->


### Codasign NES Emulator Poster
An interesting conversion with someone from Codasip about a demo project for their verification tools. The tool in question could check that instructions in an added extension would not break any of the core instructions on a chip. The demo they had created to showcase this was a NES "emulator". The chip in the original NES was a 8-bit 6502 processor with 56 instructions. These were then added as a custom extension to RISC-V and the checking tools could ensure that none of these added instructions would break the chosen RISC-V cores existing functionality. The upshot of this was that games made for NES could run directly on the RISC-V chip with the added performance improvements of the modern chip.

<!-- - poster showing off tools for checking that custom extensions to a core will not affect the core instructions
- example they used was the instruction set from the 8-bit 6502 processor in the NES which they effectivly added as an extension to a riscv core.
- this could then run any software that ran on the original hardware. Game ran with much improved performance and enegry efficiency -->

### Cake
 - They cut a cake and sang happy birthday to RISC-V. It was very awkward.

### Panels and Lighting rounds
There were a number of panels and brief two minute talks from the various sponsers of the event which I will just summarize all together here. Most of the disscusions just rehashed what had already been said, market challenges, increasing adoption, etc but there were several interesting questions asked,
    - panel was asked why vector extensions are so complicated. The answer was that the use cases provided by the flexability outweighs the added complexity
    - panel was asked if they were worried that the spec as a whole was getting to complicated. The response was again to do with flexability. The orignial goal was to have a simple core ISA that could easly be adapted to specialized workload. To that degree the core ISA has remained simple but the huge amount of custom extensions has created the complexity.
Another interesting point raised in a later panel was that RISC-V is still missing the killer application to speed up software support e.g ff 3 million people complain that google maps doesnt work on RISC-V then google will fix it. Perhaps Android phones would be a good way of getting RISC-V hardware to the mass market?

<!-- ### Panel: RISC-V at 15
- was originally used to teach computer architecture. Got an email complaining when the spec changed
- shortage in china of chip designs. All students can now create there own designs. One student one chip is an initiative in china 
- to get undergrads designing there own chips
- first riscv workshop they found that cores were already being shipped commercially
- panel was asked why vector extensions are so complicated:
    - the flexability in the instructions are useful for software so....deal with it
- asked what would they do differently in the ISA:
    - nothin worth re doing. Its more important to have a solid unchanging base for software to be built on than to make everything perfect.
- asked are you worried the spec is getting to complicated:
    - goal was to have a simple core ISA but could easly be adapted to specialized workload e.g with custom extensions -->

<!-- ### lighting rounds (company ads)
- akeana
    - processor ip company
    - soft ip

- Andes
    - vector processors 8 or 16 core
    - support up to 48 bit vecotrs
    - ai crap

- axiomsie
    - debug and code verification
    - footprint: tool to find redundant areas in silicon

- chip agents
    - ai agent for designing risc-v chips

- codasip
    - largest riscv ip provider
    - customizable riscv cores
    - ported CMSIS to RISCV

- lauterbach
    - debug and trace tools

- openEuler
    - trying building LLVM on RISCV

- semidynamics
    - started with regualr cpu then reliased people have alot of data so implemented vector
    - gazzillion misses system
    - speciazided tensor unit

- sifive
    - talked about new HiFive Unmatched Rev B. Has two chips
    - the dreaded p550

- Tristan Isolde
    - project to get more RISCV cores from the EU -->


### RISC-V in harsh environments poster
Talked to someone about their poster on a chip design for harsh environments. The application in question was for use in industrial drill bits where the temperature can reach up to 175 degrees so standard comercial chips wont surfice. I asked specifically if temperature requirements of the design were helped at all by RISC-V. The chip design could withstand the high temperatures mainly due to the materials it was made out of as well as some small implemenation details such as shutting off certain parts if they get to hot however this would not be very helpful from a software persepective. The main benefit of RISC-V was simply providing a flexible base for which to design such a chip. This seemed to be a common theme throughout the conference.

<!-- - talked to someone about there custom chip design for use in drilling systems
    - temperatures can reach 175c so normal chips wont cut it
    - riscv gives the flexability to create such a chip. e.g floating point unit can shut off if it gets to hot
    - temerapture reselinace comes from the material -->

<!-- ### Supply chain manufacturing issues
- highlighted that tsmc has over 70% of the global market
- europe needs to invest significantly to manufacture their own chips -->


<!-- ### Accelerating GenAI Workloads bu Enabling RISC-V Microkernel Support
- Gen ai workloads rely on matrix matrix and matrix vector multiplication
- IRRE
    - compiler and runtime that turns models into machinecode
    - easy to customize for specific arch through specific HAL
- need matmul that uses cache hierachy

- mmt4d netter than matmul for large matrices
- talk explained how to optimise micro kernel for an llm -->

### RISC-V based GPGPU on FPGA: A Competitive Approach for Scientific Computing?
I didnt understand much of this one due to my lack of knowledge about GPUs in general but the talk showcased some reasearch into the viability of a RISC-V base GPGPU for scientific computing. Generally the type of computations done in scientific computing use linear algebra frameworks which mosly involve matrix vector and vector vector type calculations. Modern GPGPUs are very good at this however the recent AI trend has meant they are becoming more optimized for low presicion computing scrapping 64 bit floating point support which is also heavily used in scientfic computing. The speakers hence suggested that a RISC-V based chip could provide the best balance between the two of these. The actual detail of the investigation was lost on me a bit sadly .

<!-- - scientifc computing require 64bit floating point
- GPGPUs have enabled beter perfeorfmabce in supercompiters
- GPGPUS are more optimized for low presicion computing becuase of AI
    - e.g scrapping floating point support
- scientifc computing rely on linear algebra frameworks
    - mostly matrix vector and vector vector
    - limited by memory bandwith

- study targed two benchmarks LINPACK and HCGP

- CGRA arch map better on FPGA
- sci comp users already familiar with GPGPU frameworks

- research targeted AMD Alveo V80

- early results 14 clusters of 4 cores
    -  each cluster has own configureations
    - frist implementation promising, high utilisation and high frequency
    - main challaenges is memory hiearchy -->


## 14/05/25


### CVA6: Sovereign Open Source HW (Thales)
Bassicaly just an ad for Thales products. The talk gave a general overview about what the comapny does and why they are investing into RISC-V. These were the usual reasons namely, 
    - RISC-V allows for a fully auditable processor
    - Can create chips without worrying about Geo political issues e.g over reliance on the US and China.
    - No vendor locking
    - Customization
They finally briefly talked about a new spatial on board computer based on their experimental ASTRALSoC which comprises of multi-CVA6 cores along with opentitan and a vector accelerator


<!-- - Thales overview
    - space, defence, aviation, cyber security
- geo political issues, dont want dependacies on thes euncertain countries e.g us, china
- why they chose riscv
    - a fully auditable processor
    - sovereignty: no export constarints, enable string EU investment
    - no vendor locking
    - customization: specific hardware for specific needs
- new spatial onboard computer
    - open soure and hardedned for space application
    - experimental ASTRALSoC: multi-CVA6 core + opentitan + vector accelerator -->



### Enhancing your RISC-V SoC debug and optimization with embedded functional monitors (Siemans)
This was an interesting talk giving an overview of functional monitors in embedded systems as well as an advert for Siemans own debug solutions. The talk explained that functional monitors are IP inserted directly into the chip to observe there performance as well as allow for debugging. An example given was that of a monitor on the main bus between a core and main memory. All requests from the core must go via the bus and hence a system nonitoring the bus directly can exactly where in memory software is making reads and writes as well as general bus utilisation. The talk covered some other example monitors that can be embedded into a chip such as, trace encoders, status monitors and DMA monitors.
The talk went on to highlight that for a complex chip the amount of debug data would be to much off load and provided some solutions to mitigate this. These were
- smart filtering e.g only montior writes to certain addresses
- smart logging
- counters: e.g counting busy cycles (performance monitoring)
- cross-triggering: configure monitors that rely on the result of others. e.g only offload data if a certain address was written to
The talk concluded with a customer case where they used a bus monitor to locate a specific memory write that was causing a dead lock.

<!-- - need better way to observe Socs
- a functional monitor is ip incerted into the chip to observe its performance
- easier debugging pls health monitoring after launch

- all request from core to ram must go via the bus
- the monitor can monitor everything that goes through a bus and hence,
    - see bus utilisation,
    - see where inmemory software is reading and writing to

- hent through different monitor types that cna be embedded into a chip
    - trace encoder
    - status monitor
    - bus monirot
    - dma monitor

- for a complex chip the debug data would be to much to offload. this can be fixed by
    - smart filtering e.g only montir writes to certain addresses
    - smart logging
    - counters: e.g counting busy cycles (performance monitoring)
    - cross-triggering: monitors that rely on the result of others. e.g only offload data wa certain address is written to

- customer cases
    - pico com imrpoved performance using functional monitors
    - meta: 
        - used monitor to offload the register states when something went wrong
        - tracking down a bus dead lock. Used functional monitor to view address writes which allowed them to quickly find the address casuing the deadlock -->


### XiangShan KMH
Another ad for a new core design. They claim their chips can achieve around 3GHz which is a much more competetive offering. They gave a quick overview of their two architectures Kunminghu and Nanhu and talked through their verification process. The talk stood out a little in this aspect as they mentioned they try to involve software engineers in the testing phase as well as use an "agile" verification approach.

<!-- - new riscv core
- goal of XinagShan project
    - open source riscv core that can be adopted by industry like linux
    - most active open soruce github chip project
- overview of different cores
    - Kunminghu arch
    - nanhu arch

- chips get around 3Ghz
- talked through verification process
    - tried to use an "agile verification" approach
    - want to invlove software engineers in verification process
    - developed unitychip software which is easy to use for testing  -->


<!-- ### 
- ai crap
- silsicon diversity, 
- openess and customizability of risv good for inovation
- talked about companies chips Athena chip
    - 62 SPECint2017 score
- re iterated that riscv still needs large investment from major companies

- claims ther callandor core will be the most performant in the world in 2027

- maintain silicon diversity by,
    - design resue
    - diversity through composing designs from different organizations

- Open Chiplet Architrecture (OCA)
    - a open spec for designing chips and allowing chiplets form different manufactures to be compatable
    - 5 layers
        - physical
        - transport
        - protocol
        - system
        - software

- OCA takes care of chiplet compatability
    - each company should focus on core competency design and all the oarts will be compatable -->

### Cloud Based RISC-V Servers (Scaleaway)
This talk was from a european cloud provider (Scaleaway) about their journey adding RISC-V servers to their bare metal compute offering. They gave a quick history of the company and started talking about the difficulties they had getting different OS's to boot on the machines. This talk resonated with me as we experienced similar issues when setting up our RISC-V Gitlab runners so its reasuring to see that it wasnt just us hitting these problems. The difficulties in question were, having to use board specifc OS images directly from the vendors as well custom OpenSBI and Uboot versions all the changes of which were never upstreamed. This led to alot of trial and error to find versions that worked. The talk went on to describe the hardware they chose for the final product, this being custom printed cases and racks of TH1520 T-Head boards. They also briefly talked about a specific vulnerability that was discovered in the boards they were using. This was the GhostWrite vulnerability caused by a defective instruction that allowed reads and writes to physical memory without priviledges. Unfortunately I didnt catch the part where they talked about patching this in their machines.



### Using CMSIS for simplified migration to RISC-V (10:35 S3) T2.2
An interesting demo talk showing off codeasips port of the CMSIS libary to RISC-V. Their motivation was that most embedded software projects start with legacy code running on arm cortex devices. They claim their port to RISC-V is "core aware" (I assume with a custom linker?) so that you can just take your embedded arm application as is and compile it for a RISC-V core with the libary taking care of the specific register accesses etc. The main benefit of this is you can improve the speed and power efficiency of your embedded application just by switching to RISC-V with minimal softwre changes. The talk compared some results of applications running on cortex M0 and M4 cores compared to the same applicaion on a RISC-V procsessor (I cant recall which exact design) and showcased the improvements. They also provided an email to send them your CMSIS application to see the performance improvemnents on RISC-V. I thought this was an interesting talk as it reduces the learning curve for programming embdedded systems on RISC-V significantly.


<!-- ### A Safe Software Convergence
- to many boundaries between embedded fields e.g automotive isnt special
- automotive
    - long developemtn cycles
    - unique requirements
    - complex tier system
    - extremely resistant to change
    - closest analogue is industrial

- automotive semiconductors
    - temp range -40 to 125
    - 10 to 15 year product lifecycles

- SDV
    - big market growth
    - shift from hardware dependant models to felxible software platforms
    - vdistributed arch ehicles have between 30 to 150 ecus
    - modern consolidated arch
        - fewer points of failure (debatale)

- trad approach
    - fixed function hardwrare
- modern arch
    - programmable Socs, containerized apps
- real time processing
    - sub ms response ties
- safety standards
    - ISO 26262 (Automotive)
    - IEC 61508 (Industrial)

- trying to emphasize that automotive chipps are musch more simalr to standard compute chips nowadays
- seperate desgin cycles dont make snese. Automotive should act more like tech companies

- RISCV is perfect for automotive due to the flexability, and can be independant from big chip makers
- biggest oppuritnity for risc v in autmotive may be in europe

- talk was a bt buzzwordy and not really sure what the point of the talk was -->

### VeriCHERI: Exhaustive Security verification of CHERI processors
This talk was about a verification process for the security of CHERI. CHERI (Capability hardware enhanced RISC Instructions) is a way of ensuring memory safety at the hardware level. The specification provides fine grained memory protection in hardware via the usage of "capabilities". The idea behind this is that when accessing physical memory the hardware provides you with a Capability rather than a direct pointer. As well as the requested memory address, a capability contains upper and lower bounds for allowed memory access as well as permission flags. Any reads or writes outside of these bounds will raise an exception at the hardware level. The talk itself focused on verifiying that this sysytem was in fact secure and show cased the teams tooling for this, VeriCHERI. The speaker talked about using the non inteference security model. CHERI divides physical memory into public and private parts with two sections having non interference if memory changes in one cannot affect the other.
The talk concluded with a case study for the verification tools in which they had identified a potential transient execution attack. I didnt catch the exact details but the gist is that, by measuring execution times, an attacker can probe any two bits of a protected memory address. After the talk I spoke to someone at the LowRisc booth who gave me a demo of a CHERI board raising an exception after a bad memory access (the infamous [heartbleed](https://xkcd.com/1354/) attack to be precise). I was suprised that this idea wasnt already being used in comercial hardware. We went on to discuss some of the downsides to this system, for example having to take into account large pointer sizes when writing software.

### RISE
A talk from RISE going over the recent software improvements to the RISC-V eco-system. Here are the bullet points,
    - LLVM opimizations
    - Go compiler optimizations
    - better python packaging
    - QEMU TCG: Enhanced performance for vector
    - Rust: On track to meet tier 1 requirements
    - openOCD support
    - LLVM CI improvements


### A RISCV Based Accelerator for Post Quantum cryptography
This one was quite hard to follow but that was just due to my poor cryptography knowledge. The talk covered the work done to find suitable cryptographc algorythm that could then be implemented into a RISC-V core (I beleive). Post Quantum cryptographic algorithm is one which remains secure when aginst both classical and quantum based attacks. e.g a Quantum computer may be able to factor huge numbers very easily thus rendoring some methods insecure. They had chosen to try and implement the McEliece crypt system. This uses a key system in which two participants generate matricies and then scramble them using there own private keys. The algorithm was chosen by the team as the encryption/decryption process is fast and has proven resiliance against [Shors algorithm](https://en.wikipedia.org/wiki/Shor%27s_algorithm). Some implementaition challenges with this choice were also raised. These were that the algorithm generates very large keys which are hard to store in memory and that the chip has to be able to handle computations involving large matricies. The teams work focused on optimizing the Fast Fourier Transform module. The talk concluded saying that their current implementaion design is a WIP but definitely feasable.


### Implementing Runtime-Configurable Endianness in RISC-V: Challenges and Solutions (12:45 S2) T2.3
Finished off the conference by supporting the team. This talk covered the work done to add big endian support into RISC-V. Specifically adding support to Qemu, OpenSBI and getting a minial linux kernel booting in big endian mode. They started off with an explanation of endianness as well as histoical use cases such as older instruction sets and networking protocals. Next they moved on to the endianness of RISC-V. The ISA is little endian however the latest spec allows the endianness of the data to be configured. They continued on with the details of the work done, firstly to add support into Qemu. Some examples of this were adding a a memory operation flag for store instructions and updates to the IO code to swap the byte order since the MMIO bus is still little endian. It was noted that hand assembled instructions in some compilers can be problematic as the instructions them selves are always little endian.
Getting software libaries to compile seemed more straight forward as libc already had a byte order macro that could be used. Next the talk covered some nesseccary fixes made to the linux kernel to get it to boot e.g fixes to instruction modification. They concluded the talk with some hopes for the future of the project, these being to try and get the changes upsreamed and also to at some point fork CVA6.



## Summary
Overall I thoroughly enjoyed the conference and it was very interesting to learn more about the hardware side of the RISC-V ecosystem. My only regret is that I didnt have a better initial understanding of some of the topics before hand so I could have gotten more out of the talks. Overall the quality of the presentations was very high despite some being a little hard to follow. The big endian presentation from Codethink naturally was very good. The conference has definitely shown me that Id like to learn alot more about the details of processor design going forward in my career.
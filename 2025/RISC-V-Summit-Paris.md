# RISC-V Summit Paris

This was my first ever conference so I wasnt sure exactly what to expect. I went to the conference as someone who knows very little about RISC-V with the intention of learning. With this in mind some of the talks were hard to follow as a newcomer to the scene. 
I had planned to go to specific talks but didnt realise that they were all back to back in the main venue hall so ended up attending all of them on the first day.

## 13/05/25

### From ISA to Industry: Accelerating Technical Progress and RISC-V adoption in 2025

This was a general talk about the progress in general adoption RISC-V had already made and plans for accelerating it further. The general gist of it was that RISC-V is steadily gaining traction with more commercially available chips appearing all the time. Some of the other key points covered were,
- the ratification of RVA23. This is the core 64 bit instruction set that will support the numerous extensions.
- an interesting mention for pushing for RISC-V chips in space and other hazardous environments.

There was some words from the RISE foundation about further improving the software ecosystem for RISC-V. An example was the work to port pytorch. In addition to this
there was a short announcment from the Yocto project saying there would be much better support for RISC-V going forward, namely in the form of offical yocot images and support for all RISC-V profiles. To quote, "if your compiler works then yocto can build it". With the sheer number of custom extensions ot the ISA I feel there will be some exceptions to this.


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
This was mainly an advert for Venatana showing off there Veyron V2 cores and announcing the Veyron V3. Their company focus is mainly on getting RISC-V cores into data centres. The talk highlighted the benefits of RISC-V with AI work loads mainly due to the vector and matrix multiplcation extensions that chip designers can tailor for specific use cases. In the announcement of the Veyron V3 they claimed a clock frequency of 4.2GHz which is starting to sound much more competitive with the likes of Intel and AMD as well as a [SPECint2017](https://www.spec.org/cpu2017/) score of 11.

The most interesting part of this talk was the micro architecture of the V3 core. To maintain speed and power efficiency the core executes what they called "Macro ops" which can be sequences of 1 to 5 RISC-V instructions. Individual instructions can be cached/encoded/decoded in batches which much improves efficiency. All of this is unknwon to the programmer who can write regular assembly which is then fused into macro ops by the core's "Advanced fusion engine". 


<!-- - trying to get riscv chiops into data centres
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
This was another general talk about the state of RISC-V adoption as well as an advert for ESWIN's products. The talk first covered the challenges facing RISC-V as a whole, the biggest of these being market awareness and general software support. They also talked about the AI space and how its providing good oppurtunities for RISC-V, mainly due to the ISA's modular nature with the custom vector extensions. Finally the talk covered the various products on offer from the comapny ranging from gaming monitors to various embedded devices that utilise RISC-V cores.

<!-- - challenges of riscv
    - makret awareness, general adoption, available software etc...
- AI providing good oppurtunities for riscv
    - suitable due to modular design

- General info about ESWIN
    - software and riscv cores
    - showed exmaple products using riscv
        - monitors, camera systems, automotive motor controls -->


### TLB poster conversation
As well as the main talks I had some very interesting conversations with people at the stands throughout the conference. one of these was with someone talking about their research into how to improve the efficiency of TLB shootdowns, the process by which all cores in a CPU are forced to flush their translation Lookaside Buffers (TLB's) when access to a page is denied by one of the cores. The standard way of triggering this is to interrupt all the cores however this has a alot of overhead which starts to become alot more noticable on systems with a large number of cores. A better way (supported by ARM) is to send a non interrupting TLB flush request only to the cores that would be affected. This removes the issue of having to interrupt every core (potentially unnecessarily) but still has some overhead. RISC-V currently only supports the interrupt method which isnt ideal. Although the presenter had no exact solution one idea he gave was that the hardware could expose a specific control register conataing the nessecary infomation as to which cores needed to flush their TLB's.


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
A talk from a PHD student going over their research into RISC-V's suitability for Hyper Dimensional Computing (HDC). The talk explained how HDC is a way of encoding information through the use of vectors with a large number of dimensions. The group created a new extension to RISC-V implementing this explaining how this particular model is very useful for AI workloads as it is scalable, resiliant against noise and allows for extreme parallelism. Although there are some current offerings for this type of model they are task specific and not suited to general purpose hardware. Specifically current extensions only work with hyper vectors of fixed length and type. The custom extension desgined by the team mapped well to high level C functions. Testing was done by creating a hardware accelerator which was intergrated into an RV32IM Klessydra T03 core. To free up main memory all HDC operations were executed directly on dedicated memory. The results from the research were a consistent speed up of HDC tasks showing that RISC-V is a perfect base for these types of computations. A good question was asked about the comparison of the calculations when done via standard vector operations however the asnwer was that the task in question was very specific so it was not easily comparable.

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
This was a very interesting talk about a new extension improving matrix multiplication on RISC-V cores using the standard RVV vector registers. The main idea is that the matrix is broken into tiles like such `M x N tile = AMM(MxK tile, KxN tile)`. The extension also treats multiple vector registers as a single vector allowing for larger matricies to be processed. The extension also provides 2D load and store instructions to handle the new format. The talk also talked about dealing with some edge cases and also how high level code, from example pytorch, is compiled to the new instructions. In conclusion he presenter claimed a 2 - 6 times speed up using the new extension when compared to regular vector operations

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


### codasign poster nes emulator
- poster showing off tools for checking that custom extensions to a core will not affect the core instructions
- example they used was the instruction set from the 8-bit 6502 processor in the NES which they effectivly added as an extension to a riscv core.
- this could then run any software that ran on the original hardware. Game ran with much improved performance and enegry efficiency

### cake
 - They cut a cake. It was very awkward

### Panel: RISC-V at 15
- was originally used to teach computer architecture. Got an email complaining when the spec changed
- shortage in china of chip designs. All students can now create there own designs. One student one chip is an initiative in china 
- to get undergrads designing there own chips
- first riscv workshop they found that cores were already being shipped commercially
- panel was asked why vector extensions are so complicated:
    - the flexability in the instructions are useful for software so....deal with it
- asked what would they do differently in the ISA:
    - nothin worth re doing. Its more important to have a solid unchanging base for software to be built on than to make everything perfect.
- asked are you worried the spec is getting to complicated:
    - goal was to have a simple core ISA but could easly be adapted to specialized workload e.g with custom extensions

### lighting rounds (company ads)
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
    - project to get more RISCV cores from the EU


### RISC-V in harsh environments poster
- talked to someone about there custom chip design for use in drilling systems
    - temperatures can reach 175c so normal chips wont cut it
    - riscv gives the flexability to create such a chip. e.g floating point unit can shut off if it gets to hot
    - temerapture reselinace comes from the material

### Supply chain manufacturing issues
- highlighted that tsmc has over 70% of the global market
- europe needs to invest significantly to manufacture their own chips


### Accelerating GenAI Workloads bu Enabling RISC-V Microkernel Support
- Gen ai workloads rely on matrix matrix and matrix vector multiplication
- IRRE
    - compiler and runtime that turns models into machinecode
    - easy to customize for specific arch through specific HAL
- need matmul that uses cache hierachy

- mmt4d netter than matmul for large matrices
- talk explained how to optimise micro kernel for an llm

### RISC-V based GPGPU on FPGA: A Competitive Approach for Scientific Computing ? (17:00 S2) T1.7
- scientifc computing require 64bit floating point
- GPGPUs have enabled beter perfeorfmabce in supercompiters
- GPGPUS ar emore optimized for low presicion computing becuase of AI
    - e.g scrapping floating point support
- scientifc computing rely on linear algebra frameworks
    - mostly matrix vectro and vector vector
    - limited by memory bandwith

- study targed two benchmarks LINPACK and HCGP

- CGRA arch map better on FPGA
- sci comp users already familiar with GPGPU frameworks

- research targeted AMD Alveo V80

- early results 14 clusters of 4 cores
    -  each cluster has own configureations
    - frist implementation promising, high utilisation and high frequency
    - main challaenges is memory hiearchy


## 14/05/25



### Enhancing your RISC-V SoC debug and optimization with embedded functional monitors (9:15 S2) T2.1



### Using CMSIS for simplified migration to RISC-V (10:35 S3) T2.2



### Implementing Runtime-Configurable Endianness in RISC-V: Challenges and Solutions (12:45 S2) T2.3



### RISC-V: Powering the Future of High Performance Computing? (14:30 S2) T2.5



### Flex-RV: World’s First Non-silicon RISC-V Microprocessor (16:45 S2) T2.7



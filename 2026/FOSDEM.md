# FOSDEM 2026
This was my first time at FOSDEM and I wasnt quite prepared for how many people there were and also how large it was. I therefor didnt make it to all the talks I wanted to but here are the ones I managed to attend and that I found the most interesting.
## Saturday

### f8 - an architecture for small embedded systems (K.3.601 11:40 - 12:10)
 <!-- - 16 bitarchitecture
     - cheap devices
     - bytes to kbs of data memory
     - few kb of program memeory
     - market domnated by proiotery achitectures e.g st with arm
     - 32bit and 64but has riscv butnothingopen soruce for 16bit
- small device c compiler
    - compilerfor 8bit architectures e.g mos 6502
    - unusual optimisatios forthese architectures
- sdc good for 8bit  target:
    - efficient stack ptr  relative addressing
    - unified address spaxe good for efficient poiter access
    - iregular archs can be efficient for tree decomposition based register allocation
    - good mixture of 8bit and 16bit instructions

- zero page addressing inst useful if we have efficent stack ptr relatve addressing
f8:
    - 8/16bit
    - iregualr cisc
    - core bigger than for risc but save so much code on memory that its worht it

    - showed the regitser set
        - loks very simple very nice
        - 3 16 bit general purpose registers (x, y, z) can also access at 6 8bit registers, pc, sp
    - showed exmaple of instructions
        - 8 bit instructions with 2 operands e.g adc xl, op8_2 -> xl = xl + op8_2 + c (op8_2 any of 8bit registers)
        - 8bit 1 operand 
        - 16bit 1op
        - 16bit loads
        - jumps
        - other misc 8 and 16 bit instructions e.g sign extension
    - all instructions write at most one 16 bit reg and a 16bit memory location
    - no alignment reqirements so padding bytes arent needed. This does make the hardware more complicated
    - saftey features
        - watchdog
        - reads from 0x000 trap
        - instruction 0x00 traps. e.g interuupt triggers system reset on null pointer dereference

- verilog implementation so can try out f8 on fpgas
- showed the full opcode table -->


The speaker started by talking about the common properties of 16 bit devices. They are usually cheap micro controllers with data memory in the range of bytes to Kilobytes and typically have a few kilobytes of program memory. The current market is dominated by propiotery architecures for example ST using arm. He noted that allthough there is RISC-V for 32 and 64 bit CPUs, there is no good open source instruction set for 8 and 16 bit architectures. This was the motivation for f8. He went on to talk about his work adding f8 as a target for SDCC (Small Device C compiler). This is a C compiler for 8 and 16 bit architectures (such as MOS 6502) and has some specialsed optimisations for these targets. He specificaly talked about of the ways SDCC is suited for 8-bit targets. For example efficient stack pointer relative addressing and also how a unified address space is good for efficient pointer access. Next he presented the f8 register set and showed some example instructions. F8 is an iregular CISC architecture with 3 16-bit general purpose registers (x, y, z), which can be alternativly accessed as 6 8-bit registers, as well as a stack pointer and the program counter. Some of the instructions he presented were 
- 8 bit instructions with 2 operands e.g adc xl, op8_2 -> xl = xl + op8_2 + c (op8_2 any of 8bit registers)
- 8-bit 1 operand instructions
- 16-bit 1 operand instructions
- 16 bit loads
- jumps
- other misc 8 and 16 bit instructions such as sign extension
He also noted that the architecture specification has no alignment requirements which removes the need for padding bytes in structs etc which I thought was nice. Finally he went over some of the safety features such as a watchdog and reset interrupts triggered by reads and writes from 0x0 memory addresses. He then showed the full opcode table and said there is a verilog implenantation if people want to try it out on FPGAs.

### GNU Algol 68 on baremetal (UD6.215 12:35 - 13:00)
<!-- - first algol 68 talk at fosdem
- high level prgramming language
- two types of langiage
    - macro assmeblers c, BLISS etc
        - can optimize
    - high level, when you dont want the lnaguage getting in your way
        - compiler writer does the optimising, consistency of programming constructs
    - rust, C++ pretending to be high levelwhile keepig macro assembly features - bad idea rules with alot o fbut


- algol 68
    - identity delcaration i = 1 not a varible! 
        - int i -> ref int i = loc int (sugar for creating a refernce to an allocation on the stack )
        - can control where things get allocated wth `heap` or `loc`. The heap allocaiotn however is not controlled by the programmer
- showed the flags needed for compiling with arm-none-eabi
    - even though there are gc flags there is no garbage collector and the heap keyword is undefined
- showed makefile example and code example on mcu
    - have to start in C as you need to define the stack, memory regions, interrupts etc in C as well as the algoc 68 main
        - also need to have IO functions in C/asm and have them called form algol -->

Apparently this was the first ever Algol 68 talk at FOSDEM which as pretty cool. The talk gave an overview of Algo 68 and the work needed to run it on a microcontroller. Algol 68 is a high level programming language with some interesting quirks. He started off talking about identity declaration and how a line such as `INT i = 1` in algol 68 does not create a variable `i` but creates an imutable constant and you need to explicitly. He also went over memory allocation with the `loc` and `heap` keywords. Interestingly, although you can choose to allocate on the heap, the programmer has no control over where this actually neds upo in memory. Next he went over the work done to allow compiling Algol 68 for microprocessors with `arm-none-eabi` He showed the commands for doing so and showed an example makefile. Although there were flags for garbage collection he pointed out that there actually was no garbage collector and the `heap` keyword was undefined which makes sense. He finished off by showing some classic blinky example code and how you can now call C functions in Algol 68. Turns out you cant really live without this functionality since the inital MCU setup such as initialising the stack, memory regions, interrupt table and even the defining the Algol 68 main function must be done in C or assembly. I asked the speaker afterwards why you would want to write Alogol 68 on an embedded system to which he replied "because its a beautiful language" and you know what I respect that.

### Libreboot: Free Your BIOS Today! (K.1.105 14:00 - 14:50)
<!-- - free coreboot distro
- we live in a socciety
- importance of free sofware and democracy
- libreboot
    - replaces bios/uefi
    - initialises cpu memory peripherals and loads the bootloader for the OS
    - distro of coreboot
    - modular and configurable
    - supports linux and bsd
    - meant to be easy to use for non-techinal users

- why use
    - more secure
    - no drm
    - continued updates e.g lenovo have network card whitelist so cant just use any network card
    - makes coreboot easy

- coreboot
    - coreboot only does hardware bringup e.g jumps ot payload GRUB, linux, edk2

libreboot provides a selction of payloads

- libreboot has LBMK (LibreBoot Make)
    - source based package manager (similar to apt)
    - eliminates manual build steps

- history of libreboot project
    - libre boot initially a gnu project
    - left gnu in 2016 )(ugly breakup)
    - creater steps down
    - full re write of Libreboot in 2017
    - libreboot coop, new regime removed 
    - war declared between libreboot and the FSF
        - hostile FSF fork of libreboot 
    - eventually the fork was removed after a a relase of libre boot hours before a talk anouncing the fork
    - FSF re branded hostile fork to GNU boot (temporary cease fire)
    - 2025 conflict ended with the treaty of boot
    - 2026 last stand at ubl

- libre boot binary blob reduction policy
    - avoid blobs where possible
    - snadbox if possible

- libreboot now suport intel 8th gen thinkpads
    - 
- future
    - better arm64 support
    - edk2 need on desktops now for modern GPUs
    - attract board maintainers from upstream

- demo of libreboot
    - tried to compile libreboot for a board, it failed

- questions;
    - will it brick my machine? -> probably not
    - will there be more alder lake/ twin lake support -> yes
    - asked why it was slow on someones virtual machine -> they will look at it -->

A talk giving an overview of what Libreboot is and a look at its rather spicy history. The speaker started off with a quick note on why free software is important to a democratic society and then got into the overview of Libreboot. In short it is a distribution of coreboot which is an open source BIOS/UEFI replacement. It takes care of low level hardware initialisation such as the CPU, memory and peripherals and loads the bootloader for the OS. The goals of libreboot are that it is modular and configurable, supports a open source OSs such as Linux and BSD and is meant to be easy to use for non-techinal users to install. Some of the reasons listed as to why you should use it over the UEFI that ships with your computer were 
- more secure
- no DRM - e.g Using lenovo as an example they have network card whitelist so you cant just use any network card you like
- gets continued updates
- makes coreboot easy
They then talked about LBMK which is a package manager for coreboot payloads that is as easy to use as apt. They explained how this significantly simplifies the process of installing an OS compared to using coreboot as it eliminates all the manual build steps. Next they went over the violent history of Libreboot. It was originally part of GNU until an ugly breakup in 2016. This was the start of the troubles. Around this time the original creator stepped down however after sometime they were unsatisfied with the progress of the project and so undertook a full rewrite of Libreboot. The new regime was then removed in the Librecoop of 2017 as the original creator took back control. At around this time the project also joined the Free Software Foundation (FSF). There were dissagreements however around the use of binary blobs and so conflict began. This was the start of the period known as the cold boot war. Tensions reached a boiling point when the FSF created a hostile fork of Libreboot. These projects would have competed however there was a tactical Libreboot release hours before a talk which planned to annouce the fork as the new project. The speaker was unclear as to the the content of the release however it was enough to stop the talk and force the FSF to rebrand the fork as GNU boot. This was the start of the ceasefire and, allthough the cold boot war stil continues, both sides have agreed to reduce their stock pile of project forks and commits of mass destruction. The speaker also clarifed that libreboot stil tries to avoid binary blobs where possible and use sandboxing if the use is unavoidable.

Finally the speaker ended by talking about the future of the project and by giving a short demo. They plan to add better support for arm64, support edk2 as its now needed on desktops for modern GPUs and they want to attract more board maintainers from upstream. They then attempted a demo of compiling libre boot for a specific board which failed. The speaker claimed multiple times that one of the main goals was to be easy to install for non techincal people however from the looks of it my Grandma better be up to speed with make and bash.

The speaker got asked a few questions however my favourite was "will it brick my machine?" to which the answer in short was "probably not".

## Sunday

### From C to Rust on the ESP32: A Developer’s Journey into no_std (UD2.120 09:30 - 09:55)

<!-- - neon beatbuzzer
    - buzzer quiz game with physical buttons
    - uses an esp32c3  dev board notifies main controller (raberry pi ) about button presses
    - firmware writen in C
    - want to moveit to rust wihtout the std libary

-need to use
    - esp-hal crate
    - esp_radio
        - exposes wifi ble
        - needs specific esp-alloc
        - needs esp-rots

- environ set up
    - rustup
    - esp-generte
    - started with generated defualt setup code

- embassy
    - async runtime (scheduler)
    - can declare functionsas tasks hich can run concurrently

- showed code snippets from the different sections, main func, wifi connection etc
- crashed the first time function in syscal table not implemented
    - chip has flash memory and propiotery code in rom
    - firmware in flash can call functions from ROM however
        - rom code expects a sys cal tabel in flash which it can call
        - fix has been merged

- had issues with the scheduler and socket ownership. Had to battle with the compiler a bit
    - e.g ownership was a problem and had to switch to global variables (wasquestioned if there was any benefit over C at this point)

- led maagement
    - most esp32 have rmt protool used for infared tranceivers
    - useful crate fr this

- in general rust had alot of useffl liabries already implemented

- showed demo of buzzer which looked cool -->

This described how the speaker re-wrote the firmware for a buzzer beat game he created from C to Rust without the standard libary and the challenges he faced. The game is in esscence a quiz with physical buttons. The button controller uses an ESP32C3 dev board which notifies a Rasberry Pi running the Quiz app. The firmware used the esp-hal and esp_radio crates which also required esp-alloc. He also used embassy which is an async runtime. He went on to show some code snippets and talk about an interesting crash with the error `syscall table not implemented`. The ESP32 has some propiotery code in ROM which can be executed by the code in the flash memory. However it turns out that the ROM code exepcts a syscall table to be setup in flash memory and hence it crashed as this wasnt implemented. He also said there were alot of issues with the scheduler and socket ownership which caused him to battle with the compiler a bit. In general ownership was a problem and he had to switch back to using global variables at which point it was questioned if there was actually much benefit over the original C implementation. Overall though he said that most of the functionality he needed there was already a rust crate for which was nice. Finally he finished off with a demo of the game.

### Securing Memory Isolation in Texas Instruments Microcontrollers (UD6.215 10:00 - 10:20)
<!-- - first company to make a silicon transistor
- first laser guidanc system
- high level systems isolation through virtulisation/ virtual memory, rings in the OS

- TI MSp430
    - physical tamper protecton
    - memory protection unit

    - iPE
        - TI has flat address space
        - areas of memory marked by writing markes to certain addresses
        - code outside address range cannot access code and data inside

- how secure is it
    - not very good
        - most attackes succeeded  e.g conrolled call corruption (discoved on this device, can corrupt memeory anywhere in the region), code gadget reuse etc

- can we protect against these
    - can repuropse MPU to prevent architectural attacks
    - have to assu,e the debugger is secure

- could the design be improved:
    - most bugs stem from hardware
    - difficult to research on as hardware and firmware closed source
    - however openMSP430 was released which issimialr enough
    - new propsal - openIPE
        - TI devices have propietory boot code that sets up the security featres
            - speaker wants to open soucre this
        - managed to mitigate an interrupt timing attack ith new arch
    - talked about the software security validation
    - goals of openIPE
        - unified memory isolation implementation
        - rapid prototyping of security fetaures
        - thourogh testing

- questions
     - - asked if their solution was really fell uder confidentioal computing since it was a software solution (new open source security firmware)
      - does open ipe targe specific hardware - says its quite generic but can be attapted easily e.g specific registers
        -  -->

A talk describing the the work done to expose security flaws in TI microcontrollers and a proposed solution to fix some of them. The speaker started by giving some back ground on TI. They were the first company to make a silicon transistor and apprently worked on the first laser guidance system. They then talked about the investigation they had done on the MSP430 microprocessor. The MSP430 has phsyical tamper protection as well as a memory protection unit (MPU). It als has a Intellectual Property Encapsulation (IPE) system which was the focus of the investigation. Since the processor has a flat address space memory is protected by writing markers to certain addresses. Code running outside the marked address range cannot access code and data inside. The speaker then said that this system isnt very secure and most attack methods they tried succedded. One such method was specific to this device which he called controlled call corruption which can corrupt memory anywhere in the protected region. In response to this they proposed the MPU could be re-purposed to protect against attacks like this however they didnt say exactly how. In general the speaker said that most bugs come from hardware and its difficult to do research on hardware issues as its closed source (as well as any code shipped with the chips in ROM). This changed however when openMSP430 was released which is n open source implementation of the MSP430 cores which is close enough for research. From looking at this they proposed openIPE which is an open soruce version of the propietory boot code on TI devices that sets up the security features. The speaker claimed they had already mitigated a interupt timing attack using this new implenentaion. The goals of openIPE are,
- provide a unified memory isolation implementation
- allow rapid prototyping of security fetaures
- have thorough testing
The speaker was asked if openIPE targets specific hardware to which he replied its quite generic but can be attapted easily e.g with the specific registers.



### The Ultimate Office Chair: Hacking a BMW Comfort Seat with an ESP32 (UD2.120 10:30 - 10:55) 

<!-- - guy works for bmw
- g series seat, control logic is on the seat itself
- why not
- x14 connector
    - 2 CAN buses
    - 12v inputs
    - hearest has small explosive device and there is small side airbag so need to remove them before using as office chair
- CAN
    - bmw cna2.0a
        - 500kbits baud with 2 or 8bye messages
    - - no encryption
        - can use SocketCAN to connect
    - to connect need to wire both
    - showed example in rust of of simpler setup (bmw z controller thingy)

- seat has multipe ecus
- seat powers on for 5 seocnd initllaily then powers down
    - mutiple vehicle states that need to be correct before the seat stays on
    - need to simulate ZGW gateway
- seat draws 5A while moving
- used a getway (car) as a debugger to work out the protocol overthe CAN bus
        - main messages were neytwrok changes, Active ecus, PWF (needs conatsly to stake awake)
        - went over the process of parsing the CAN messages (need to revrse engineer some ids form the gateway)
        - need to send the VIN as well interestingly every 100ms
        - in summary the seat needs sevral reating messages to keep ECUs awaye and then the messages to activate massge, movment etc

- chair looked pretty cool
- connected to esp32 to control chair over wifi

- he broke the rear led light bar while dissambling -->

A talk about how a guy took a BMW seat and used it for his office chair. This talk was packed. The seat itself was a G series seat from the more expensive cars and had multiple ECUs with all the control logic on the seat itself. It has an X14 connector with 2 CAN buses and multiple 12V inputs. The speaker also noted that the seat contains a side airbag and a small explosive device in the head rest which should be removed safely before the seat is placed in an office. The seat uses CAN2.0a with a 500kbit baud using 2 or 8 byte messages. It also has no encryption and strangely you need to have both CAN ports connected before it will do anything. The speaker said you can use an application such as SocketCAN to connect and showed a quick example of so rust code commpunicating with a BMW Z dashboard controller. He then talked about the process of listening on the CAN bus to work out what messages were needed to allow communication with the seat, most of which required alot of reverse engineering since theres not much offical documentation. Some interesting quirks were,
- On first startup the seat powers on for 5 seconds then powers down. It then requires an additional command to start up again
- There are multiple vehicle states that need to be correct and constantly sent to the seat for it to accept any movement commands. A wierd one was the cars VIN which needs to be sent every 100ms.
- To send require messages you need to simulate the ZGW gateway interface that exists in the car usualy.
He finished off by showing some pictures of it in his office which looked pretty cool and explained that he has it connected to his home network via an ESP32. He also said that the chair has rear leds but he accidentaly broke them in the dissasembling process.

### Understanding Why Your CPU is Slow: Hardware Performance Insights with perf-go (UB5.132 11:00 - 11:30)

 <!-- - how far cpus have come
    - cpus now have around 20 cores, 
    - ~ 3ghhz
     - big caches
- read times
    - L1 cache 1ns
    - ram 100ns
    - nvme 10 us
- cpu cache
    - each core has deicated l1 and l2 cache
        - L3 cache usually shared across cores
    - cache lines 64bytes 
    - cpu tries to pre fetch so its ready in cache

- can write cache friendly code
    - temporal locality - accessed once so will likely access again
    - spatial locality - access at adress so could liekly acces form there again
    - design data structures with the above in mind
-   - in go struct of arrsys will benefit simd instrctions as an array of structes is likely to be larger than the cache line
        - liekly that internal arrays in structs will be cached

- branch prediction
    - CPU piplining
        - instructions processed in stages
            - fetch - decode -> execute -> memory access -> register write back
        - can run different section sof thepipeline  simultaneously so can processmutipe instructions in parralel
        - moderncpus have 10 to 20 stages
    - code with branches makes it harder to pipeline as instructions cannot be pre fetched (pipeline must be restarted if a prediction is wrong)
    - showed example of bad and good Go code for branch prediciiton
        - taking a number and incrementing two counters in an if. Much more efficent if we calculate both negateive and positive at the same time to make code predictable

- how cna you measure this
    - modern cpus have hdedicated hardware counters
        - cycles, instructions cache hts and misses, branches and misses
    -   - hard to get this info for VMs

- perfgo
    - tool to automate benhcmark workflow
    - wrappe for perf and pprof to make performance benchmarking easier

- showed demo of tool
    - comparing branch prediction performance on the two perviosuly mentioned code examples -->

An interesting talk disccussing how you can write your code to make better use of CPU cache lines and branch prediction. The speaker started by talking about how far CPUs have progressed in recent years and gave a quickl overview of the cache hierachy. Modern CPUs have around 20 cores with a clock speed of about 3GHz. Each core has dedicated L1 and L2 cache with a larger L3 cache usually shared across cores. Cache lines are generally 64 bytes and the CPU tries to pre-fetch so data is ready in cache. Read times in the cache hierachy are about,
- 1ns for L1 cache
- 100ns for RAM
- 10 us for NMVE drives
He next talked about how you can write cache friendly code. Data structures should be designed with spatial and temporal locality in mind, e.g if some data was accessed once it will likely be accessed again at that same address. An example given of this was that in Go using structs of arrays will benefit SIMD instructions as an array of structs is likely to be larger than the cache line and its likely that internal arrays will be cached. Next he talked about branch prediction and how pipelining works in the CPU. Instructions are (aproximately) processed in a cycle of fetch, decode, execute, memory access, register write back. Multiple instructions can be processed in parrallel as you can run different sections of the pipeline simultaneously. Code with branches makes it harder to pipeline as instructions cannot be pre fetched (an incorrect branch prediction means the pipeline has to be discarded). He then showed an example of a good and bad code snippet, one of which avoid branches and so could make better use of pipelining. The code involved taking a number an incrementing a two counters if the number was positive or negative. Its much more efficient for the CPU if both counters are incremented at the same time to maek the code predictable. The speaker finished off by talking about a tool he had created for better measuring performance of Go code. Modern CPUs have dedicated hardware counters that keep track of cycles, instructions, cache hits and misses, branches and misses so the information is readily available. He presented his tool perfgo which was a wrapper for the perf and pprof tools but made it a bit easier to compare benchmarks. He ended witha demo of the tool comparing the two previously presented code examples. For the bad case you would expect a branch miss around half the time however it was actually only about 40% as CPUs are super smart nowadays.



### skiftOS: Building a microkernel-based operating system from the ground up (K.4.201 12:30 - 13:00)

<!-- - general purpose micor kernel in c++
- still early stage 

- build
    - cutekit build system (built form scratch)
        - cargo inspired c++ packge manager and build system

- boot
    - karm
        - freestanding core chared libary between all componnts of the OS
        - writtn in karm flavoured c++
            - c with lambd modules and corouteins, no exceptions
            - need unsafe block for raw ptrs
        - opstart
            - efi bootloader
            - custom boot protocol
            - uses same ui as rest of OS
- Hjert micro kernel
    - core kernel functions
    - 4k lines of code (simply)
    - showed exmaple of syscall

- strata services 
    - handles userspace applications
        - shell,
        - fs
- hideo desktop
    - custom display protocol
    - similar to wayland
    - scales wellon all screen sizes

- has its own browser vael
    - html andcss only
    - no js yet

- gave demo of desktop envionrmnet
-   - look very imprssive until it crashed in 
    - runs doom - mgreat success

- gave demo of karm starting the broswer rather than boting the OS

- future
    - networking
    - namespacing
    - on disk filesystem
    - sound server
    - html/css in the bootloader

- quesions
    - runsonly on x86 at the moment
    - havent tried it on real hardwareyet
    - what security does it have - page tables and virtual memory, task domain system
    - will ui libary run on linux - yes -->

A talk about a general purpose operating system written from scratch. I was very impressed with this talk as the speaker real had impleneted every single step themselves which seems rare nowadays. It was also intentionally not a POSIX compatible which was a nice change. The talk went over the general architecture and ended with a demo. The project used a custom build system called cutekit (also written by the speaker I beleive) which is a C++ cargo inspired package manager that uses JSON for simplicity. Another interesting feature is that all stages of the OS use a freestanding core shared libary called Karm which, as the speaker described it, is written in Karm flavoured C++. This was bassicaly just c with lambda modules and coroutines, no exceptions unsafe block syntax for raw pointers. They next went over the four stages pf the OS. The boot stage uses an EFI bootloader replacement called opstart which has a custom boot protocol and uses the same UI as the rest of the OS for consistency. Next they talked about the kernel which uses a micro kernel architecture and shoed and example of a system call. The kernel is only 4k lines of code which I thought was cool. Above the kernel they have what they called the Strata services layer which handles userspace applications as well as the file system. Finally at the top there is the Hideo desktop which uses a custom display protocol. The desktop is similar to wayland and scales well on all screen sizes. The OS also has its own browser which in its current state only handles HTML and CSS. They gave a demo of running the OS on QEMU which looked very impressive until it crashed. They also showed it running Doom which is a nessecity in my opinion. They also gave a demo of the bootloader running the broswer directly without loading the OS which was neat. The talked finished up by talking about the future plans for the project. Some of the planned features are:
- networking
- namespacing
- an on disk filesystem
- sound server
- html/css in the bootloader
- Support JS in the browser

Some of the questions asked were,
- Does it run on real hardware? -> A: They havent tried it yet but should in theory
- Does it run on other CPU architectures? -> A: Only x86 at the moment
- What security features does it have? -> A: Page tables and virtual memory and a task domain system.
- Will the desktop UI libary work on linux -> A: Yes

<!-- ### Systems Programming: Lessons from Building a Networking Stack for Microcontrollers (UB5.132 14:30 - 15:00)

- ether-switch
    - relied on recieving a packet to send one
    - tcp stack was stateless
    - code was bad apprently
- seqs
    - doenst need to recive packet to send
    - only http netwokring stac for rasberry pi pico

- Lneto - networking stack in go for microcontrollers
    - showed basic interface example
    - more maintainable but does use sligthl more memory than ether-switch

- lessons learned
    - dont use heap allocations
    - avoid globabl mutable state - stack allocate data structures
    - compose structs with non pointer types
    - make the zero value useful
        - state tcp closed represents no connection at all
    - keep api synchronus
    -  -->

### Why build an 8-bit homebrew computer in 2026 (H.1302 15:55 - 16:15)
- inspired tobuild is own 8bit 6502 computer
- ram bit bottle neck on old computers
- speaker wanted computer to support alot of memory
- wanted it to be introducible
- minitel output (classic terminal screen)
    - 40 or 80 columns

- wants
    - 6502 cpu
    - 32kb ram
    - support an atari controller - simple controler dry contacts
    - one channel buzzer

- showed memory map on tiny screen
-    - left 8kb available via cs line to run simple roms
    - 16kb for internal rom
- computer has boot menu
- no 6502 assmbly implementaion for menu system so he wrote libary
- in 6502 if nothing registered on at memeory address then high is returned
- menu
    - first 8btes of rom are the menu
    - program therfor has to start at the 9th byte
- controller
    - all lines pull low when correspodning button pressed so can pul all high by defualt 

- encourgaed to build own becuase
    - error must come from hardware issues or bad logic - very simple

- future
    - loading from cassete


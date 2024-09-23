# Setup and Resources
1. [PLC Fundamentals (Level 1) Course](https://www.plcdojo.com/courses/take/plc-fundamentals/lessons/15202842-course-intro-curriculum-objectives
2. [Lessons In Industrial Instrumentation - Tony R. Kuphal](https://www.ibiblio.org/kuphaldt/socratic/sinst/book/liii.pdf)
3. [PLCDev.com](https://www.plcdev.com/)
4. ...
# Introduction
There are three files to download for this course:
- RSLinx Classic![[Pasted image 20240918133856.png]]
- RSLogix Emulate 500![[Pasted image 20240918133915.png]]
- RSLogix Micro![[Pasted image 20240918133936.png]]
- (Optional) FactoryTalk View Studio
	- Factory talk view studio is where you CREATE applications for your process. Factory talk view ME station is where you RUN the application you created in factory talk view studio

Looks to be A PLC Emulator, the drivers for that PLC Emulator and the link between PC and PLC

There are also two PDF's that go along with these downloads:
- [[MicroLogix 1400.pdf]]
- [[PLC_procedure.pdf]]

The Three Programs after installation are:
- RSLinx
- RSLogix Micro Starter Lite
- RSLogix Emulate500

When Stopping a file, you must Hault (HLT) then click File>Close

Programs communicate as such
**RSLogix** <-> **RSLinx** <-> **RSLogix Emulate500**
![[RSLogixComms]]
## RSLogix 500 vs 5000 vs Studio 5000
- RSLogix 500
	- Supports AB MicroLogix processors
	- Mid Range PLC
- RSLogix 5000
	- Support AB CompactLogix processors
	- Does not have a Free or Trial version
- CCW - Connected Components Workbench
	- Supports Micro800 Processor 
	- Low Range PLC 
## PLC Programming Languages
Some types of languages:
- Function Block Diagrams 
- Instruction Lists 
- Ladder Logix
- Graph Programming
- Structured Text 
	- Akin to C or Assembly code
# PLC Programming Overview
## Objectives:
- PLC Role in Automation
- RSLogix 500 Programming Environment 
- Ladder Logic
- PLC Program Flow and what it does
- Begin looking into Process as Inputs, Conversions, and Outputs
## PLC Automation
Early Automation was using Relays for automation. These have become Programable Relays and PLC's

PLC's are fully scalable controllers.

Why aren't we using typical PCs to control computers?
- Computers tend to be instable (Crashes, Power Spikes, errors)
- PLCs are built to be very robust and simple by design to be less prone to crashes and errors
## PLC Program Flow
- When you create a PLC Program, you start with Ladder 2 - MAIN
	- This is your MAIN function
- You do not want all of your code in One ladder
- Code will run sequentially starting from Ladder 2 -MAIN RUNG 0 then RUNG 1 then RUNG 2 … unless there is a Jump command
- Every time a PLC runs through a Ladder such as Ladder 2, this is called a Scan
> [!Important] Inputs, Conversions and Outputs
> You should think of how a machine and a process works, it is essentially Inputs, Conversions and Outputs
> ![[Pasted image 20240918154613.png]]
# Inputs and Outputs (IO)
## Outline:
1. What is IO
2. Why is IO important
3. What is Digital and Analog IO
4. What types of IO modules are there?
## IO Overview
- IO stands for Input and Outputs. When working with PLC's, Inputs are manipulated to produce Outputs.
- The PLC will use a program to decide what action to take, when and how for its control of the devices attached to it
## Digital IO
- AKA Discrete IO
- These are devices that send a Boolean signal such as On/Off, Open/Closed, 1/0, True/False
- These signals only have two states, they can never be in between
- Digital Outputs are devices that work with Boolean rules. A solenoid is either energized or de-energized,
## Analog IO
- Analog devices have many different states expressing varying levels of precision
- For example, you can have many different voltages, percentages...
- Analog output devices include heater coils, motor drives
## IO Modules
- An IO Module is an addon to the PLC to send or receive inputs and outputs
- Sometimes these modules will be specialized based on the device it needs to interface with
- PLCs then become very scalable depending on your needs
	- As long as there is room on the DIN rail, you can add more if needed
# Programming Fundamentals
## Objectives
1. Basic Datatypes
2. Data Tables and Addressing
3. Flow of Rungs and Branches in a Ladder
4. Program Instructions 
5. PID Control Loop
## Data Management
In the Project Folder, you will find a "Data Files" Folder. Internal Management of PLC data happens here.

There are many different data options the PLC can store:
1. **Binary!**
2. **Integers!**
3. **Float!**
4. Timers
5. Control
6. String
7. Long
8. Message
9. PID
10. and More...
### Data Tables 
Everything you do in RSLogix gets a Memory Address and Location. 

Binary is B
Analog is N and F
- N is Integer
- F is Float

A typical structure of a memory register and IO address will have the following:
`[TYPE][FILE(if applicable)]:[ROW or WORD or SLOT][/ or .][BIT or WORD or CHANNEL]

For example: `I:2.2`
TYPE: is `I` meaning this is an INPUT
FILE: is not applicable b/c all inputs are stored in the same file
SLOT: (since we are addressing IO) is `2` meaning this module is the second module to the right of our PLC 
PERIOD: `.` because we are addressing a word meaning that this is an analog input address
CHANNEL: (since this is an IO address) is `2` meaning this sensor is plugged into the third (0-1-2) position of the module 

Example: `B3:12/15`
`B` - This is a BIT type (True or False)
`3` - The File is #3
`12` - The WORD Field. In a bit file, each line contains 16 bits to make up a word. The `12` here means this is looking at the 13th word in that file
`/` - because we are addressing a bit, this is storing true or false data
`15` - because this is a bit address, `15` is referring to the 16th bit in the word

> [!tip] WORD
> Below is an example of what a WORD is in the context of datafiles:
> ![[Pasted image 20240919150746.png]]


Example: `O:4/8`
`O` - Output
`4` - Slot is the 4th module to the right of the PLC 
`/` - Addressing a bit, digital output address
`8` - device is plugged into the 9th position of the module

Example: `T4:3/DN`
`T` is a Timer

## Condition and Outputs (Left to Right)
- Programs run top to bottom, 0 down
- Rungs run from left to right
- White space in the middle divides the conditions from the outputs:
	- 0000-Conditions-Outputs
![[Pasted image 20240919144959.png]]

If the condition is not met, the PLC will skip to the next row

You cannot have two actions on the same branch: you cannot have an Output followed by another output. Actions need to be on separate branches

Its best to think of ladder logic as an electrical circuit:
- AND is Serial
- OR is Parallels 
## XIC, XIO, OTE
### XIC - Examine IF Closed
![[Untitled.png]]
Examine this statement if the circuit is closed 
### XIO - Examine IF Open 
![[Untitled-1.png]]
Examine this statement if the circuit if Open
### OTE - Output Energized
![[Pasted image 20240919150536.png]]
This will take a bit within the datafiles and set it to 1 to keep the bit as a value of 1 as long as the condition is true

This is the preferred method for setting a bit to 1 or 0
## OTL and OTU
These are other methods for setting a bit
### OTL - Output Latch
![[OTL.png]]
When the condition becomes true, then the output will energize and latch in that position. Essentially will sticks on. This will remain latched until it receives an OTU - Output Unlatched
### OTU - Output Unlatched
![[OTU.png]]
This command will Unlatch a previously latched output
### Best Practice Programming 
RSLogix will not let you energize an Output in many different placed. However, it will let you energize in many different places using Latching and Unlatching.
> [!warning] Energize in ONE spot and ONE spot only 
> This might seem convenient, but it will create many issues down the road. 
> 
> **You will want to energize a bit in ONE spot in your program**
> 
> Latching in multiple spots creates a nightmare when attempting to debug where output energizing is happening.
## ONS, OSR, OSF
### ONS - One Shot
This comes in handy in the case where you want a light to turn on once, even if you were to hold the button down. This will trigger exactly one time. When this is triggered, the ONS will trigger for exactly 1 scan and then will reset itself. One time instantaneously. 

The One Shot `ONS` will handle 90% of the PLC needs. Depending on the processor, some processors will prevent multiple instructions to the right of a `ONS` and some prevent branching around a `ONS`
### OSR - One Shot Rising
Acts as an Output Instruction. When this is triggered, The output bit acts exactly as the One Shot `ONS`, the storage bit will stay on with the light switch until it is turned off.
### OSF - One Shot Falling
Acts as an Output Instruction. This works opposite to the One Shot Rising. The One Shot Falling will trigger when the condition becomes false, then the output bit will trigger for one scan and the storage bit fires and stays energized until the light switch is closed again
### The Storage Bit
- OSR: False to True Transition
- OSF: True to False Transition
![[OneShots2 1.jpg]]
## TON, TOF, RTO
Sometimes you want things to happen on a delay.
### TON - Timer ON (Delay)
![[Pasted image 20240919162449.png]]

Timers work as such for TT, EN, DN
![[Pasted image 20240919163304.png]]

If everything to the left of a timer is true, the timer is considered as "Energized". If the timer becomes de-energized, it will reset its accumulator 

A timer has the following fields:
- EN - This is the bit that turns on when the timer is currently energized
- TT - This is the bit that turns on when the timer is currently timing or counting
- DN - This is the bit that turns on when the timer has completed counting and has counted "DOWN"
- Timer Base - This is the unit base that the timer uses such as seconds or milliseconds. 
- Preset - This is the timer value, what the timer is counting up to. Multiply by the base to determine the timer limit (5 x 1.0 = 5 seconds)
- Accum - This is what is the accumulated number. Usually 1.
### TOF - Timer OFF (Delay)
Timer will start when its conditions are False and will start to count exactly like TON
### RTO - Retentive Timer ON
The same as TON Timer On, but it is a retentive timer on. Retentive timers require a reset in order to reset the timer's accumulator. Holds onto timing data when de-energized. 
## CTU and RES
### CTU - Counter UP
- Counter that counts up. There is also a CTD for Counter DOWN. 
- The counter counts every time that the condition becomes true (called transitions)
- Counters have CU for Count Up Bit and a DN for Done bit. CU bit typically energizes every time a count occurs (if scan time is 6ms, then occurs for 6ms). 
- DN turns on when the counter is Done. 
- Counters need to be reset to turn them back to 0. This uses the RES command
## Comparators
These are logic statements for comparing two pieces of information:
- Greater Than (`GRT`)
- Less Than (`LES`)
- Greater Than or Equal To (`GEQ`)
- Less Than or Equal To (`LEQ`)
- Equal To (`EQU`)
- Not Equal To (`NEQ`)
- Limit Test (`LIM`)
	- This is a test of a value to be compared between a low and high limit
	- This is Inclusive (`GEQ` and `LEQ`)
	- You can revert the Low and High limits to revert the polarity
### Limit Test Example (`LIM`)
![[Pasted image 20240920134934.png]]
This is a Limit Comparator that compares an Accumulated value, `C5:0.ACC`, with a Low Limit of 4 and High Limit of 7. This is inclusive, so this condition will be true if the Accumulated value is between 4-5-6-7
In Math notation, this would be: `[4,7] = 4,5,6,7`

If you were to switch the Limits such that: `[7,4]` for a Low Limit of 7 and High Limit of 4, this is a valid condition. This will reverse the `LIM` condition so that it is an exact opposite of the original condition. Therefore, the condition will become true when it is any number outside of the range specified:
```
[7,4] = INF -> 4, 7 -> INF
Example: 0,1,2,3,4,7,8,9,10 will cause the condition to be true 
```
**The condition will not be true if the the Accumulator is either 5 or 6**
## Mathematical Operators and CPT
These are statements for performing math calculations:
- Add (`ADD`)
	- Typically comparing two constants will not work, RSLogix would like to use memory locations
- Subtract (`SUB`)
- Multiply (`MUL`)
- Divide (`DIV`)
- Compute Result (`CPT`)
	- Useful for more complex math. 
	- Allows for use of Expressions in one clean block
	- Example:
		- ![[Pasted image 20240920140424.png]]
> [!Tip] Math Operators and Constants
> Typically RSLogix will not let your program compile if you use two constants in a math function. Example: `ADD(4,5)`. RSLogix prefers to use memory locations as inputs
## Scale with Parameters (`SCP`)
This instruction will take an input that is defined against one scale and translate it according the a different scale.
`**May need to confirm math on this one`
![[Pasted image 20240920142120.png]]
Formula for Output Scale value is below:
$$
\text{Output} = \left( \text{Input} - \text{Input}_{\text{min}} \right) \cdot \left( \frac{\text{Output}_{\text{Max}} - \text{Output}_{\text{Min}}}{\text{Input}_{\text{Max}} - \text{Input}_{\text{min}}} \right) + \text{Output}_{\text{Min}}
$$
```
Scaled Value = (mA Input Value – Input Min) * ((Output Max – Output Min) / (Input Max – Input Min)) + Output Min
```

This can also be visualized as: 
$$
\text{Output} - \text{Output}_{\text{Min}} = \left( \frac{\text{Output}_{\text{Max}} - \text{Output}_{\text{Min}}}{\text{Input}_{\text{Max}} - \text{Input}_{\text{min}}} \right) \cdot \left( \text{Input} - \text{Input}_{\text{min}} \right)
$$
## Move (`MOV`)
This will move a value from one memory location to another. It is essentially a copy command (however, there is a copy command)

A couple of good use cases are:
1. If you would like to set a time value ahead of time:
![[Pasted image 20240920145928.png]]
2. Resetting a value back to 0 for a counter or timer
3. Recasting - When you convert from one data type to another
## Jump (`JMP`) and Label (`LBL`)
Allows the PLC to move to a different rung (label) to run logic there
### Jump - `JMP`
- Allows the PLC program to jump to a location and continue to execute from there. Almost like a skip or function block command 
- Any jump is addressed as a Q
	- E.G. Q2.0 - Program File 2 and Label 0
### Label - `LBL`
- A rung with a Label (as its condition) will not run unless it is jumped to
- For example, if you have a label condition, the PLC will skip over it unless there is a Jump command to that spot
- This allows you to use this almost as a function or as a bookmarker
	- Function - Jump command all by itself, will skip if not jumped to
	- Bookmark - Place a jump command ahead of it if it is something you want to be run every time but also have the ability to return to this spot 
## PID - Proportional Integral Derivative Control Loops

### What is PID?
 ![[Pasted image 20240920154016.png]]PID stands for Proportional-Integral-Derivative, which is a type of control loop feedback mechanism commonly used in industrial control systems. Here's a breakdown of its components and how they work together:
### Components of PID Control
1. **Proportional (P)**:
    - The proportional term produces an output that is proportional to the current error value.
    - **Error** is the difference between a desired setpoint and the current process variable.
    - A larger error results in a larger control output, which helps to reduce the error quickly.
    - However, solely using the proportional control can lead to a steady-state error.
2. **Integral (I)**:
    - The integral term focuses on the accumulation of past errors.
    - It integrates the error over time, which helps eliminate the steady-state error by adjusting the control output based on the accumulated error.
    - If the error persists, the integral term increases, driving the system to correct the offset.
3. **Derivative (D)**:
    - The derivative term predicts future errors based on the rate of change of the error.
    - It provides a damping effect, which helps to stabilize the system and reduce overshoot by reacting to the speed at which the error is changing.
    - This can help to smooth out the response of the system.
### PLC Logic
![[Pasted image 20240920154327.png]]
A PID will receive a setpoint (either inputted or hardcoded), that value tells the PLC what setpoint it should try to maintain on the control and process variables. There are three elements: Setpoint, Process Variable, Control Variable. 

The idea is to chance or control the Control Variable in order to get the Process Variable to line up with the setpoint value. 

For example, if we would like to maintain a temp of 72 degF in our home, we will control the A/C Compressor Power in order and watch the thermostat as our Process Variable 

Below is an example of the PID Setup Screen:
![[Pasted image 20240920155250.png]]
## Instruction Sets and References

# Program Setup
## Setup Program and Processor
- Start by opening RSLogix500
- New File Btn -> All Processors that are compatible with the IDE
- RSLinx is a program that manages network and connectivity between PLC, HMI, Servers...
- Who Active will bring up a smaller version of RSLinx to see what is available for communication
	- For this example, we will connect to the MicroLogix 1100 that is not currently running. This is the network location that the program will comm with by default
![[Pasted image 20240923121654.png]]
- Ladder #2 is our default program, we will rename this to "MAIN": 
![[Pasted image 20240923121807.png]]
- Next we will want to configure our controller: Controller Properties
	- Here we can change the driver from an Emulator to a live PLC
![[Pasted image 20240923121947.png]]
## Setting up Module Configurations
- Click on IO Configuration
![[Pasted image 20240923122142.png]]
- We will be setting up the PLC to be able to integrate with the Inputs and Outputs
![[Pasted image 20240923122238.png]]
- We will then select the BWA Base and 16pt
![[Pasted image 20240923122436.png]]
- In the Emended IO Configuration, this is where we can set filters on inputs to prevent jitter comming in, typically this will be set to 1 mS for Inputs and typically 60 Hz for Analog inputs
![[Pasted image 20240923122637.png]]
- Table to the right of the I/O configuration are all of the compatible IO modules with this 110
	- We will add an Analog 4 Channel Input (1762-IF4) to slot 1
	- Double click and see what config can be done
![[Pasted image 20240923122949.png]]
![[Pasted image 20240923123343.png]]
## Scaling and Resolution
Overview of Bit levels: 
![[Pasted image 20240923123829.png]]
- Bits are very very important for working with PLC's 
- When we work with a MicroLogix processor, we have a 14-bit resolution
	- 14 bits = 2^14 = 16,384 levels
- Example:
	- If I scale a 10-bit analog signal to a float with a range of 0-100, how many decimal places should I display to utilize (almost) all available precision?
	- A 10 bit signal is 2^10 for 1,024 levels
	- A range of 0-100 has 101 levels
	- If we were to add a single decimal point, this will increase that range by a factor of 10 (0.1, 0.2 ... 0.9)
	- That means with the addition of a single decimal place, that takes us to: `100*10+1=1001` where 100 is the range limit, 10 is the number of decimal places between 0.0 and 0.9
	- If we increase it to 2 decimal places, that means that between 0 and 1 we have (0.00,0.99) for 100 extra levels to keep track of.
	- therefore: `100*100+1=10,001`
	- Our 10-bit float will only hold up to 1,024 levels which is close to the single decimal point
	- We would need to use at least a 14 bit float to hold the range needed for 2 decimal places (0.00,100.00)
## Function Files
- Function files are inherited from the type of processor
- There are a couple of important ones to cover:
	- `HSC` - High Speed Counter
		- Receives high frequency inputs (up to 20,000 Hz)
	- `STI` - Selectable Timed Interrupt
		- Allows us designate one of the program files and scan one of those program files more often than the rest OR to call it out of order
		- If we need one ladder to execute at one specific time without needing to JUMP to it, 
	- `RTC` - Real Time Clock
		- A wall clock that runs inside the PLC that can be set to program around time
		- If you need a program to run at a certain time of day such as 9am, this is how you would do it
	- `...` - There are More as well
## Program Files
- These are where the ladder logic files are stored
- LAD2 is what the PLC will continuously scan
	- Typically all subroutines will be called from the MAIN ladder 
- There are no typical naming conventions with how it should be named
	- These could be: `ALARMS`, `IO`, `CONTROLS`
	- ![[Pasted image 20240923130906.png]]
	- Could also be: `DIGITAL INPUTS`, `ANALOG INPUTS`
## Adding and Expanding Program Data Files
### Adding More Program Files
- Covering what happens when you would like to add and expand your program files
- `JSR` - Jump to Subroutine
	- `JSR - U:3` is a Jump to Subroutine or Ladder File #3
> [!Important] Expansion
> As soon as you add a new Program File, **Immediately** add a `JSR` (Jump to Subroutine) command so you do not forget
### Managing Data Files
![[Pasted image 20240923133121.png]]
- When you define a spot in memory (E.g. an Examine if Closed for B3:0) when you open the Data File B3, you will see the :0 memory spot with that symbol:
	- ![[Pasted image 20240923133510.png]]
- If you need more than 16 bits, 
	- If you open the properties for the Binary file, instead of 1 element, set to 10 elements.
	- This will change your memory location from 16 bits to 16x10 = 160 bits ranging from B3:0 through to B3:9
	- ![[Pasted image 20240923133737.png]]
	- There is a limit to how many elements you can have in a B3 data file
	- If you go over the limit, you can create a new data file to define a new file slot and data type
	- ![[Pasted image 20240923133953.png]]
# IO Programming
## Objectives
1. Practical Considerations when integrating IO
2. Best practices for IO Programming
3. Processing Digital IO
4. Processing Analog IO
5. Understanding LL, L, H, HH in Analog Process Control
## Programming Digital IO
This is the IO Configuration being used for this section of programming:
![[Pasted image 20240923142601.png]]
- Description is human readable for program, symbol is mainly used when setting up HMI
- `I:1/0` - Input, Module 1, Channel 0
- `B3:0/0`
	- ![[Pasted image 20240923144050.png]]
	- Best practice is to setup program such that the output energized relies on a XIC input is because input mapping can change. We need a middle man piece of logic such that if the I/O mapping changes, then the change happens here and can be easily managed in one spot
- ![[Pasted image 20240923144411.png]]
	- This is what is called input mapping, mapping an input from the IO module to a bit location in memory 

Below is an example of how Inputs and Outputs (Segregation of IO) can be mapped:
![[Pasted image 20240923145002.png]]
## PROPER Digital Control Logic
- There are three items to control a Digital Device:
	- Trigger (Something that starts the cycle)
		- True for only ONE Scan 
		- Energized our digital and then immediately looses its control of the process
		- Only fires once for each time the trigger conditions are present
	- Hold-In (keeps it energized)
		- This is what keeps the circuit going once it is triggered
		- The Hold-In cannot start or stop the circuit
		- The digital device being controlled is its own hold-in
	- Interrupt (Something that breaks the cycle)
		- A condition in series with the hold-in (same branch)
		- Its only purpose is to break the circuit from the hold-in
		- Cannot start the circuit or keep it going 
![[Pasted image 20240923163921.png]]
- Hold-In does not have a place in Memory
	- Device we are controlling will hold itself in
### DCL Circuit Example:
![[Pasted image 20240923165246.png]]
1. Start button is pressed
2. One Shot is activated 
3. Trigger Bit is activated for one scan
4. During this scan, because the trigger bit is energized, this condition is true
5. The Digital Device activated
6. Next time through scan, the trigger bit is no longer energized, but the digital device is, therefore this condition is true and circuit repeats. This is the Hold-In portion of the logic
7. If the stop button has not been pressed, then the circuit continues. If the stop button has been pressed, then this condition becomes false and (XIO->1 ->F) and this rung does not trigger

In this example, the start button can be swapped out with any sort of control logic. Likewise, the stop button or Interrupt can be swapped with logic as well:
![[Pasted image 20240923165839.png]]

In its essence, Digital Control Logic template is:
![[Pasted image 20240923170256.png]]
### Additional Notes on Digital Control Logic
From instructor Notes
![[Pasted image 20240923170436.png]]
![[1596816449975.png]]
![[Pasted image 20240923174252.png]]

Following is code snipped for rungs:
```
XIC B9:0/0 ONS B9:0/3 OTE B9:0/5
XIC B9:0/1 ONS B9:0/4 OTE B9:0/6 
BST XIC B9:0/5 NXB XIC B9:0/2 XIO B9:0/6 BND OTE B9:0/2 
```
### Dead-wrong, Crappy and Garbage Digital Controls
#### Careful with Latching
![[Pasted image 20240923170926.png]]
We need to be careful when we use latching as if we latch a device in different programs, there is a possibility that the device will perpetually be triggered.

It also makes troubleshooting a nightmare when you try to determine which latching logic is triggering the device.

Latches should be used for permanent bits that should be always energized such as for testing setups. Should be something that should Always be energized. The same with Unlatched, such that you only have the unlach or latch command on a single rung and no where else. 

Almost as an Always TRUE or Always FALSE Constant. Do not use Latching and Unlatching for controls. Use OTE for this. 
#### Dangers of not using Digital Control Logic
In the below example, the programmer would like for a proximity switch to activate a motor whenever it is energized:
![[Pasted image 20240923172314.png]]

This has caused the input devices to be in direct control of the output device. 

This may look like a simple solution, but what happens if the Proximity switch is jammed? The motor will run forever and burn out. 

The challenge here is to think through all the ways that your program can fail. The purpose of the Digital Control Logic is to provide a "middle man" to make sure that something that needs to be controlled is being controlled correctly
### Programming Analog IO

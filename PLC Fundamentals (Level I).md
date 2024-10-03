# Setup and Resources
1. [PLC Fundamentals (Level 1) Course](https://www.plcdojo.com/courses/take/plc-fundamentals/lessons/15202842-course-intro-curriculum-objectives
2. [Lessons In Industrial Instrumentation - Tony R. Kuphal](https://www.ibiblio.org/kuphaldt/socratic/sinst/book/liii.pdf)
3. [PLCDev.com](https://www.plcdev.com/)
4. [Data File Management Sheet by Adam Brigmann](https://docs.google.com/spreadsheets/d/1APY6ZYkW5Ri102nrqk3s50NMDwLXUXLQWyU8Em5qayI/edit?usp=sharing)
5. ...
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
## Programming Analog IO
- Adding Ladders: Digital IO and Analog IO
	- Call Ladder from the Main using a `JSR`
- Adding an Analog 2 Channel Input
	- Analog 2 Channel Input, 2 Channel Output (`1762-IF2OF2`)
	- All as 4 to 20 mA signals
	- Scale for Raw Prop for Input channel 0 and 1
	- Scale for PID for for Outputs channels 0 and 1
	- ![[Pasted image 20240924113036.png]]
- Back over to the Analog IO ladder logic 
	- First thing we want to do is scale with Parameters `SCP`
	- First Rung
		- I:3.0 - Channel 3, Dot for word level, and 0 for the channel
		- Input Min - 4,000 (Correlates to the 4 to 20 mA signal)
		- Input Max - 20,000 (Correlates to the 4 to 20 mA signal)
		- Scaled Min - 0 (PSI)
		- Scale Max - 5 (PSI) (we are compressing 16,000 levels of resolutions down to 6 levels) 
		- Output - F8:0 for the memory location as Pressure (0-5 PSI)
	- We may also like to put in some conditional processing in case our pressure sensor is out of range
		- Add branch
		- On other branch, add a `LES` command and test of `A=I:3.0<B=4000`
		- If true, we will use the `MOV`  to move the minimum output of 0 to the stored pressure location of `F8:0`. Now this memory location will never read as a negative value
	- We may also like to filter out if the pressure is greater than the input max
		- Add a branch to the above
		- Use a `GRT` greater than command to test for `A=I:3.0>B=20000` and then a `MOV` command to move 5 to the F8:0. 
	- Suppose we want to see what percent of the pressure sensor is of the max
		- Extend branch up
		- Add `SCP` and 4000-20,000 scaled to 0 to 100 and store as a float or int
- ![[Pasted image 20240924120216.png]]
## Analog Process Control (LL,L,H,HH)
There are 4 levels for Analog process control limits:
- LL - Low Low Limit
- L - Low Limit
- H - High Limit
- HH - High High Limit 
![[Pasted image 20240924121302.png]]
For example, suppose we need to control the temp of a process to be within a limit, Low and High limits are where the PLC will control the process (e.g. turning on and heater or chiller). However, if we are outside that range, we are in either a Low Low or High High Limit. This is where there may need to be alarms that are triggered in the PLC to alert the operators that there is something wrong. Usually this will interrupt the process depending on the application. 
# Process Logic
## Objectives:
1. Learning how processes work
2. concept of HOA Control
3. Basic level control programming
4. Analog and Digital Control programming and options
5. how to control and protect a pump
6. PID control with a heater program
## Process Programming Overview
Process Logic is the heart and soul of a PLC Program. First we need to stop and visualize the process. Some questions to ask:
1. What does the machine look like?
2. How does it work?
3. How is it laid out?
4. What did the Process Engineer have in mind when developing the P&ID Diagram (Piping and Instrument Diagram) or Controls Schematics

You also will need to think about how things can also go wrong and how we will handle those exceptions. 

This is typically a visual flow diagram or cause and effect diagram to help develop what the program is to look like. It will be a lot easier to write the program if you can visualize as much as possible up front first including how it is to tie into other aspects of the automation pyramid:
![[65f854814fd223fc3678ea64_64ecb5979b278426f219fe7b_What-is-the-Automation-Pyramid.png]]
## Blower HOA (Hand / Off / Auto) Control
HOA stands for Hand, Off, Auto. It is a control scheme for discrete (Digital) as well as analog devices. There are three phases:
- Hand (Manual)
	- This is where we have some flexibility depending on the operator's process
	- Can work like a Jog
- Off 
	- This means that blower will not turn on under ANY circumstances
- Auto (Automatic)
	- Process will turn the blower on and off automatically
	- Can the blower running in Auto be switched to Manual?

We will write the control logic for the three buttons to set the status to the blower: 
- We have a Digital IO and Controls File
- IO Config looks like this: 
	- ![[Pasted image 20240924125301.png]]
- First, make sure that the main branch is running the Digital IO and Controls file via the `JSR` command
- Next, we need to bring Digital IO into the program the correct way by associating the digital IO with bits in the data file
	- Start with the XIC and address it it the first channel of output in module 1. This will be "Hand PB Input" for pushbutton input
	- Then associate that input with an OTE in our data file as "B3:0/0" as "Hand PB" with a symbol as "Hand_PB"
- Then we will setup the OFF and AUTO in the same way
	- ![[Pasted image 20240924125824.png]]
- Next we will setup the Outputs:
	- We will create an XIC that will look at a bit in memory to energize our output
	- ![[Pasted image 20240924130035.png]]

Next we will verify and switch to the controls file to implement the controls logic:
- First we will want to devise a way to setup the state of the blower based on the states defined in our IO section (`MANUAL = B3:0/0, OFF = B3:0/1 and AUTO = B3:0/2`)
- We would like to store the machine state as an INT to be able to refer to it in the future
- We will setup a rung to XIC if the HAND PB is energized and a MOV command to store this as an INT
- Next we will add a one shot to make sure that this is energized once and only once per scan
	- MANUAL ![[Pasted image 20240924130749.png]]
- We will repeat this for the Off and Auto states:
	- OFF State![[Pasted image 20240924130823.png]]
	- AUTO State ![[Pasted image 20240924130913.png]]

Next we will want to energize the blower based on this state. The blower states are (0 = OFF, 1 = HAND, 2 = AUTO). In short, the blower can only be running if it is in either HAND or AUTO. If it is in AUTO it will need some additional controls to make sure it is supposed to be in this condition
- ![[Pasted image 20240924131529.png]]
- The Blower Auto Energize Bit in this example is a fallback

We may also want some logic to make sure that the operator cannot switch from AUTO to MANAUL:
- ![[Pasted image 20240924131849.png]]
Extra Notes:
- Move source is what we want the number to be, the origin
- One shot `ONS` needs a memory location
![[Blower_HOA.pdf]]
## Digital Tank and Pump Controls
Machine concept:
![[Pasted image 20240924150342.png]]

`I:1/0` (Input:Slot 1/Channel 0) ![[Pasted image 20240924151623.png]]

We have 4 inputs and 2 outputs:
![[DTPC_DIGITAL_IO.pdf]]

Smart way would be to define inputs and outputs first in the Digital IO file and then in the main logic/controls file, start with everything we would like to control. 
![[Pasted image 20240924152857.png]]

Our tank will be running two modes, a fill and a drain

When we are setting modes, we can use a bit for two modes or an integer for multiple

In this example, we will use a bit to determine the mode. We also need a delay timer to make sure that waves in the tank do not cause the limit switches to oscillate and cause the pump to turn on and off rapidly

In order to make sure that once something is triggered, we need to hold the state until we reach a higher level
![[Digital_Tank_Pump_Controls.pdf]]
## Analog Tank and Pump Controls
In this example, we are modifying our program from before to remove the digital controls and adding an analog sensor that can be calibrated such that it can detect the level of the tank. 
![[AnalogTankPumpControl.pdf]]
## PID Heater Control
Setup a program to control a heater based on temp.
Digital control -> Analog Control -> Incorporate a PID controller
### DIGITAL
#### IO SETUP:
![[Pasted image 20240925115941.png]]
#### LOGIC: 
![[PID_Heater_DIGITAL.pdf]]
### ANALOG
#### IO SETUP:
- ![[Pasted image 20240925122629.png]]
- One Channel Each for the 1762-IF4 and 1762-OF4 configured for 4-20mA and Scaled for PID
#### LOGIC: 
![[PID_Heater_ANALOG.pdf]]
> [!important] Analog Scaling with `SCP`
> When you are bringing in an Analog input and scaling using the `SCP` command, the Min and Max ranges are below:
> ![[Pasted image 20240923123343.png]]
> 
> Raw/Proportional: 
> `4,000 - 20,000 -> (4-20mA)`
> 
> Scaled for PID:
> `0 - 16,383 -> (14-bit signal)`
> 
> #SCP #ScaledForPID #InputMin #InputMax 
##### PID Setup
- 72 is what we would like to maintain
- 212 is the Max in degF
- 32 is the Min in degF
- Control Output CV is only set if we are in PID Control mode MANUAL
![[Pasted image 20240925130138.png]]
- Tuning of PID involves slowly increasing the Kc value (Controller Gain) until the system starts to oscillate between the set points, and then cut it down to 2/3 of the set point that we had that causes it to oscillate
	- Then do the same with the Ti. Raise the Ti until oscillate and then cut down by 2/3 
	- In most cases, this will give us a sufficiently stable process
- Further development needed in PID tuning
##### Preventing Out of Range Outputs to the PID
![[Pasted image 20240925134515.png]]
# Alarms and Notifications
## Objectives:
1. Alarms and Notification theory
2. Practical considerations for safety 
3. Practical considerations for utility for the operator
4. initiating and clearing event status best practices
5. Use of setpoints
## Alarms Overview
Alarms tell the system when there is something wrong. They will interrupt processes and stop things to protect people and machines.

Notifications are a step below to notify when things are starting to get out of order or can become a problem. These are almost like warnings.
## Considerations 
Best practice is to stop and think about how and when alarms and notifications should be used. The number one consideration is safety.

For anything critical, we would like to notify the operator. However, there are certain instances where an alarm is not needed. For example, you would not put a Low Low alarm on a Freezer as the whole goal is to get as cold as possible (unless the aim is to maintain a certain temp), however it may help to have a notification. 

Another best practice is to make sure that notification and alarms are used when needed. If you over notify an operator, they will begin to tune them out and not pay attention when things become very bad. More is not always better. What is beneficial to keep this running.
## Dual-bit Alarm and Notification Programming
I've developed a tracking sheet for management of data files as I do not have auto complete: https://docs.google.com/spreadsheets/d/1APY6ZYkW5Ri102nrqk3s50NMDwLXUXLQWyU8Em5qayI/edit?usp=sharing

You want to make sure you have a real and steady condition before you sound an alarm.

Best practice to setup alarms in the PLC. It is not best practice in the long run if much of the alarm functionality is handled in the HMI as there is always a possibility that the HMI will go down.
### Adding Alarms on the Tank Pump Program
![DualBitAlarmsNotifications.pdf](./attachments/DualBitAlarmsNotifications.pdf)
### Adding Alarms on the Analog Heater Program
- ONS makes the remainder of the rung true on a pulse while OSR sets a pulse to a memory location (a bit)that can be used to do other things. They're very similar.

![Heater_Alarms.pdf](./attachments/Heater_Alarms.pdf)
## Setpoints
Setpoints are tools used to allow the operator to set limits and parameters for the system to function. 

We previously set a LL limit of 15%. However, there may come a day when we want the operator to set this to 5% instead. In order to do this change, we would need to revisit site and modify the program. However, we can also use SetPoints to allow for this.

This is as simple as adding an integer for our LL level setpoint that we assume can be set from the HMI:

Keep in mind that this mainly depends on the application. There are always instances where you do not want certain setpoints to be set by the operator. You may want certain setpoints to be set from another person with more knowledge (i.e. Maintenance vs the operator). 
![[Pasted image 20240926104820.png]]
# HMI (Human Machine Interface)
## Objectives
1. Concept and Importance of HMI
2. Exposure to a variety of HMI methods
3. Exposure to how an HMI program works
4. Understanding tags and addresses
5. basic process of creating a screen with controls
6. learn how HMI manages alarms and notification events
7. Get an overview of permission management on an HMI
## HMI Overview
HMI stands for Human Machine Interface to allow for the visualization and control of a Machine / Process / System.

The HMI allows us to read and write to the PLC. The PLC Programmer needs to be careful to display the right information and appropriate level of control on the HMI for the operator based on considerations such as safety and security. 
![[CHA-102WR_1__54360.jpg]]
## HMI Alternatives
The front panel of the PLC display can be programmed as an HMI
![[Pasted image 20240926111646.png]]

HMI's can be as expensive or cheap as needed.
## Basic Flow of an HMI Program
Typically with Rockwell we would use Factory Talk to build an HMI

One example:
![[Pasted image 20240926111906.png]]
- This screen also have an invisible button on the bottom right that is held for 20s to enable a diagnostic and service data mode

![[Pasted image 20240926112003.png]]

![[Pasted image 20240926112134.png]]

![[Pasted image 20240926112155.png]]
- S box is who is logged in

![[Pasted image 20240926112229.png]]

![[Pasted image 20240926112248.png]]

For this demo, we are using FactoryTalk View Studio. 

In almost all HMI dev environment is the use of tags. This refers to every piece of information being written from or to a PLC. 

For example: 
![[Pasted image 20240926112537.png]]
![[Pasted image 20240926112709.png]]
- Device is reading from an external device
- Memory is reading from the HMI's computer

Best to think of the HMI as a layer above the PLC that talks with the PLC via tags and tag addresses. Tags are defined to memory locations in the PLC. 

A common component with HMI development is the tag browser 
## Setting up a Screen
Setup an OPC server connection to our emulator "RSLogix Emulate 500". Factory talk cannot talk directly with the emulator driver, therefore we need to use OPCUA (Built into RSLinx).

Project>Add new server>RSLinx OPC Server
![[Pasted image 20240926115834.png]]
## Alarms/Events/Notifications
Alarms are configured using the alarm configuration window
![[Pasted image 20240926121556.png]]
![[Pasted image 20240926121618.png]]
![[Pasted image 20240926121627.png]]
![[Pasted image 20240926121651.png]]
- This is where you would setup the tags assigned to alarm silence and alarm reset
## Permissions
There are times where you want only certain people to be able to make modifications and see certain screens.

This takes place under user management. First create a new user and password and add this user to a user group (show users only).

Next we will click on Runtime security to select the permissions for certain users: 
![[Pasted image 20240926122140.png]]
![[Pasted image 20240926122233.png]]

Pages and items can be assigned security codes to determine if they can be accessed by the correct user: 
![[Pasted image 20240926122750.png]]
## Designing around UX
Some considerations:
1. The Usability of the HMI for operators
2. If there is a setup procedure for this machine to be able to have it run at first. Shutdown team.
3. How intuitive does the HMI need to be? Revolving door of users or an expert with the system?
4. Security -> what damage can be done and how far can certain user accounts get?
5. Time for development

Ultimate goal is for someone with no prior experience with this system to be able to look at the HMI to be able to tell how the system works and what to do. 
# Communications
## Objectives
1. Intro to general idea of comms
2. serial connections
3. ethernet connections
## Communication Overview
Many different options for Communications: 
![[Pasted image 20240926125024.png]]

In RSLogix, we typically will be using is RSLinx. RSWho will show us the devices on the network
![[Pasted image 20240926130226.png]]

This also gives us the option of taking AB PLC and publishing their tags on DDE and OPC protocols as an interface for other devices to read and write tags to the PLC. 

# Program Walkthrough `*skip*`
## Objectives
1. Dissect an entire PLC program
2. Reinforce Program Instructions
3. Reinforce Program Flow
4. Reinforce HMI/PLC Interaction
5. Reinforce Process Logic
6. Reinforce Alarm Functionality
## Main
- The `S:1/15` labeled as "First Pass" #initialrun #firstpass
	- ![[Pasted image 20240926161424.png]]
	- This bit energized only during the first scan when the PLC is turned on and then turned FALSE afterwards
	- Typically used as an init bit to make sure that when the PLC is turned on to make sure that certain actions happen
	- In this example, we are moving a 0 int to the blower mode so that when the PLC first turns on, the blower turns off

- Project Structure
	- This project is structured as:
		- DIGITAL INPUTS
		- DIGITAL OUTPUTS
		- ANALOG INPUTS
		- ANALOG OUTPUTS
		- CONTROLS
		- ALARMS
		- DISPLAY

- Unlatch of the `S:5/0` bit "Overflow Trap"
	- ![[Pasted image 20240926162004.png]]
	- Prevents math errors from killing the program during execution (e.g. diving by 0)
	- This is a MATH Error bit
	- **This needs to appear on the last rung on Ladder #2 MAIN to prevent the PLC from faulting (and stopping) out completely**
## Digital Input
- Symbols
	- These allow us to find these easily when we use an OPC server to visualize this
## Digital Output
- We want to make sure that we are energizing outputs in one spot and one spot only
## Analog Input
- Three Branches for Analog Input Possibilities
	- LIM Branch
		- The Limit Block tests that the Input falls between 0-16383 (Scaled for PID), if True, then scale it to 0-100 and store as a float (F8:0)
	- LES Branch
		- Check the value and see if it is less than 0
		- That means that it is out of range and will force in a 0% to prevent bad math to the output (F8:0)
	- GRT
		- Checks the value to see if it is greater than 16383
		- That means that it is out of bounds in the high end and uses the MOV command to move 100% as the output (F8:0)
## Analog Output
![[Pasted image 20240926165557.png]]
## Controls
![[Pasted image 20240927113801.png]]

Next rung is the logic for turning the system off. Either from the physical push button or HMI:
![[Pasted image 20240927113902.png]]

Next rung allows the system to transition to a faulted state if the general alarm notification bit is on:
![[Pasted image 20240927114052.png]]

HOA control for setting blower mode:
![[Pasted image 20240927114151.png]]

Energizing of the blower based on:
- System Mode =! 0 (either On or FAULTED, but not OFF) AND Blower Mode must be in AUTO
- IF system is in HAND (Manual) mode, then blower will run
![[Pasted image 20240927114526.png]]

This is Filling of a tank:
```python
if system_mode =! "OFF":#Either ON or FAULTED
	if Tank_level < 20: #tank Scaled for PID less than 20
		if alarm_reset =! True: #if alarm_reset is False
			Tank_Fill_Start_Delay_Timer
			...
#Wondering if there is a way to translate lader logic to python style
```
![[Pasted image 20240927114640.png]]

Instead of tank fill logic, this set of ladder logic will drain tank
![[Pasted image 20240927115451.png]]

These two rungs will handle filling and draining of the tank
![[Pasted image 20240927115554.png]]

Heater control, when the system mode =! 0, and no high temp alarm, then activate heater. This heater uses two signals, a digital and an analog: On/Off and analog is how much to heat.
![[Pasted image 20240927121643.png]]

This next rung will send a signal to tell it how much to heat. 
![[Pasted image 20240927121823.png]]
This rung takes in an input from the HMI (Set by the operator) and inputs it to the integer: `N7:6`

`PD9:0.SPS`: 
- PD9 is the PID Data File
- Stored as Set Point for PID Control Loop

PID:
- Process Variable (`N7:8`): This is the integer that the PID will look at. This is the Scaled temp coming in from an analog input
- Control Variable (`N7:9`): We will be controlling this number, which is being written out to an analog heater. 
- The PID is controlling this Control Variable `N7:9` (Analog output to the heater) to make the Process Variable `N7:8` (The Analog Input from the temperature sensor) attain and maintain our setpoint from `N7:6` which is currently set to 90
![[Pasted image 20240927124011.png]]

Any time our system is ON or FAULTED and the General Alarm Bit is energized (silences or Clears Notifications)
![[Pasted image 20240927124027.png]]
## Alarms
Compare tank level with tank set point LL, if below the LL setpoint and the alarm reset button has not been pushed, timer for 5 seconds and then ONS the alarm trigger bit.
![[Pasted image 20240927124148.png]]

![[Pasted image 20240927124446.png]]
Hold in circuit from previous ONS that will hold in this alarm until it is either reset of no longer active

*May need confirmation below:*
As a reminder, a one shot is used to trigger an instruction Once per scan instead of many times. For example, A PLC may have a scan time of 8ms. If you try to be as quick as possible with pressing a push button, this would register 4486 counts.  For notifications, this is important to prevent the user from registering 4000 alarms all at once instead of refreshing as long as the instance is true. 
![[Pasted image 20240927125957.png]]
 
![[Pasted image 20240927130419.png]]
![[Pasted image 20240927130530.png]]
![[Pasted image 20240927130706.png]]
![[Pasted image 20240927131249.png]]

Temperature Alarms
![[Pasted image 20240927131302.png]]
![[Pasted image 20240927131416.png]]
![[Pasted image 20240927131431.png]]
![[Pasted image 20240927131511.png]]

Categorical Alarms:
- These are alarms that can be read on another system, does not say what the issue is, just that there is something wrong with the tank or temp or blower etc...
![[Pasted image 20240927131538.png]]
![[Pasted image 20240927131658.png]]

This is a General Alarm Notification Bit, something is wrong with the system
![[Pasted image 20240927131723.png]]
## Display
Ladder 9 Display
- Heater CV, this is the output of the PID sent to the heater to determine how much to heat up, but we want to show the operator what % of the heat the heater is generating. Is the Heater at 50% of its output or 100% of its output?
![[Pasted image 20240927131921.png]]
## DemoTest File
![[DEMOTEST-200809-142534.pdf]]
# Shakedown and Debug
## Objectives
1. How to prepare a program for use
2. concept of emulation
3. how to conduct a dry run
4. how to force IO
5. potential consequences of mistakes
## Emulation
![[Pasted image 20240927140643.png]]
RSLogix Emulate 500 is a program that allows us to interact with a PLC program while it is running and to assist with the debug of that program
### Step 1: 
- File>Open>DemoTest Program in RSLogix Emulate 500
- Set the station # to 1
- Now PLC program is loaded
- When you open the same program and same version in RSLogix 500, you can connect
### Step 2: 
- Open the same program in RSLogix 500
- Click Comms>Who Active Go Online
- ![[Pasted image 20240927140855.png]]
- ![[Pasted image 20240927141003.png]]
- Open the EMU500, this is the program running 
- Click OK
### Step 3:
- You will notice that now the REMOTE PROGRESS Status have changed and the ladder is spinning
- Click Run:
- ![[Pasted image 20240927141210.png]]
### Step 4: 
- Program is now live and running in the emulator and we are connected to it via RSLogix 500 so we can see what it happening in the PLC program while it is running 

> [!Info] PLC Modes
> There are Three Modes a PLC typically will have:
> 1. `RUN`: Program is running
> 2. `PROGRAM`: Program is not running and allows us to transfer between us and the PLC. PLC is idle
> 3. `REMOTE`: Allows a computer to change the mode between Run and Program. Instead of us physically changing the mode from the PLC or touch screen, we can do if from here
>
> PLC Emulator will not be able to execute a PID Control Loop, this needs to be tested on an actual PLC

## Dry Run
If a bit is energized, it is highlighted green

We need to test every single input, output, element, function on the HMI and PLC to make sure that it is good to go. 
## Forcing IO
Suppose we wanted to force certain inputs and output bits to energize to simulate and IO firing in real life. This is achieved using the forces:
![[Pasted image 20240927144114.png]]

This is done by right clicking a bit and selecting Force On or Force Off. If something is being forced on or off, we can also remove the force as well. 
![[Pasted image 20240927144214.png]]

Looks like sometimes you may need to force something online and then click the "toggle bit" option to make sure that it sticks

There is no way to simulate a PID using an emulator, must be controlled using a live system

Remember to remove forces after you have completed your testing
## Electromechanical Checks
This is testing of the program on a live system. We need to make sure that the machine is wired correctly and that all components in the PLC panel work. This is also know as IO Checks.

This involves testing the electrical connection between the PLC modules and devices. 

We need to check:
- No cable cross talk from high voltages causing bad readings
- All valves can fire
- Motors are running in the correct direction
- Nothing is stuck
- Everything is wired correctly
## Full Function Test
This is when all utilities and hardware is installed, this is a final test. At this point, we should have full confidence there will be no surprises once we power the system on and run a test. There should be no major changes at this point.

You never want a major bug to show up at this stage. Be thorough and don't let people rush this process.
## Troubleshooting Methodology
This is when something goes wrong and you need to find out what the problem is and how to fix it. 

The Acronym "RV AIR" Will help with this process:
- `R` - Recognize
	- Recognize that there is a problem
	- This is a report or suspicion that there is a problem
- `V` - Verify
	- Verify that the problem is actually a problem
	- This could be user error
	- Ask them to pull out their phone and take a picture of the problem
- `A` - Analyze
	- Now we take a look at the problem and check to see where things are wrong
- `I` - Isolate
	- Isolate the problem to determine where the root cause is.
	- Get the problem down to a single thing that can be fixed
- `R` - Repair (and **Test**)
	- Fix the problem and Test that this fix is valid
## Consequences
Most machines have a way of killing people one way or another if they are not handled correctly. 
## Proper Programming makes Debugging and Troubleshooting Easy
Writing good logic and diligent notes is the key to making debug easier and quicker. Consistent code will make it easier to find when things go wrong. 
# Getting the Job Done Fast!
## Overview
This section overviews how to best scope out a project from scratch and getting started with how to plan and set things up
## Creating your Architecture
Step one when starting a project is creating the program's architecture.

You want to break things down based on the size of the project you are creating. For example, if you have 1000 I/O's, then it might make sense to divide based on the type of device for each file. Otherwise, smaller PLC projects might only have one file with all of the IO.

For example, in a medium sized project, you may want program files such as:
- `MAIN`
	- This is where the JSR's live that jump to all of the other rungs
- `Digital Inputs`
- `Digital Outputs`
- `Analog Inputs`
- `Analog Outputs`
- `HOA`
	- Hand, Off, Auto Logic
- `Control Logic`
- `Alarms`
- `System Modes`
- `Process Logic`
Don't forget to add `JSR`(Jump to Solutions) on the Main file to jump to these rungs for every scan. 

Remember that the logic used on a PLC is circular, there is no start or end point for your program. Meaning that it does not usually matter the order that items get called before others (Typically the Order of Operations does not matter). 

We also need to keep in mind that PLC logic is always in development, make sure to plan for when we need to make a change. 
## Approaching the logic
After setting up the architecture, next logical step is to work on setting up the IO.

You will need to create or ask for an IO list. This is a list of all of the devices coming into the system and everything that the system needs to output to. What devices do we need input from and what do we need to be controlling.

### Digital Inputs 
![[Pasted image 20240930122933.png]]
Usually: XIC -> OTE. This allows us to call a bit to find the status of an input instead of calling the actual input device. 

### Analog Inputs
![[Pasted image 20240930123003.png]]
Usually: Scaled with Parameters with logic to check if the input is between 0 and 16,383. 

### Digital Output
![[Pasted image 20240930123420.png]]

### Analog Output

### HOA
![[Pasted image 20240930123611.png]]

### Alarms (Logic for 1 alarm)
![[Pasted image 20240930123909.png]]

A lot of work can be done before we even know what the machine control logic is. This is mainly IO setups, Alarms, Modes...
## Start from a Template
A lot of work can be done by taking logic from a PLC template and adapting it to your particular situation. 

Example Template:
![[Pasted image 20240930125524.png]]

Digital Inputs:
![[Pasted image 20240930124645.png]]

Digital Outputs:
![[Pasted image 20240930124705.png]]

Analog Inputs:
![[Pasted image 20240930124728.png]]

Analog Outputs:
![[Pasted image 20240930124746.png]]

Analog Alarms:
HH, LL
![[Pasted image 20240930124828.png]]
Logic for if there are any alarms or status of alarm groups:
![[Pasted image 20240930125018.png]]

Digital Alarms:
![[Pasted image 20240930125103.png]]

HOA Mode:
![[Pasted image 20240930125137.png]]

Controls:
![[Pasted image 20240930125202.png]]

System Modes:
![[Pasted image 20240930125237.png]]

Hour Meter:
![[Pasted image 20240930125309.png]]
![[Pasted image 20240930125330.png]]
![[Pasted image 20240930125348.png]]
![[Pasted image 20240930125401.png]]
## Primer for Level 2
Refresher on Digital Control Logic:
![[Pasted image 20240930130744.png]]

![[DEMOTEST-200809-142534 1.pdf]]











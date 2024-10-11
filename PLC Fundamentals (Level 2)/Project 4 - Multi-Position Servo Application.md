# Project 4 - Multi-Position Servo Application
## Checklist:
- Setup File Folder using Template
- Setup Data Type Planning File: https://docs.google.com/spreadsheets/d/1ysUcPVzLaOEI_JjlReSGKend5t8eQ3-afiUJFhji0tc/edit?usp=drive_link
- Read Specs
- Define System Inputs and Outputs
- Define Customer Cause/Effect Test cases
- Develop Process Diagram
## Customer Specifications
### PDF![[Project4-200808-214834.pdf]]
### Input/Outputs and Reserved Data Locations
| INPUT/OUTPUT | MEM    | NAME       | SYM   | DESC.                                 |
| ------------ | ------ | ---------- | ----- | ------------------------------------- |
| INPUT        | I:0/0  | HOME SW    | HSW   | Home Limit Switch                     |
| INPUT        | I:0/1  | FILL SW    | FSW   | Fill Limit Switch                     |
| INPUT        | I:0/2  | DRAIN SW   | DSW   | Drain Limit Switch                    |
| INPUT        | I:0/3  | FLUSH SW   | FLSW  | Flush Limit Switch                    |
| INPUT        | I:0/4  | CYCLE CALL | CC    | Cycle Call input btn                  |
| OUTPUT       | O:0/0  | SERVO      | SER   | Servo Moto (Moves Cam)                |
| OUTPUT       | N7:0   | VALVE POS  | POS   | Valve Position                        |
| INPUT        | B3:0/0 | RESET      | RESET | Reset Button Input                    |
| INPUT        | B3:0/1 | E-STOP     | ESTOP | Emergency Stop (Return to safe state) |
### Test Criteria (Cause/Effect)
### Process Diagram
FIRST PASS: https://excalidraw.com/#json=GHMwMDGeq0Lt3RTRmD-xW,8nPOA7vrFP8iVLsAtOaKGg
![[diagram1_large.png]]
### Notes
- Modular water treatment system is being integrated into an existing system. When the host system calls for a cycle, servo will cycle a control valve from the home position to fill for 10 seconds, then drain for 20 seconds, then flush for 10 seconds, then back to home until the next call, cam-operated microswitches tell us what position the value is in
- Purpose of the Cam is to activate another machine's ability to perform a Fill, Drain and Flush sequence.

Summary
- This machine is the control PLC for a system of three other machines in a water treatment facility:
	- A machine that will Fill a tank
	- A machine that will Drain a tank
	- A machine that will Flush a tank
- Our PLC controls a servo to move the control cam to 4 positions:
	- Home
	- Fill
	- Drain
	- Flush
- A typical control sequence is:
	- Input to PLC for a call cycle
	- Servo will cycle our control valve Cam from the Home position to fill for 10s
	- Servo will cycle our control valve Cam from the Fill position to Drain for 20s
	- Servo will cycle our control valve Cam from the Drain position to Flush for 10s
	- Servo will cycle our control valve Cam from the Flush position to Home
		- System can only receive a cycle call when it is in the home position
- Control Sequence is:
	1. Home -> Fill
	2. Fill for 10s
	3. Fill -> Drain
	4. Drain for 20s
	5. Drain -> Flush
	6. Flush for 10s
	7. Flush -> Home
- Ignore if not bypass bit (Use digital control logic to keep bypass on)

- Additional Constraints:
	- Int N7:0 to report the current position of the valve
		- 0=HOME
		- 1=Fill
		- 2=Drain
		- 3=Flush
		- 4=Traveling
		- -1 = Error State
	- Binary B3:0/0 as a reset to abort and return the valve to HOME
	- Binary B3:0/1 as a E-Stop
		- Home = Do Nothing
		- Fill = Move to DRAIN immediately and stay at DRAIN for 20s, skip FLUSH and return HOME (activate bypass bit)
		- Drain = Stop immediately and move to an error state
		- Flush = Stop immediately and move to an error state 

- May also be a good idea to setup a breakpoint system that will halt the scans?

- Finite State machine States will be different that Machine Position
- String file should report out machine current State and Position
	
- What if the system looses power in the middle of a cycle, should have a homing sequence
- Ability to manually send the valve to a particular position
	- FORCE HOME (Use bypass bit to disable input in the way)
	- FORCE FILL (Use bypass bit to disable input in the way)
	- FORCE DRAIN (Use bypass bit to disable input in the way)
	- FORCE FLUSH (Use bypass bit to disable input in the way)

- Need to make sure that limit switches only fire once and can be ignored
- E-stop and Reset
	- Reset will abort the process and return to Home. I think Reset should drain (ignore Flush) the system first then return home
	- E-Stop
		- Assuming worst case is if someone is in the tank when it is filling, returning to a safe state would be to Drain and return Cam to home position

![[Pasted image 20241010144837.png]]

---
# Project Brief: Project Name
**Project Name**: 
**Date**: 
**Project Client**: 
## Project Overview:
- [Summary of the Project]
## Goals and Objectives:
- [Outcomes that the project needs to meet]
## Constraints and Assumptions:
- [Limitations and current working assumptions]
## Project Scope:
- [Outline of Deliverables]
## Target Audience:
- [Who is this Product or Service For]
## Success Criteria:
- [Who determines the success Criteria? What is the Success Criteria?]
## Budget:
- [Time and Resource Budget]
---
## Project Development Notes
## Project Final Delivery
### Ladder Logic
### Data File Planning
### Final Report
## Lessons Learned

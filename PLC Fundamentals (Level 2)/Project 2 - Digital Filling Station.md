# Project 2 - Digital Filling Station
## Checklist:
- Setup File Folder using Template
- [Setup Data Type Planning File](https://docs.google.com/spreadsheets/d/1E7w4lTBc3KbPq-TJDU3Yv7Jzpnvk4Pch6cOKYsRYWHQ/edit?usp=sharing)
- [Read Specs](obsidian://open?vault=AdamsObsidianNotes&file=Classes%2FPLC-Notes%2FPLC%20Fundamentals%20(Level%202)%2Fattachments%2FProject2-200817-091946.pdf)
- Define System Inputs and Outputs
- Define Customer Cause/Effect Test cases
- Develop Process Diagram
## Customer Specifications
### PDF: 
![[Project2-200817-091946.pdf]]
### Input/Outputs
- Inputs:
	- I:0/0 - Proximity Switch (PS)
		- 0 = No Box Present 
		- 1 = Box is Detected
	- I:0/1 - Level Switch (LS)
		- 0 = Box is Empty OR Box Not Present
		- 1 = Box is Full
	- I:0/2 - Red Photo Eye (RP)
		- 0 = Box Not Present OR Blue Label
		- 1 = Red Label is Detected (Pecans)
	- I:0/3 - Blue Photo Eye (BP)
		- 0 = Box Not Present or Red Label 
		- 1 = Blue Label is Detected (Walnuts)
- Outputs:
	- O:0/0 - Conveyor Motor (CM)
		- 0 = Motor is OFF, Conveyor does not move
		- 1 = Motor is ON, Conveyor moves box down line
	- O:0/1 - Walnut Hopper (WH)
		- 0 = Hopper is Closed
		- 1 = Solenoid opens to allow walnuts to Fall from Hopper
	- O:0/2 - Pecan Hopper (PH)
		- 0 = Hopper is Closed
		- 1 = Solenoid opens to allow Pecans to Fall from Hopper
### Test Criteria (Cause/Effect):
0. INIT -> CM ON, WH OFF, PH OFF
	1. S1 -> 1-0-0
1. PS ON -> CM OFF, WH OFF, PH OFF
	1. S2 -> 0-0-0
2. PS ON, RP ON -> CM OFF, WH OFF, PH ON
	1. S3 -> 0-0-1
3. PS ON, RP OFF, BP ON -> CM OFF, WH ON, PH OFF
	1. S4 -> 0-1-0
4. PS ON, RP OFF, BP ON, LS ON -> CM ON, WH OFF, PH OFF
	1. S5 -> 1-0-0
5. PS OFF, LS OFF, RP OFF, BP OFF -> CM ON, WH OFF, PH OFF
	1. S1 -> 1-0-0
6. PS OFF, LS OFF, RP ON, BP ON -> CM OFF, WH OFF, PH OFF
### Process Diagram
First Pass:
![[Pasted image 20241004130218.png]]
Second Pass:
![[Pasted image 20241004133604.png]]
Third Pass:
![[Pasted image 20241004134237.png]]
### Notes:
- This is a machine that will fill boxes with Nuts. The type of nut is determined by the color read using a photo eye and a level switch to determine when the box is full and ready to send down the line
- Process:
	1. Box arrives on conveyor
	2. Prox Switch closes when box arrives, conveyor stops
	3. Label is read by the Photo eyes (Red or Blue)
		1. Red -> Pecans
		2. Blue -> Walnuts
	4. Level sensor energizes when the box is full
	5. Motor runs to operate conveyor down the line
- Risks:
	- We do not want the Box to run away and pour nuts on the floor
	- We do not want to overfill the box
	- We do not want the box to sit stationary for too long
- When you fill the box, how do you start the conveyor again?
- A Finite State Machine diagram may be a better way to go of visualizing and documenting the system function based on Inputs and Outputs:
	- Inputs/Outputs: `label - symbol (0 = ???, 1 = ???)`
	- Example: [Finite-State Machines: Explanation & Example](https://youtu.be/6oe1Tmg9rjM?si=wfS5LldhERMhUcL2)![[Pasted image 20241004121011.png]]
	- [CSE370: Introduction to Digital Design](https://courses.cs.washington.edu/courses/cse370/97au/admin/Slides/Week10Lecture1/sld002.htm) ![[Pasted image 20241004121101.png]]
	- [How To Design A Finite State Machine](https://www.cs.princeton.edu/courses/archive/spr06/cos116/FSM_Tutorial.pdf)
	- Don't forget to include a Reset Input (Return Machine to INIT State) and and E-Stop state
- Progress:
	- Completed up to DCL for motor, will need to add in DCL for the hoppers based on system mode
## Project Development Notes
## Project Final Delivery
### Ladder Logic
### Data File Planning
### Final Report
## Lessons Learned
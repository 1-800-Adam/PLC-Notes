# Project 1 - Digital Control Logic
## Misc. Notes:
- Digital Control Logic needs a bit for the trigger, bit for the interrupt and those two to control the hold in, cannot perform using the output bit:
- Step 1 is to try to develop a process diagram for this system using the inputs and outputs that passes the test cases on paper
- Order of Instructions matters when using One-shot
- **I may need to check original template to see if I had a JSR to the main causing the program to not progress and act wonky**
## Specifications
![[Project1-200808-214416.pdf]]
## Inputs:
Input Slot 0
- I:0/0 - Low Pressure Switch (Closes at 90PSI and above) (below?)
	- If pressure is below 90 psi, we want the pump to energize
- I:0/1 - High Pressure Switch (Closes at 110PSI and above)
	- If pressure is at 110 psi, we want the pump to de-energize
## Outputs:
Output Slot 0
- O:0/0 - Pump
- O:0/1 - Running Indicator
	- Lights up whenever we have more than 90 PSI in our receiver
	- Indicates that anything running off of the receiver tank is ready. if the indicator is off, the pressure is too low
## Summary:
- We will be maintaining the pressure in an air tank using a compressor.
- There are two pressure switches (90 and 110 PSI) for Low and High
- We want to illuminate the indicator light when pressure is above 90PSI
## Test Criteria:
- Program startup should have the pump run immediately but light should be off
- Force the Low Pressure Switch Closed, pump should remain energized and light should also energize
- Leave the low pressure switch closed and force the high pressure switch on. Pump should de-energize and the light should remain energized
- Leave the low pressure switch closed and force the high pressure switch off, pump should de-energize and light remains on
- force both pressure switches off and verify the pump starts back up and light goes off (off or deenergized?)
## Cause/Effect Test Cases:
1. Program INIT-> Pump ON, Lamp OFF
2. LPS ON -> Pump ON, Lamp ON
3. LPS ON, HPS ON -> Pump OFF, Lamp ON
4. LPS ON, HPS OFF -> Pump OFF, Lamp ON
5. LPS OFF, HPS OFF -> Pump ON, Lamp OFF
## Process Diagram:
![[Project1Process.png]]
## Program First Pass
![[Pasted image 20241002172052.png]]

## Ladder Logic:
![[Project 1-Digital Control Logic PROGRAM FILES.pdf]]
## Data File Planning:
![[Project 1-Digital Control Logic DATA FILES PLANNING.pdf]]
## Project Final Report:
![[Project 1-Digital Control Logic.pdf]]

## Lessons Learned:
- IO Runs first
- Instructor starts on the controls file by adding two OTE's for the two digital outputs
- Instructor implements a timer for fluids such that if the switch is left open for >5s, then activate the pump
	- We want the pump to turn on when P<90PSI and after 5s.
- Pump start trigger is placed on a separate line such that when timer completes, then activates a ONS to the trigger
- Timer for 110 PSI switch closed for 5s to activate a one shot to the Digital Control Logic Hold In to provide the Interrupt 
- Instructor includes same logic for the light bulb based on timers to prevent light bulb from turning on and off rapidly
- Might be good to start with all digital outputs using digital control logic 
## Instructor PLC Code:
![[InstructorCode.pdf]]
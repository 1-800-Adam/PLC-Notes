# Project 3 - Inventory Management
## Checklist:
- Setup File Folder using Template
- Setup Data Type Planning File: https://docs.google.com/spreadsheets/d/1fLwWuTZw5sCbBgob_4DRfogfaNnqx7fowCvkUAa6044/edit?usp=drive_link
- Read Specs
- Define System Inputs and Outputs
- Define Customer Cause/Effect Test cases
- Develop Process Diagram
## Customer Specifications
### PDF
![[Project3-200808-214725.pdf]]
### Input/Outputs

| INPUT/OUTPUT | MEM   | NAME    | SYM  | DESC.                                               |
| ------------ | ----- | ------- | ---- | --------------------------------------------------- |
| INPUT        | ST9:0 | BARCODE | BC   | Barcode scan into PLC from barcode scanner (STRING) |
| OUTPUT       | N7:0  | P#: 123 | p123 | Part #123 quantity on‐hand                          |
| OUTPUT       | N7:1  | P#: 456 | p456 | Part #456 quantity on‐hand                          |
| OUTPUT       | N7:2  | P#: 789 | p789 | Part #789 quantity on‐hand                          |
### Test Criteria (Cause/Effect)

| Seq. | Input                                    | Output                                                   |
| ---- | ---------------------------------------- | -------------------------------------------------------- |
| 1    | `INIT`                                   | `[N7:0 = 0] [N7:1 = 0] [N7:2 = 0]`                       |
| 2    | `123-10:1`                               | `[N7:0 = 10] [N7:1 = 0] [N7:2 = 0]`                      |
| 3    | `123-10:1`                               | `[N7:0 = 20] [N7:1 = 0] [N7:2 = 0]`                      |
| 4    | `456‐15:1`                               | `[N7:0 = 20] [N7:1 = 15] [N7:2 = 0]`                     |
| 5    | `789‐30:1`                               | `[N7:0 = 20] [N7:1 = 15] [N7:2 = 30]`                    |
| 6    | `123‐19:2` AND `456‐14:2` AND `789‐29:2` | `[N7:0 = 1] [N7:1 = 1] [N7:2 = 1]`                       |
| 7    | `149‐1:1`                                | `[N7:0 = 1] [N7:1 = 1] [N7:2 = 1]` AND `ERROR ALARM BIT` |
### Process Diagram
- Start by reading in the active part numbers, setting as 0's for current qty
- When input string is updated
	- Parse the input: input Part Number, input Qty, input Direction
- Check if this is a valid part number
- Evaluate current input if input direction is 2:
	- Error Check: req_qty = input P# current quantity
	- Error Check qty: if (req_qty - input qty) is LT 0, then error bit (invalid qty) activate and clear input 
	- else: all clear bit 
- If all clear bit, then perform calculation
### Notes
- Project Summary
- ![[Pasted image 20241008173809.png]]
- https://literature.rockwellautomation.com/idc/groups/literature/documents/rm/1747-rm001_-en-p.pdf
- ![[Process.png]]
---
# Project Brief: Project Name
**Project Name:** PLC Inventory Management
**Date:** 10/8/2024
**Project Class:** Paul Lynn's PLC Dojo > Applied Logic > Project 3
## Project Overview:
- Project will provide a simple inventory management system for tracking part numbers coming into and out of the system. PLC will take in a string that will have part number, quantity, and if it is incoming or outgoing inventory. 
- There are three current part number: P#123, P#456, P#789
- The Incoming string format is: `[part_number]-[qty]:[In/Out]`
	- In is marked as a 1
	- Out is marked as 2
## Goals and Objectives:
- [Outcomes that the project needs to meet]
## Constraints and Assumptions:
- Should the project include error checking:
	- If you attempt to take out more parts than there is inventory of (10 - 50 = -40, impossible to have negative inventory)
	- May be worth while to include a middle man between writing to test if the request is allowed
	- Only applicable for inventory removal 
- Should the system include a method for additional part numbers?
	- May need to include a method for saving a list of active part numbers
- I would like to look into exporting/saving results of current inventory in case our system looses power 
- There is no use cases for a E-Stop
- I would like to look into including an undo command
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
![[Project 3 - Inventory Management.pdf]]
### Data File Planning
### Final Report
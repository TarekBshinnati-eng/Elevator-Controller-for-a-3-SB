# Elevator Controller for a 4-Floor Building

**Course:** EECE 320 - Digital Systems Design
**Institution:** American University of Beirut (AUB)
**Project Type:** Verilog HDL Implementation

## Project Overview

This project implements an elevator controller for a four-floor building (Ground Floor + 3 floors) using Verilog Hardware Description Language (HDL). The controller is designed as a Moore Finite State Machine (FSM) that manages elevator movement, direction, and door operations based on floor and cabin requests.

## Features

- **4-Floor Support:** Ground Floor (GF), Floor 1, Floor 2, and Floor 3
- **Dual Request System:** Handles both floor requests (external) and cabin requests (internal)
- **Intelligent Direction Management:** Prioritizes requests in the current direction of movement
- **Door Control:** Automated door opening/closing at requested floors
- **Reset Functionality:** Asynchronous active-high reset to initialize the system

## Project Structure

```
.
├── README.md                          # This file
├── src/
│   └── elevator_controller.v          # Main Verilog source code
├── docs/
│   ├── project_requirements.pdf       # Original project specifications (EECE 320)
│   ├── project_report.pdf             # Complete project report
│   ├── project_report.docx            # Editable project report
│   ├── design_code.pdf                # Design code documentation
│   ├── design_code_merged.pdf         # Merged design documentation
│   ├── testbench_code.pdf             # Testbench documentation
│   └── page2.pdf                      # Additional documentation
└── images/
    ├── simulation/                    # Simulation waveforms and results
    │   ├── waveform_test1.png
    │   ├── waveform_test2.png
    │   ├── eda_playground.png
    │   └── simulation_overview.png
    └── development/                   # Development process screenshots
        └── [ChatGPT and development screenshots]
```

## Module Interface

### Inputs
- `clk` - Clock signal for synchronization
- `rst` - Asynchronous active-high reset signal
- `f_req[3:0]` - Floor requests (external):
  - `f_req[0]` - Ground Floor request
  - `f_req[1]` - Floor 1 request
  - `f_req[2]` - Floor 2 request
  - `f_req[3]` - Floor 3 request
- `c_req[3:0]` - Cabin requests (internal):
  - `c_req[0]` - Ground Floor request
  - `c_req[1]` - Floor 1 request
  - `c_req[2]` - Floor 2 request
  - `c_req[3]` - Floor 3 request

### Outputs
- `request[3:0]` - Pending floor requests (latched)
- `current_floor[1:0]` - Current floor position:
  - `2'b00` - Ground Floor
  - `2'b01` - Floor 1
  - `2'b10` - Floor 2
  - `2'b11` - Floor 3
- `direction` - Movement direction:
  - `1'b0` - Moving down
  - `1'b1` - Moving up
- `door_open` - Door state:
  - `1'b0` - Closed
  - `1'b1` - Open

## System Behavior

1. **Reset State:** On reset, the elevator initializes at Ground Floor with doors closed and direction set to up
2. **Request Handling:** Floor and cabin requests are combined and latched until fulfilled
3. **Movement:** Elevator moves one floor per clock cycle
4. **Door Operation:** Doors open for one clock cycle when reaching a requested floor, then close
5. **Priority Logic:**
   - Requests in the current direction are serviced first
   - Requests in the opposite direction are serviced after current direction requests are completed
6. **Idle State:** When no requests are pending, the elevator remains stationary with doors closed

## FSM States

The controller implements the following states:

- **IDLE States:** `IDLE_0`, `IDLE_1`, `IDLE_2`, `IDLE_3` (one for each floor)
- **Movement States:** `MOVE_UP`, `MOVE_DOWN`
- **Door States:** `OPEN_DOOR_0`, `OPEN_DOOR_1`, `OPEN_DOOR_2`, `OPEN_DOOR_3`

## Simulation

The project has been tested using EDA Playground with multiple test scenarios:
- Single floor requests
- Multiple requests in the same direction
- Requests in both directions
- Idle state behavior

Simulation waveforms and results can be found in the `images/simulation/` directory.

## Testing Scenarios

The testbench validates:
1. ✓ Single floor request handling
2. ✓ Multiple requests in ascending direction
3. ✓ Multiple requests in descending direction
4. ✓ Mixed direction requests
5. ✓ Idle state maintenance
6. ✓ Door opening/closing timing
7. ✓ Reset functionality

## Usage

### Simulation on EDA Playground

1. Visit [EDA Playground](https://www.edaplayground.com/)
2. Copy the contents of `src/elevator_controller.v`
3. Create a testbench module to test various scenarios
4. Run the simulation and observe waveforms

### Local Simulation (ModelSim/Icarus Verilog)

```bash
# Using Icarus Verilog
iverilog -o elevator_sim src/elevator_controller.v testbench.v
vvp elevator_sim

# Using ModelSim
vlog src/elevator_controller.v testbench.v
vsim elevator_controller_tb
run -all
```

## Design Considerations

- **Timing:** All state transitions occur on clock edges
- **Synchronization:** All inputs are assumed to be clock-synchronized
- **Request Duration:** Floor and cabin requests are single clock cycle pulses
- **Non-simultaneous Requests:** Multiple simultaneous requests are not expected

## Documentation

Detailed documentation including state diagrams, design explanations, and simulation results can be found in the `docs/` directory:

- **project_report.pdf** - Complete project documentation with state diagrams and analysis
- **project_requirements.pdf** - Original project specifications
- **design_code.pdf** - Design code and implementation details
- **testbench_code.pdf** - Testbench implementation

## Future Enhancements

Potential improvements to the design:
- Emergency stop functionality
- Weight sensor integration
- Floor arrival announcements
- Priority floor override
- Multi-elevator system coordination

## License

This project was developed as part of the EECE 320 course curriculum at the American University of Beirut.

## Authors

Tarek Bshinnati and Soheil Haroun, under the guidance of Professor Mazen Saghir

---

**Note:** This is an educational project developed for learning purposes in digital systems design and HDL implementation.

# Field-Oriented Control for a PMSM

This project implements and tests a **Field-Oriented Control (FOC)** system for a Permanent Magnet Synchronous Motor (PMSM). The project combines MATLAB/Simulink motor-control models with Texas Instruments C2000 embedded-controller tests and hardware experiments.

The control system includes:

- Cascaded speed and current control loops
- Clarke and Park transformations
- Inverse Park transformation
- Space Vector PWM (SVPWM)
- Rotor-position feedback from an encoder
- Phase-current and DC-link voltage sensing
- PI-controller anti-windup protection
- Simulation, inverter testing, and C2000 deployment support

## Repository Structure

| Directory | Purpose |
| --- | --- |
| `FOC/` | Main FOC and PMSM Simulink models, simulation scripts, and generated model files |
| `Final_model_in_production/` | Production versions of the control models and generated code artifacts |
| `inverter Model/` | SVPWM/SPWM inverter models and inverter communication files |
| `Motor_Model_mathmatical/` | Mathematical motor-model development files |
| `C2000_tests/` | Embedded tests for the ADC, encoder, LEDs, and telemetry viewer |
| `Differential_Reciever_PCB/` | PCB-related files for the differential receiver and encoder interface |
| `presentation/` | Project presentation material |

## Requirements

### Simulation

- MATLAB and Simulink
- Motor Control Blockset
- A MATLAB release compatible with the included `.slx` models

### Hardware Deployment

- Texas Instruments Code Composer Studio (CCS)
- C2000Ware SDK
- A supported TI C2000 microcontroller and development board
- PMSM, inverter, encoder, current sensors, and DC-link voltage-sensing hardware

> Hardware work involves dangerous voltages and rotating machinery. Use suitable protection, current limits, emergency-stop equipment, and qualified supervision.

## Getting Started

1. Clone or download this repository.
2. Open MATLAB and set the repository root as the current folder.
3. Add the relevant model directory to the MATLAB path, for example `FOC/` or `Final_model_in_production/`.
4. Open the required Simulink model.
5. Run the matching initialization or tuning script before starting the simulation.
6. Check the motor parameters, controller gains, sampling time, sensor scaling, and inverter limits.
7. Run the simulation and inspect the scopes and generated plots.

## Main Models and Scripts

The exact model depends on the experiment. Common entry points include:

- `FOC/FOC_physical_2.slx` — FOC model
- `FOC/FOC_physical_2_prototybe.slx` — prototype FOC model
- `FOC/PMSM_Model.slx` — PMSM model
- `FOC/Communication.slx` — communication model
- `FOC/Model_Calculations.m` — model calculations and parameter preparation
- `FOC/FOC_physical_2_Testing_mathmatical.slx` — mathematical-model testing
- `inverter Model/SVPWM_inverter.slx` — SVPWM inverter model

Production models are kept in `Final_model_in_production/`. Use the corresponding initialization and tuning files in the same directory when working with those models.

## Simulation Checks

During simulation, verify the following signals:

- Rotor speed and speed-reference tracking
- $d$-axis and $q$-axis current responses
- Phase currents and current limits
- DC-link voltage
- SVPWM duty cycles and phase voltages
- PI-controller saturation and anti-windup behavior
- Encoder angle and electrical-angle calculations

Start with a low-speed, low-voltage test case and increase the operating point gradually. Confirm that the duty cycles, currents, and controller outputs remain within safe limits.

## C2000 Hardware Deployment

1. Open the appropriate C2000 test project from `C2000_tests/` in CCS.
2. Configure the target device, clock, ADC channels, PWM outputs, encoder interface, and serial/telemetry settings.
3. Verify the sensor wiring and signal polarity before connecting the inverter.
4. Build the project in CCS.
5. Connect the debugger and flash the generated program to the C2000 controller.
6. Test the ADC, encoder, PWM, and communication interfaces separately.
7. Enable the motor-control loop only after the individual hardware tests pass.
8. Monitor the system using the telemetry viewer and stop immediately if current, speed, or voltage exceeds the configured limits.

## Safety

This project controls a motor inverter and may use hazardous DC-link voltages. Before applying power:

- Inspect all high-voltage and phase connections.
- Confirm that the emergency stop works.
- Use current limiting and appropriate fuses.
- Keep clear of the rotating motor and coupling.
- Validate PWM dead time and gate-driver behavior.
- Never connect or disconnect motor phases while power is applied.
- Test new firmware with the lowest practical voltage and speed.

## Troubleshooting

| Problem | Things to check |
| --- | --- |
| Simulation does not start | MATLAB path, required toolboxes, initialization script, and model compatibility |
| Motor current is incorrect | Sensor offset, sensor scaling, phase order, Clarke/Park angle, and ADC configuration |
| Motor rotates in the wrong direction | Encoder direction, electrical-angle offset, phase sequence, and sign conventions |
| PWM output is unstable | DC-link measurement, controller gains, sampling time, saturation, and dead time |
| Hardware build fails | CCS project settings, C2000Ware installation, target device, and generated-code configuration |
| Telemetry data is missing | UART/IPC configuration, baud rate, signal mapping, and `TelementaryViewer/` settings |

## Team

- Ahmed Samy Mohamed Hagag
- Ahmed Maged Salah El-sayed
- Ahmed Yasser Mohamed Gamaleldin El Sadiq
- Mohamed Reda ELSayed Mahmoud ELSherbiny
- Mohamed Wagih El Sayed Sayed Ahmed
- Mostafa Ahmed Mohamed Amin
- Wael Mohamed Abdelhalim Moubark
- Youssef Ahmed Hassan Abdelkader

**Supervisors:** Dr. Mohamed Fekry, Dr. Mahmoud Gamil

**Institution:** Zagazig University, Faculty of Engineering
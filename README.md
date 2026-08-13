# Rotary BLDC Actuator Control Model

This repository contains a MATLAB/Simulink model of a rotary brushless DC (BLDC) actuator and its control architecture. The model is developed for controlled and smooth actuator motion using the Profile Position Mode (PPM) philosophy of the EPOS4 controller.

The project focuses on actuator-level modeling and on the full control chain required to generate, track, and execute a position command in simulation.

## Project objective

The main objective of this project is to model a rotary BLDC actuator and develop the associated control algorithm so that the motor can move smoothly, accurately, and in a controlled way. The implementation is organized around the cascaded control-loop structure required by the selected positioning strategy.

In the current version, the model includes:

- a trajectory generator,
- a position controller,
- a velocity controller,
- a current controller,
- Clarke-Park and inverse Clarke-Park transforms,
- inverter modeling,
- actuator modeling including motor, gearbox, load, and sensors.

## Control architecture

The control system follows a cascaded structure:

1. **Trajectory Generator**  
   Generates the position reference using a trapezoidal or triangular motion profile based on:
   - target position,
   - profile velocity,
   - acceleration,
   - deceleration.

2. **Position Control**  
   Compares reference position with measured position and generates a reference velocity.

3. **Velocity Control**  
   Compares reference velocity with measured velocity and generates the quadrature current reference.

4. **Current Control**  
   Regulates the direct and quadrature current components and produces the required voltage commands.

5. **Inverse Clarke-Park Transforms**  
   Convert the voltage commands from the rotating reference frame into three-phase voltage components.

6. **Voltage Source Inverter**  
   Converts the electrical control commands into three-phase motor excitation.

7. **Clarke-Park Transforms**  
   Convert measured three-phase currents back to the rotating reference frame for feedback control.

8. **Actuator Subsystem**  
   Contains the motor model, gearbox, load, and sensor measurements for position and velocity.

## Motion profile

The reference trajectory is generated in three motion phases:

- acceleration,
- constant-speed motion,
- deceleration.

This creates a trapezoidal velocity profile and allows smooth motion without abrupt speed transitions. If the target position is close enough, the constant-speed phase is omitted and the motion becomes triangular.

A stopping mechanism and a small position tolerance are included in order to reduce oscillations near the target.

## Main design choices

Some important implementation choices in the current model are:

- **Position controller:** P controller.
- **Velocity controller:** PI controller.
- **Current controller:** two PI controllers, one for each current-axis component.
- **Field-oriented operation target:** direct-axis current is regulated toward zero and quadrature-axis current follows the torque-producing reference.
- **Inverter model:** averaged inverter, selected for faster simulation, lower noise, and easier debugging compared to a switched inverter.

## Repository structure

A typical repository structure is:

- `config/` : configuration files and project settings
- `data/` : supporting data and parameters
- `models/` : Simulink models and control subsystems
- `resources/` : additional project material
- `model4.prj` : MATLAB project file

## Requirements

This project requires:

- MATLAB
- Simulink
- Simscape
- Simscape Electrical 
- Simscape Driveline

## How to use

1. Clone or download the repository.
2. Open MATLAB.
3. Open the project file:
   - `model4.prj`
4. Open the main Simulink model from the `models/` directory.

## Planned next steps

Planned improvements include:

- updating the mechanical parameters of the actuator and load,
- retuning the velocity and position controllers,
- replacing the averaged inverter with a switched inverter,
- revisiting solver settings if the inverter model is upgraded,
- investigating feedforward decoupling for higher-speed operation.

## Hardware basis

The model is based on the following actuator-related hardware references:

- Maxon EC frameless DT 85 M motor
- Spinea TwinSpin gearbox
- Maxon TSX 85 MAG encoder
- Maxon EPOS4 Compact 50/15 CAN controller

## Notes

This repository is intended to support development, simulation, documentation, and collaboration around the rotary actuator model. The structure and implementation details may evolve as the model becomes more physically accurate and more closely aligned with the real system.
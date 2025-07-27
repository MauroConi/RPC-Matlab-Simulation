# RPC-Matlab-Simulation
This MATLAB script simulates the dynamic response of a vehicle (quarter-car model) over a trapezoidal raised pedestrian crossing, analyzing displacement, velocity, acceleration, jerk, energy, power, wheel force, and frequency through Power Spectral Density (PSD).

README.txt  
Raised Pedestrian Crossing (RPC) Simulation – MATLAB Code  
Authors: Mauro Coni, Kevin Panetto, Enrico Giancaspro, Nicoletta Rassu, Francesca Maltinti  
Affiliation: DICAAR, University of Cagliari  

Overview  
--------  
This MATLAB script simulates the dynamic response of vehicles traversing Raised Pedestrian Crossings (RPCs) with varying geometric profiles and speeds. The simulation evaluates key mechanical parameters such as vertical displacement, acceleration, jerk, incident energy, dissipated power, and wheel force.  

The model is based on a quarter-car system and is intended to support the design of RPCs by quantifying their impact on vehicle dynamics and passenger comfort.  

Input Parameters  
----------------  
The following parameters can be modified within the script to simulate different conditions:  

1. **H** – Bump height (in meters)  
   - Example: `H = [0.07, 0.10, 0.13, 0.16]`  

2. **P** – Ramp slope (expressed as a decimal)  
   - Example: `P = [0.05, 0.07, 0.09, 0.11]`  
   - Ramp length is computed as `LR = H / P`  

3. **Vkm** – Vehicle speed (in km/h)  
   - Example: `Vkm = [20, 30, 40, 50]`  
   - Converted to m/s: `v = Vkm / 3.6`  

4. **Vehicle Type** – Defines mass, stiffness, and damping  
   - Passenger Car: `m = 300 kg`, `k = 30000 N/m`, `c = 3000 Ns/m`  
   - Ambulance: tuned for reduced jerk  
   - Heavy-Duty Vehicle (Fire Department): reinforced suspension  

Simulation Settings  
-------------------  
- **Simulation Time**: `t_end = 2.0 seconds`  
- **Time Step**: `dt = 0.001 seconds`  
- **Road Profile**: Trapezoidal bump with flat top and two ramps  
- **Vehicle Model**: Quarter-car system  

Outputs  
-------  
The script generates the following outputs for each simulation run:  

1. **Vertical Displacement (z_vehicle)** – Vehicle body movement over RPC  
2. **Vertical Velocity (vz_vehicle)** – Speed of vertical motion  
3. **Vertical Acceleration (az_vehicle)** – Key indicator of ride comfort  
4. **Vertical Jerk (jerk_vehicle)** – Rate of change of acceleration  
5. **Incident Energy** – Energy absorbed by suspension system  
6. **Dissipated Power** – Energy dissipated by damping  
7. **Force on Wheel** – Total vertical force transmitted to the wheel  
8. **Power Spectral Density (PSD)** – Frequency analysis of vertical response  

Output Format  
-------------  
Results are written to a text file named `risultati_simulazione.txt` and visualized through plots generated during execution.  

Usage  
-----  
To run the simulation:  
1. Open the script in MATLAB.  
2. Modify input parameters as needed.  
3. Execute the script.  
4. Review the output file and plots for analysis.  

License  
-------  
This code is provided for academic and research purposes. Please cite the original authors when used in publications.  

Contact  
-------  
For questions or collaboration inquiries, contact:  
Prof. Mauro Coni – mconi@unica.it 


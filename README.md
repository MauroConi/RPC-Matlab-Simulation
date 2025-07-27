clc; clear; close all;

v = 10;    % Vehicle speed (m/s)

% Parameters of the AUDI A4 vehicle
m = 300;  % Vehicle mass (kg)
k = 50000; % Suspension stiffness (N/m)
c = 3000;  % Suspension damping (N·s/m)

% Parameters of the ambulance vehicle
% m = 900;  % Vehicle mass (kg)
% k = 40000; % Suspension stiffness (N/m)
% c = 4000;  % Suspension damping (N·s/m)

% Parameters of the BUS vehicle
% m = 4000;  % Vehicle mass (kg)
% k = 140000; % Suspension stiffness (N/m)
% c = 10000;  % Suspension damping (N·s/m)


% Parameters of the trapezoidal bump
H = 0.10;  % Bump height (m)
LP = 10;   % Length of the elevated section (m)
LR = 1.43; % Length of the ramps (m)

% Simulation time
t_end = 2;  
dt = 0.001; 
t = 0:dt:t_end;  

% Vehicle position
x_vehicle = v * t;

% Generation of the trapezoidal bump profile
road_profile = zeros(size(x_vehicle));

idx1 = (x_vehicle >= 0) & (x_vehicle < LR);
idx2 = (x_vehicle >= LR) & (x_vehicle < LR + LP);
idx3 = (x_vehicle >= LR + LP) & (x_vehicle < 2*LR + LP);

road_profile(idx1) = H/LR * x_vehicle(idx1);
road_profile(idx2) = H;
road_profile(idx3) = H - H/LR * (x_vehicle(idx3) - (LP + LR));

% Simulation of the vehicle response (mass-spring-damper model)
z_vehicle = zeros(size(t));
vz_vehicle = zeros(size(t));
az_vehicle = zeros(size(t));  
jerk_vehicle = zeros(size(t)); 
incident_energy = zeros(size(t)); 
dissipated_power = zeros(size(t)); 
force_on_wheel = zeros(size(t)); 

for i = 2:length(t)
    F_spring = -k * (z_vehicle(i-1) - road_profile(i));
    F_damper = -c * vz_vehicle(i-1);
    F_total = F_spring + F_damper;

    az_vehicle(i) = F_total / m;
    vz_vehicle(i) = vz_vehicle(i-1) + az_vehicle(i) * dt;
    z_vehicle(i) = z_vehicle(i-1) + vz_vehicle(i) * dt;

    jerk_vehicle(i) = (az_vehicle(i) - az_vehicle(i-1)) / dt;
    incident_energy(i) = F_total * abs(z_vehicle(i) - z_vehicle(i-1));
    dissipated_power(i) = c * vz_vehicle(i)^2;
    force_on_wheel(i) = abs(F_spring) + abs(F_damper);
end

% Frequency analysis with PSD (Power Spectral Density)
[psd_response, freq] = pwelch(z_vehicle, [], [], [], 1/dt);

% Creation of the figure with a 3x3 grid
figure;
tiledlayout(3,3);

% Road profile
nexttile;
plot(t, road_profile, 'r', 'LineWidth', 2);
xlabel('Time (s)');
ylabel('Road profile (m)');
title('Road surface profile');
grid on;

% Vertical response
nexttile;
plot(t, z_vehicle, 'b', 'LineWidth', 2);
xlabel('Time (s)');
ylabel('Vertical response (m)');
title('Vehicle vertical response');
grid on;

% Vertical jerk
nexttile;
plot(t, jerk_vehicle, 'g', 'LineWidth', 2);
xlabel('Time (s)');
ylabel('Vertical jerk (m/s^3)');
title('Vehicle vertical jerk');
grid on;

% Frequency analysis with PSD (range 0 - 5 Hz)
nexttile;
plot(freq, psd_response, 'k', 'LineWidth', 2);
xlim([0 5]); % Zoom on x-axis
xlabel('Frequency (Hz)');
ylabel('PSD (m^2/Hz)');
title('Power Spectral Density (PSD) of vertical response');
grid on;

% Vertical speed
nexttile;
plot(t, vz_vehicle, 'm', 'LineWidth', 2);
xlabel('Time (s)');
ylabel('Vertical speed (m/s)');
title('Vehicle vertical speed');
grid on;

% Vertical acceleration
nexttile;
plot(t, az_vehicle, 'c', 'LineWidth', 2);
xlabel('Time (s)');
ylabel('Vertical acceleration (m/s^2)');
title('Vehicle vertical acceleration');
grid on;

% Incident energy
nexttile;
plot(t, incident_energy, 'y', 'LineWidth', 2);
xlabel('Time (s)');
ylabel('Incident energy (J)');
title('Incident energy');
grid on;

% Dissipated power
nexttile;
plot(t, dissipated_power, 'k', 'LineWidth', 2);
xlabel('Time (s)');
ylabel('Dissipated power (W)');
title('Dissipated power');
grid on;

% Force on the wheel
nexttile;
plot(t, force_on_wheel, 'b', 'LineWidth', 2);
xlabel('Time (s)');
ylabel('Force on the wheel (N)');
title('Force on the wheel');
grid on;

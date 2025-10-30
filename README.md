# EXP 3 : IIR-BUTTERWORTH-FITER-DESIGN

## AIM: 

 To design an IIR Butterworth filter  using SCILAB. 

## APPARATUS REQUIRED: 
PC installed with SCILAB. 

## PROGRAM (LPF): 
```
// Second-order Butterworth Low-Pass Filter Design in Scilab

// Step 1: Define filter specifications
fc = 1000;       // Cutoff frequency in Hz
fs = 10000;      // Sampling frequency in Hz (if discrete) - optional for continuous
wc = 2 * %pi * fc; // Cutoff angular frequency (rad/s)

// Step 2: Define s-variable for Laplace transform
s = poly(0, 's'); // s = Laplace variable

// Step 3: Define Butterworth second-order transfer function
// H(s) = wc^2 / (s^2 + sqrt(2)*wc*s + wc^2)
H = wc^2 / (s^2 + sqrt(2)*wc*s + wc^2);

// Step 4: Display the transfer function
disp("Second-order Butterworth Low-Pass Filter Transfer Function:");
disp(H);

// Step 5: Frequency response
f = 0:10:5000;          // Frequency vector from 0 to 5000 Hz
w = 2 * %pi * f;        // Convert to angular frequency
Hjw = horner(H, %i*w);  // Evaluate H(jw)
magnitude = abs(Hjw);    // Magnitude response
phase = atan(imag(Hjw)./real(Hjw)); // Phase response

// Step 6: Plot magnitude response
scf(0);
plot(f, 20*log10(magnitude));
xlabel("Frequency (Hz)");
ylabel("Magnitude (dB)");
title("Second-order Butterworth Low-Pass Filter - Magnitude Response");
grid();

// Step 7: Plot phase response
scf(1);
plot(f, phase*180/%pi);
xlabel("Frequency (Hz)");
ylabel("Phase (Degrees)");
title("Second-order Butterworth Low-Pass Filter - Phase Response");
grid();
```


## PROGRAM (HPF): 
```
clc;
clear;
close;

// User Inputs
wp = input('Enter the pass band frequency (Radians )= ');
ws = input('Enter the stop band frequency (Radians )= ');
alphap = input('Enter the pass band attenuation (dB)= ');
alphas = input('Enter the stop band attenuation (dB)= ');
T = input('Enter the Value of sampling Time= ');

// -------------------------
// Prewarping
// -------------------------
omegap = (2/T)*tan(wp/2);
mprintf("\nomegap = %f\n", omegap);

omegas = (2/T)*tan(ws/2);
mprintf("omegas = %f\n", omegas);

// -------------------------
// Order of Filter
// -------------------------
N = log10(((10^(0.1*alphas))-1) / ((10^(0.1*alphap))-1)) / (2*log10(omegap/omegas));
mprintf("N = %f\n", N);

N = ceil(N);
mprintf("Round off value of N = %d\n", N);

// -------------------------
// Cutoff Frequency
// -------------------------
omegac = omegap / (((10^(0.1*alphap)) - 1)^(1/(2*N)));
mprintf("omegac = %f\n", omegac);

// -------------------------
// Normalised Analog LPF Prototype
// -------------------------
mprintf("\nNormalised Analog LPF Transfer function H(S)=\n");
hs_Normalised = analpf(N, 'butt', [0,0], 1);
disp(hs_Normalised);

// -------------------------
// Analog HPF (LPF → HPF transformation)
// -------------------------
s = poly(0,'s');
hs_HPF = horner(hs_Normalised, omegac/s);
mprintf("\nAnalog HPF Transfer function H(S)=\n");
disp(hs_HPF);

// -------------------------
// Digital HPF (Bilinear Transform)
// -------------------------
z = poly(0,'z');
Hz_HPF = horner(hs_HPF, (2/T)*((z-1)/(z+1))); 
mprintf("\nDigital HPF Transfer function H(Z)=\n");
disp(Hz_HPF);

// -------------------------
// Frequency Response (with grid)
// -------------------------
HW_HPF = frmag(Hz_HPF, 512);
w = 0:%pi/511:%pi;

plot(w/%pi, abs(HW_HPF));
xlabel(' Normalized Frequency (w/π)');
ylabel('Magnitude');
title('Frequency Response of Butterworth IIR HPF');
xgrid();
```


## OUTPUT (LPF) : 


## OUTPUT (HPF) : 

## RESULT: 

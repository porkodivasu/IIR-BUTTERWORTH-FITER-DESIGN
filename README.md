# EXP 3 : IIR-BUTTERWORTH-FITER-DESIGN
# REG:212223060199
## AIM: 

 To design an IIR Butterworth filter  using SCILAB. 

## APPARATUS REQUIRED: 
PC installed with SCILAB. 

## PROGRAM (LPF): 
```
clc;
close;
wp = input('Enter the pass band frequency (Radians )= ');
ws = input('Enter the stop band frequency (Radians )= ');
alphap = input('Enter the pass band attenuation (dB)= ');
alphas = input('Enter the stop band attenuation(dB)= ');
T = input('Enter the Value of sampling Time= ');
omegap = (2/T)*tan(wp/2);
omegas = (2/T)*tan(ws/2);

Ncalc = log10(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1)) / (2*log10(omegas/omegap));
N = ceil(Ncalc);

omegac = omegap / (((10^(0.1*alphap)) - 1)^(1/(2*N)));


disp("Computed (non-integer) N = " + string(Ncalc));
disp("Rounded N = " + string(N));
disp("Analog cutoff omegac = " + string(omegac));

hs = analpf(N, 'butt', [0,0], omegac); 
disp(hs);
z = poly(0, 'z'); 
Hz = horner(hs, (2/T)*((z-1)/(z+1)));
disp("Digital H(z) = ");
disp(Hz);

HW = frmag(Hz, 1024); 
w = 0:%pi/1023:%pi;
plot(w/%pi, abs(HW));
xlabel('Normalized Digital Frequency w');
ylabel('Magnitude');
title('Frequency Response of Butterworth IIR LPF');
```



## PROGRAM (HPF): 
```
clc;
clear;
close;

wp = input('Enter the pass band frequency (Radians )= ');
ws = input('Enter the stop band frequency (Radians )= ');
alphap = input('Enter the pass band attenuation (dB)= ');
alphas = input('Enter the stop band attenuation (dB)= ');
T = input('Enter the Value of sampling Time=');

omegap = (2/T)*tan(wp/2);
disp(omegap, 'omegap=');
omegas = (2/T)*tan(ws/2);
disp(omegas, 'omegas=');

N = log10(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1))/(2*log10(omegap/omegas));
disp(N, 'N=');
N = ceil(N);
disp(N, 'Round off value of N=');

omegac = omegas/(((10^(0.1*alphas)) -1)^(1/(2* N)));
disp(omegac, 'omegac=');
disp('Normalized Analog LPF Transfer function H(s)=');
hs_Normalised = analpf(N,'butt',[0,0],1);
disp(hs_Normalised);

disp('Analog LPF Transfer function H(s)=');
hs = analpf(N,'butt',[0,0],omegac);
disp(hs);

s = poly(0,'s');
hs_hp = horner(hs, (omegac^2)/s);  

disp('Analog HPF Transfer function H(s)=');
disp(hs_hp);

z = poly(0,'z');
Hz = horner(hs_hp, (2/T)*((z - 1)/(z + 1)));

disp('Digital HPF Transfer function H(z)=');
disp(Hz);
HW = frmag(Hz,512);
w = 0:%pi/511:%pi;

plot(w/%pi, abs(HW));
xlabel('Normalized Digital Frequency (×π rad/sample)');
ylabel('Magnitude');
title('Frequency Response of Butterworth IIR High-pass Filter');
xgrid();
```



## OUTPUT (LPF) : 

<img width="960" height="876" alt="image" src="https://github.com/user-attachments/assets/94e85944-da90-471e-8b3c-05a40c8d4dce" />



## OUTPUT (HPF) : 

<img width="993" height="765" alt="image" src="https://github.com/user-attachments/assets/f89e6757-7137-4097-a6d3-75b7eb1a5f02" />



## RESULT: 

Thus, design of Butterworth Low pass and High pass IIR filter waveforms were plotted and output was verified.

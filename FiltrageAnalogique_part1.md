# FiltrageAnalogique
clear all 
close all 
clc

sample_time = 0.0005;
time_vector = 0:sample_time:5-sample_time;
sample_rate = 1/sample_time;
num_samples = length(time_vector);
cutoff_frequency = 2*pi*50;

frequency1 = 500;
frequency2 = 400;
frequency3 = 50;

%  tracage de signal
signal = sin(2*pi*frequency1*time_vector)+ sin(2*pi*frequency2*time_vector)+sin(2*pi*frequency3*time_vector); 
plot(time_vector,signal);
% tracage de spectre
spectrum = fft(signal);
frequency_vector = (-num_samples/2:num_samples/2-1)*(sample_rate/num_samples);
% plot(frequency_vector,fftshift(abs(spectrum)))
title("spectre Amplitude")
% tracage de fct H
angular_frequency = 2*pi*frequency_vector;
%  transmettance Complexe c'est le filtre utilise pour faire le filtrage de signal
filter_function=(1i*angular_frequency/cutoff_frequency)./(1+1i*(angular_frequency/cutoff_frequency));
 plot(frequency_vector,abs(filter_function))
 title(" tracage de fct H")

filter_function1=((1i*angular_frequency)/50)./(1+1i*(angular_frequency/50));
filter_function2=((1i*angular_frequency)/50)./(1+1i*(angular_frequency/150));
filter_function3=((1i*angular_frequency)/50)./(1+1i*(angular_frequency/350));
subplot(2,1,1)
semilogx(frequency_vector,20*log(abs(filter_function1)),frequency_vector,20*log(abs(filter_function2)),frequency_vector,20*log(abs(filter_function3)))
title(" tracage de trois gain(Bode Diagram)")
grid on

% tracage diphasage le gain de fct H1 et H2 et H3 
%ce diphasage egale arg(h)
%ce tracage nous permets de faire un observation de dephasage se produit par l'information et le bruit

 filter_function1=((1i*angular_frequency)/50)./(1+1i*(angular_frequency/50));
filter_function2=((1i*angular_frequency)/50)./(1+1i*(angular_frequency/150));
filter_function3=((1i*angular_frequency)/50)./(1+1i*(angular_frequency/350));
subplot(2,1,2)

 semilogx(frequency_vector,angle(filter_function1),frequency_vector,angle(filter_function2),frequency_vector,angle(filter_function3))
 title(" tracage de trois phase(Bode Diagram)")
 grid on
 %%on fait le produit entre le signal et h selon la valeur de

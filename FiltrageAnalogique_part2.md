# FiltrageAnalogique
clear all
close all
clc
%cette fonction utiliser pour extraire la vecteur de audio musical
[audio, sample_rate] = audioread('test.wav');
%rendrela vecteur lineaire pour eviiter le probleme de dimension
audio = audio';
%extraire le length de audio
num_samples = length(audio);

frequency_vector = (0:num_samples-1)*(sample_rate/num_samples);
frequency_vector_shift = (-num_samples/2:num_samples/2-1)*(sample_rate/num_samples);
%appliquer la transformation de faurier directe
audio_spectrum = fft(audio);
% afficher le spectre de signal pour visualiser les pics de bruit 
plot(frequency_vector_shift,fftshift(abs(audio_spectrum)));

filter_order = 1;%ordre de filtrage ce paremtre nous permet de rendre le signal ideale aussi faire l'amplification
cutoff_frequency = 4500;%frequence de corpure 
%la transmitance complexe 
transfer_function = filter_order./(1+1j*(frequency_vector/cutoff_frequency).^100);

filter_function = [transfer_function(1:floor(num_samples/2)), flip(transfer_function(1:floor(num_samples/2)))];%on faire le choix de cet intervalle pour filter le signale et son symtrique par pour a fs/2
%faire le filtrage 
filtered_spectrum = audio_spectrum(1:end-1).*filter_function;

filtered_audio= ifft(filtered_spectrum,"symmetric");
%afficher la filtre (la transmettance complexe)
semilogx(frequency_vector(1:floor(num_samples/2)),abs( transfer_function(1:floor(num_samples/2))),'linewidth',1.5)
%afficher le signal filter
plot(frequency_vector_shift(1:end-1),fftshift(abs(fft(filtered_audio))))

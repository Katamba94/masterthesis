# masterthesis
%% SCRIPT DE TRAITEMENT DES SIGNAUX VIBRATOIRE
%%
clear;
close all;
%cd('E:\Users\Ir dany\Documents\MsThesis_dany')
%% NIVEAU 0:  Importation des signaux
%%
data=lvm_import('outilNeuf_v100_avanceE_p1,5_18-03-28_1449_009');
x11=data.Segment1.data;
signal_on=x11(:,1);
 %Faibleusure
data=lvm_import('outilfaibleUsure_v100_avanceE_p1_18-03-28_1510_008');
x12=data.Segment1.data;
signal_fu=x12(:,1);
 %Usure avanc�e
 data=lvm_import('outilUse_v100_avanceE_p1_18-03-28_1510_009');
 x13=data.Segment1.data;
signal_ua=x13(:,1);
% 
%% %% %Parametres du signal
Fs=25e3; % frequence d'echatillonnage 
h=1/Fs;% pas d'echantillon
N11=length(signal_on);% longueur du signal pour l'ouitil neuf
N12=length(signal_fu);% longueur du signal pour l'ouitil neuf
N13=length(signal_ua);% longueur du signal poiur une faible usure
t=0:h:(N11-1)*h;% vecteur de temps
%% filtrage 
ordre=3;
fmax=Fs/2;
 Wn =[3000 4000]/fmax;% frequence normalisee
[b,a]=butter(ordre,Wn,'bandpass'); % filtre Butterworth
 signfilt_on = filter(b,a,signal_on);
 signfilt_fu = filter(b,a,signal_fu);
 signfilt_ua = filter(b,a,signal_ua);

 %% 
 RMS_on=rms(signal_on); % valeur efficace pour l'outil neuf
 RMS_fu=rms(signal_fu); % valeur efficace pour l'outil neuf
 RMS_ua=rms(signal_ua); % valeur efficace pour une faible usure4
 
KURTOSIS_on=kurtosis(signal_on); % coeficient d'applatissement pour l'outil neuf
KURTOSIS_fu=kurtosis(signal_fu); % coeficient d'applatissement pour l'outil neuf
KURTOSIS_ua=kurtosis(signal_ua); % coeficient d'applatissement pour une fable usure

FC_on=max(abs(signal_on))/rms(signal_on); % Facteur de crete pour l'outil neuf
FC_fu=max(abs(signal_fu))/rms(signal_fu); % Facteur de crete pour l'outil neuf
FC_ua=max(abs(signal_ua))/rms(signal_ua); % Facteur de crete pour une fable usure

skewness_on=skewness(signal_on); % coeficient d'asymetrie pour l'outil neuf 
skewness_fu=skewness(signal_fu); % coeficient d'asymetrie pour l'outil neuf
skewness_ua=skewness(signal_ua); % coeficient d'asymetrie pour une faible usure

var_on=var(signal_on); % la variance pour l'outil neuf 
var_fu=var(signal_fu); % la variance pour l'outil neuf 
var_ua=var(signal_ua); % la variance pour une faible usure

mean_on=mean(signal_on); % la moyenne pour l'outil neuf 
mean_fu=mean(signal_fu); % la moyenne pour l'outil neuf
mean_ua=mean(signal_ua); % la moyenne pour une faible

% 
%% 
[F11,Pz11] = tfdr_moy(signal_on,h,1,0);
[F12,Pz12] = tfdr_moy(signal_fu,h,1,0);
[F13,Pz13] = tfdr_moy(signal_ua,h,1,0);

%% 
%% 
figure(1);
plot(signal_on,'b')
ylabel('Amplitude[V]')
xlabel('Temps[s]');
title('Signal Brut:Outil neuf');
legend('Capteur1')

  figure(2);
plot(t,signal_fu,'b') 
xlabel('Temps[s]');
ylabel('Amplitude[V]')
title('Signal Brut: Faible usure ');
legend('Capteur1')

 figure(3);
plot(t,signal_ua,'b') 
xlabel('Temps[s]');
ylabel('Amplitude[V]')
title('Signal Brut:Usure avanc�e');
legend('Capteur1')
%% 
%% 
figure(4);

plot(t, signfilt_on,'b')
xlabel('Temps[s]');
ylabel('Amplitude[V]')
title('Signal filtr� :Outil neuf')
legend('Capteur1')

figure(5);
 
plot(t,signfilt_fu,'b')
xlabel('Temps[s]');
ylabel('Amplitude[V]')
title('Signal filtr� :Faible usure')
legend('Capteur1')

figure(6);

plot(t,signfilt_ua,'b')
xlabel('Temps[s]');
ylabel('Amplitude[V]')
title('Signal filtr� :Usure avanc�e')
legend('Capteur1')
grid


%% 
figure(7);
bar([RMS_on,RMS_fu,RMS_ua],'b');title('RMS') 
figure(8);
bar([FC_on,FC_fu,FC_ua],'b');title('FC')
figure(9);
bar([KURTOSIS_on,KURTOSIS_fu,KURTOSIS_ua],'b');title('Kurtosis')
figure(10);
bar([var_on,var_fu,var_ua],'b');title('variance')
figure(11);
bar([mean_on,mean_fu,mean_ua],'b');title('Moyenne')
%% %% %% %% 
figure(12);
%subplot(311);
plot(F11,Pz11,'g');
xlim([0 14000]);
ylabel('Desit� spectrale[m/s^2]');
xlabel('Frequences [Hz]');
%title('Signal frequentiel outil neuf');
legend('spectre outil neuf')

%subplot(312);
%figure(13);
hold on
plot(F12,Pz12,'r');
xlim([0 14000]);
%ylabel('Amplitude [V]');
%xlabel('Frequences [Hz]');
%title('Signal frequentiel faible usure');
legend('spectre faible usure')

%figure(14);
plot(F13,Pz13,'b');
xlim([0 14000]);
%ylabel('Amplitude [V]');
%xlabel('Frequences [Hz]');
%title('Signal frequentiel usure avanc�e')
legend('spectres usure avanc�e');
hold off

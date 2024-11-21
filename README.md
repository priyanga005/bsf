% FIR Band-Stop Filter Design using Hanning Window
clc;
clear;

% Specifications
fs = 1000;               % Sampling frequency in Hz
f1 = 100;                % Lower stopband frequency in Hz
f2 = 300;                % Upper stopband frequency in Hz
N = 51;                  % Filter order (odd for symmetric FIR filter)

% Normalized cutoff frequencies (0 to 1)
w1 = 2 * pi * (f1 / fs); % Lower stopband frequency in radians
w2 = 2 * pi * (f2 / fs); % Upper stopband frequency in radians

% Time index for impulse response
n = -(N-1)/2:(N-1)/2;    % Symmetric indices

% Ideal impulse response for band-stop filter
h_lowpass = (w1/pi) * sinc(w1/pi * n);  % Low-pass component
h_highpass = sinc(n) - (w2/pi) * sinc(w2/pi * n);  % High-pass component
h_ideal = h_lowpass + h_highpass;       % Combine for band-stop

% Apply Hanning Window
h_hanning = h_ideal .* hanning(N)';

% Plot Impulse Response
figure;
stem(n, h_hanning, 'filled');
title('Impulse Response of FIR Band-Stop Filter (Hanning Window)');
xlabel('n');
ylabel('h[n]');
grid on;

% Frequency Response
[H, f] = freqz(h_hanning, 1, 1024, fs);  % Compute frequency response
figure;
plot(f, abs(H), 'b', 'LineWidth', 1.5);
title('Frequency Response of FIR Band-Stop Filter (Hanning Window)');
xlabel('Frequency (Hz)');
ylabel('Magnitude');
grid on;

% Show Stopband Frequencies
hold on;
xline(f1, 'g--', 'LineWidth', 1);  % Lower stopband frequency
xline(f2, 'r--', 'LineWidth', 1);  % Upper stopband frequency
legend('Magnitude Response', 'Lower Stopband', 'Upper Stopband');

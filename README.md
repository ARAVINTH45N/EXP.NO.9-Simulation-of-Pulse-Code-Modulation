# Pulse Code Modulation (PCM) and Time Division Multiplexing (TDM) Experiment

## Aim

To simulate and visualize the process of Pulse Code Modulation (PCM) for two analog signals and their subsequent Time Division Multiplexing (TDM).

## Apparatus Required

* Personal Computer with Python 3.x installed.
* Libraries: NumPy, Matplotlib.

## Procedure

1.  Import the necessary libraries (NumPy and Matplotlib).
2.  Define the parameters for the simulation, including sampling rate, duration, quantization levels, and frequencies of the message and clock signals.
3.  Generate the time vector based on the sampling rate and duration.
4.  Generate two analog message signals (sine waves) with different frequencies and amplitudes.
5.  Generate a clock signal (square wave) for the multiplexing process.
6.  Quantize each of the analog message signals using a uniform quantization method based on the defined number of quantization levels.
7.  Simulate a simple Time Division Multiplexing (TDM) technique by interleaving the samples of the two quantized signals. In this simulation, alternating samples from each quantized signal are used.
8.  Plot the original analog message signals, the clock signal, the quantized signals, and the final multiplexed signal using Matplotlib.
9.  Analyze the resulting plots to understand the effect of quantization and the process of time division multiplexing.

## Program Code
```

python
import numpy as np
import matplotlib.pyplot as plt

# Parameters
sampling_rate = 5000  # Sampling rate (samples per second)
duration = 0.1  # Duration of the signal in seconds
quantization_levels = 16  # Number of quantization levels (PCM resolution)
clock_frequency = 200  # Clock signal frequency

# Message signal 1 parameters
frequency1 = 50  # Frequency of message signal 1
message_signal1_amplitude = 1.0 # Amplitude of message signal 1

# Message signal 2 parameters
frequency2 = 100  # Frequency of message signal 2
message_signal2_amplitude = 0.8 # Amplitude of message signal 2

# Generate time vector
t = np.linspace(0, duration, int(sampling_rate * duration), endpoint=False)

# Generate message signals
message_signal1 = message_signal1_amplitude * np.sin(2 * np.pi * frequency1 * t)
message_signal2 = message_signal2_amplitude * np.sin(2 * np.pi * frequency2 * t)

# Generate clock signal
clock_signal = np.sign(np.sin(2 * np.pi * clock_frequency * t))

# Quantize message signal 1
quantization_step1 = (max(message_signal1) - min(message_signal1)) / quantization_levels
quantized_signal1 = np.round(message_signal1 / quantization_step1) * quantization_step1

# Quantize message signal 2
quantization_step2 = (max(message_signal2) - min(message_signal2)) / quantization_levels
quantized_signal2 = np.round(message_signal2 / quantization_step2) * quantization_step2

# Simulate PCM modulated signals
pcm_signal1 = (quantized_signal1 - min(quantized_signal1)) / quantization_step1
pcm_signal1 = pcm_signal1.astype(int)

pcm_signal2 = (quantized_signal2 - min(quantized_signal2)) / quantization_step2
pcm_signal2 = pcm_signal2.astype(int)

# Multiplexing (simple time-division multiplexing)
multiplexed_signal = np.zeros_like(t)
for i in range(len(t)):
    if i % 2 == 0:
        multiplexed_signal[i] = quantized_signal1[i]
    else:
        multiplexed_signal[i] = quantized_signal2[i]

# Plotting the results
plt.figure(figsize=(14, 12))

# Plot message signal 1
plt.subplot(5, 1, 1)
plt.plot(t, message_signal1, label="Message Signal 1 (Analog, 50Hz)", color='blue')
plt.title("Message Signal 1 (Analog, 50Hz)")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)

# Plot message signal 2
plt.subplot(5, 1, 2)
plt.plot(t, message_signal2, label="Message Signal 2 (Analog, 100Hz)", color='green')
plt.title("Message Signal 2 (Analog, 100Hz)")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)

# Plot clock signal
plt.subplot(5, 1, 3)
plt.plot(t, clock_signal, label="Clock Signal (200Hz)", color='purple')
plt.title("Clock Signal (200Hz)")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)

# Plot quantized signals
plt.subplot(5, 1, 4)
plt.step(t, quantized_signal1, label="Quantized Signal 1", color='red', where='post')
plt.step(t, quantized_signal2, label="Quantized Signal 2", color='orange', where='post')
plt.title("Quantized Signals")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)
plt.legend()

# Plot Multiplexed Signal
plt.subplot(5,1,5)
plt.step(t, multiplexed_signal, label = "Multiplexed Signal", color = 'black', where='post')
plt.title("Multiplexed Signal")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)

plt.tight_layout()
plt.show()
```

## Output
![image](https://github.com/user-attachments/assets/3f740752-994f-406a-977e-7dd53a81cea0)


## Result
The simulation successfully demonstrates the process of Pulse Code Modulation (PCM) applied to two analog signals and their subsequent Time Division Multiplexing (TDM). The top two plots show the original analog signals. The third plot illustrates the clock signal used for sampling and multiplexing. The fourth plot displays the quantized versions of the two analog signals, showing the discrete amplitude levels introduced by the quantization process. Finally, the bottom plot shows the multiplexed signal, where samples from the two quantized signals are interleaved in time. This visualization helps in understanding how multiple signals can be transmitted over a single channel by allocating them different time slots.

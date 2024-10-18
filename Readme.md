# Applicant's Task - Signal analysis

Imagine, your SDR records a baseand signal (in complex-values IQ samples) of unknown characteristic. You know it's an RF signal that is used for communication, i.e. it contains some digital data.

Your task is to analyze the signal and understand its contents as deep as possible. Points to identify might be:

- Which modulation is used?
- Which parameters (like frame length, allocated bandwidth, ...) does the signal have?
- Which impairments are degrading the signal quality?
- Which structure does the signal have? Are there pilots or reference signals contained?

Some hints: 
- The signal is received over a flat channel (i.e. no multipath effect is present)
- It is known that the signal is CP-OFDM-modulated with CP and FFT size constant over all symbols. 
- It is known that the FFT size is a power of two and <2^13
- It is known that the CP overhead is between 10-20% of the OFDM symbol

You can use Python or C++ to analyze the record. In python, the received signal can be loaded by

```
import numpy as np

rx_signal = np.load("filename.npy")
```

Also, the data is provided as a CSV-file where the first and second column represent the real and imaginary parts of the RX signal, respectively.

# Applicant's Solution

Please find attached pdf for code, results and short discussion. [Solution](task_solution.pdf)

Brief description of performed signal analysis is given below,

- ### Power Spectral Density (PSD) Analysis:

  - **Method**: Welch’s method was applied to calculate and visualize the PSD of a received signal. This method reduces signal variance, helping to observe key features such as occupied bandwidth and null subcarriers.
  - **Results**: The normalized occupied bandwidth was found to be 59%, with 1200 out of 2048 carriers occupied. The DC subcarrier was null.

- ### Cyclic Prefix (CP) and FFT Length Detection via Correlation:

  - **Method**: CP correlation was used to detect FFT lengths by iterating through possible values and identifying peaks in the correlation. A low-pass filter was applied to smooth the correlation profile, and valid peaks were detected based on specific thresholds.
  - **Results**: The FFT length was detected to be 2048, and the CP length was found to be 256, with a detected frequency offset of approximately -2.94e-6 rad/sample.

- ### Post-FFT Analysis:

  - **Method**: After discarding CP samples, FFT was performed on OFDM symbols.Frequency and phase offset is present in the signal as observed in constellation diagrams. A correction for frequency offset on all time domain samples was applied. However, further corrections weren't made because of lack of knowledge about used pilot symbols or reference symbols (sequence and location in time-freq grid).
  - **Results**: The detected modulation scheme was 16-QAM. Out of the 2048 subcarriers, 1198 were found to be occupied for most symbols, excluding the DC subcarrier.

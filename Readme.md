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

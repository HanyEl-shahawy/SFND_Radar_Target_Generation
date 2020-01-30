## Radar Specifications
%%%%%%%%%%%%%%%%%%%%%%%%%%%
% Frequency of operation = 77GHz
% Max Range = 200m
% Range Resolution = 1 m
% Max Velocity = 100 m/s
%%%%%%%%%%%%%%%%%%%%%%%%%%%

## Implementation
 * FMCW waveform generation using the specs of Radar. Calculated Bandwidth, Chirp Time and Slope.
 * Generating signal simulating moving target at 100m with velocity 50m/s
 * Running FFT on beat signal and normalize. We are interested in only half of spectrum
 * Running 2D FFT on the beat signal to generate range doppler map [RDM]
 * 2D FFT output is an image that has reponse in the range and doppler FFT bins. So, it is important to convert the axis from bin sizes to range and doppler based on their Max values. Then again take one side of signal from range dimension.
 * Implement CA-CFAR technique to suppress clutter, specs of cells are:
	train_cells = 10
	train_band = 8
	guard_cells = 4
	guard_band = 4
	threshold_offset = 1.4
 * From line 188-line 214, i created 2 nested loops to loop over the 2D block. Each time i sum noise in surronding training cells. Then calculate average, convert it to db and add the configured offset.
 * Afterwards, the signal level in CUT is compared with the noise level within the surronding region, if it is higher, a value of 1 is assigned else 0. 
 * Since cell-under-test cannot be located at edge of matrix, all cells which have not been processed in range doppler map are set to 0. The threshold block is the same size as RDM now.

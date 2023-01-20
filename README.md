# spectroPS
Machine learning tools for spectral pulse shaping in python

An artificial neural network (ANN) is used to determine the ideal amplitude mask that can transform a given pulse spectrum into a desired pulse 
spectrum using an acousto-optic programmable dispersive filter (AOPDF), such as the Dazzler by Fastlite.

The AOPDF is a literal black box which shapes the spectral amplitude and phase of a light pulse as a laser beam is transmitted
through the box. As shown in Fig. 1, the AOPDF accepts a 1D amplitude mask vector, A, and an input pulse spectrum, I, and outputs a shaped pulse spectrum, S.

![alt text](https://github.com/pbrosseau/spectroPS/blob/main/spectroPS_pulse_shaper_diagram.png?raw=true)
 <br />Fig. 1: Acousto-optic programmable dispersive filter (AOPDF) input and output.

Generally, one desires the pulse spectrum to have a specific shape, such as a super-Gaussian (Fig. 1, orange line). One would therefore 1) measure the
pulse spectrum without an amplitude mask applied by the AOPDF (Fig. 1, blue line), 2) calculate a mask by dividing the desired spectrum by the 
current spectrum and 3) measure the pulse spectrum with the amplitude mask to confirm that the masked spectrum resembles the
desired spectrum.

![alt text](https://github.com/pbrosseau/spectroPS/blob/main/spectroPS_test_goal.png?raw=true)
 <br />Fig. 2: Blue: spectrum of pulse without an amplitude mask and orange: desired pulse spectrum.

This task is complicated by several factors, as the ideal amplitude mask is not exactly equal to the desired spectrum divided by
the measured un-masked spectrum. First, the AOPDF masks the amplitude rather than the intensity, so the square root of the measured 
spectrum must be used. Secondly, there is generally a slight calibration off-set between the AOPDF wavelength vector and the
spectrometer wavelength vector, requiring that the calculated mask is shifted or re-scaled. Thirdly, both the spectrometer and AOPDF can
have non-linear responses. These three factors make determination of the ideal mask vector a non-trivial task, and the calculated
mask vector generally differs from the ideal mask vector by some small amount, which could be enough to significantly change the
shape of the masked pulse spectrum.

The ANN is used to characterize the AOPDF transfer function and predict the ideal mask for a given input spectrum and a goal spectrum.
Fig. 2 shows that, once trained, the ANN can predict the ideal amplitude mask for the input spectrum and goal spectrum in Fig. 1.
The shaped output spectrum with the modelled amplitude mask indeed resembles the desired spectrum, as show in Fig. 3.

![alt text](https://github.com/pbrosseau/spectroPS/blob/main/spectroPS_masks.png?raw=true)
 <br />Fig. 3: Blue: the ideal amplitude mask to transform the input pulse into the desired pulse and orange: the ANN predicted amplitude mask.

![alt text](https://github.com/pbrosseau/spectroPS/blob/main/spectroPS_predicted_goal.png?raw=true)
 <br />Fig. 4: Blue: spectrum of shaped pulse with the ideal amplitude mask and orange: spectrum of shaped pulse with the ANN predicted amplitude mask.

The measured spectrum has length n. The ANN accepts a 1D vector, consisting of the measured spectrum concatenated with the desired spectrum, 
with length 2n. The ANN outputs a 1D vector, corresponding to the amplitude mask, of length n.

Patrick Brosseau 2023/01/20

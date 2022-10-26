Pulsewave
=========


Pulse Wave Introduction
-----------------------
Pulse wave analysis from Doppler holography



Here is one picture of the video of pulse wave analysis to the program. The program called One Cycle will give such results.

Calculation of Pulse Init
-------------------------

Let's move on the step of Pulse Init calculations. The application which has been made allows us to see the pulse init graphic, which in order words allow us to easily see every pulses and then allow us to calculate them. Here's an example: 


This is a sum of signal pulse of the video implemented to the pulse wave program. The first time the pulse increases is a start of a first cycle. Let's see a detailed explanation of the pulse wave system:


A systole is when the signal increases, which can be seen each time the pulse goes higher. Meanwhile a diastole is when the signal decreases, which can be seen each time the pulse goes lower.

We'll now move on the another part which is the Artery Mask.

Artery Mask
-----------

We'll start with a technical part in which the artery mask algorithm is shown:

    ``
    function artery_mask = createArteryMask(video)
    mask = std(video, 0, 3);
    mask = imbinarize(im2gray(mask), 'adaptive', 'ForegroundPolarity', 'bright', 'Sensitivity', 0.2);
    pulse = squeeze(mean(video .* mask, [1 2]));

    pulse_init = pulse - mean(pulse, "all");
    C = video;
    for kk = 1:size(video, 3)
        C(:,:,kk) = video(:,:,kk) - squeeze(mean(video, 3));
    end
    pulse_init_3d = zeros(size(video));
    for mm = 1:size(video, 1)
        for pp = 1:size(video, 2)
            pulse_init_3d(mm,pp,:) = pulse_init;
        end
    end
    C = C .* pulse_init_3d;
    C = squeeze(mean(C, 3));

    artery_mask = C > max(C(:))*0.2;

    end
    ``


We'll focus on this particular part of code:

``
    pulse_init = pulse - mean(pulse, "all");
``

This part shows an initial pulse : this is the first assessment of averaged pulse wave.

If we watch another part of code, we'll find this:

```
    C = C .* pulse_init_3d;
```
It is a system that correlate all pixels current pulse with the initial pulse.

What is an Artery Mask?
-----------------------

Artery Mask allows us to find the arteries & veins for the given video implemented in the pulse wave program.


An artery Mask is the result of the correlation between pulse init & each pixel. The yellow part is represented by a binary number with the following explanation: when the binary number equals 1, it shows an artery.


Analysis of Complete cycles
---------------------------

We now have a comparison between a pulse init and initial pulse 0:


The red part is representing the pulse init, meanwhile the blue part is the pulse init 0 (Initial pulse : first assessment of averaged pulse wave).

We will now focus on a function called "Detrend". Here's a visual representation about it:


The linear function is represented by a binary number with the following explanation: when the binary number equals 1, it shows an artery.

Moving on further, another application has been developed in which the goal is to remove incomplete cycles, in order to make the cycle analysis easier. Here's a picture explaining the goal: 


The goal is to delete incomplete cycles.

To do so, we used an algorithm of an application to analyse the cycles, let's watch it: 

```
filepath = uigetfile("*");
V = VideoReader(filepath);
video = zeros(V.Height, V.Width, V.NumFrames);        
for n = 1 : V.NumFrames
    video(:,:,n) = rgb2gray(read(V, n));
end
for pp = 1:size(video, 3)
    video(:,:,pp) = video(:,:,pp) ./ mean(video(:,:,pp), [1 2]);
end
mask = std(video, 0, 3);
mask = imbinarize(im2gray(mask), 'adaptive', 'ForegroundPolarity', 'bright', 'Sensitivity', 0.2);
pulse = squeeze(mean(video .* mask, [1 2]));
pulse_init = pulse - mean(pulse, "all");
y = pulse_init;
y = y/max(pulse_init);
y = detrend(y);
m = islocalmin(y);
jj = 1;
for ii = 1:size(m)
    if m(ii) && y(ii) < 0
        index(jj) = ii;
        jj = jj + 1;
    end
end
plot(app.UIAxes, y(index(1):index(size(index, 2))));
```

If we get on it further, we'll see the pulse init calculation init, represented by the following code:

```
y = y/max(pulse_init);
```

Then after doing this calculation, we'll use the detrend function for this:

```
y = detrend(y);
```

This is how it is used in our algorithm. It represents the detrend application of a pulse init.

Next, we did find peaks of the pulse init calculation in order to analyse it. Here's a result of it:


Every stars which are shown in red represents a peak. Our goal will be to analyse it further.




OneCycle Application
--------------------

In order to get a graphic as a result of the inputted video, you would need to use the OneCycle application.

First of all, open Matlab and make sure to be in the correct repository. The repository name is "Pulse Wave". Once you're in the correct repository, in the terminal type "one_cycles". Therefore it should open an application like this:

Once this is open, click on "Load files". Among all the files, you have to select the video you'd like to analyse. Once the video is added, it should show an information saying it has been set as the video which will be analysed. You can or not enable the "Segmentation AV" mode. Once you did set everything you need for your analysis. Click on "execute". 

After you did so, the green circle icon should become red. It means the application is doing all the necessary calculations to give an analysis as a result. Once the circle icon becomes green again, more windows should appear. For our case this will give the result of the pulse wave analysis.

Here's an example of result you can expect: 


The application itself is made to remove any uncomplete cycles. Which means that all the time you will be able to get a complete cycle analysis.

We'll see in the next chapter the explanations of the analysis results.

Input and Output explanations
-----------------------------

As you can see, the cycle is regular, it goes up and down like in the example below:



When the cycle goes up, it stands for a diastole. As a reminder, a diastole is a rhythmically recurrent expansion especially : the relaxation and dilation of the chambers of the heart and especially the ventricles during which they fill with blood. And when the cycle goes down, it stands for a systole. As a reminder, a systole is a rhythmically recurrent contraction especially : the contraction of the heart by which the blood is forced out of the chambers and into the aorta and pulmonary artery.

Further explanations will be added soon!



.. autosummary::
   :toctree: generated

   lumache

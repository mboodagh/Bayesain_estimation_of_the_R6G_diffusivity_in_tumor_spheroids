# Bayesain_estimation_of_the_R6G_diffusivity_in_tumor_spheroids

# General description
In this work, the goal is to determine the diffusion coefficient of the rhodamine 6G (R6G) diffusing in breast tumor spheroids. The notebook file **"Diffusion_in_spheroids"** provides a detailed description of how the attenuation and diffusion coefficients are obtained. The sample images are in a zip file named **"spheroid_images.zip".**
Here, we demonstrate how the mass diffusivity and light attenuation coefficients can be accurately determined for R6G as a mock drug diffusing into cancer spheroids using the Bayesian approach and MCMC sampling method. Specifically, we determine the diffusivity and attenuation coefficients by sampling from the posterior using the No-U-Turn Sampler. The sampling is conducted using PyMC3 in Python. 

# Motivation
Cancer is one of the leading causes of death worldwide, with breast cancer (BC) being the most commonly diagnosed and the second most common cause of cancer mortality in women. A plethora of techniques have been developed to study cancer progression in vitro. Among these methods, cellular spheroidsare are widely used, as they are a simple yet robust 3D models. There is a myriad of studies that have numerically studied drug delivery as a forward problem both in nano/micro-scale through the discretization of the continuum diffusion equation. However, determining the diffusion coefficient of drug based on the given concentration field is considered to be an inverse problem, which has not gained as much attention. Inverse problems are often ill-posed and difficult to directly solve. Accordingly, many methods have been developed to regularize this ill-posedness. Among the regularization methods, the Bayesian approach not only estimates the unknown variable of interest, but it provides the uncertainty of estimation. The concentration field is given for different times in the form of images taken via a confocal microscope, and it is required to find the diffusivity. It is also important to note that as light penetrates in media, its intensity decreases exponentially with depth, according to Beer-Lambert law.  Therefore, the attenuation coefficient is another unknown, which should be determined alongside the diffusion coefficient.



# Method
## Confocal microscopy
Diffusion of R6G into spheroids was imaged on an LSM 880 confocal microscope. Z-stack images were taken of the spheroid using a 10x objective (100x final magnification) at 10 minute intervals for 4-6 hours as show in the figure below.

![spheroid_images (2)](https://user-images.githubusercontent.com/60017299/143723639-7c903926-9106-4868-8efd-ee12921b57e0.png)
 In the above figure, a,b and c correspond to images taken at 0, 100, and 200 min after the diffusion was initialized.


## Mathematical model

It is valid to assume that the concentration C stands for the light intensity (see our paper). Having considered an exponential light attenuation, the measured concentration can be modeled by modified diffusion equation. The variables of interest are the diffusion coefficient and the attenuation coef-ficient. In defining the variables, it is fruitful to modify them such that theyreflect certain constraints like remaining positive or negative. In the currentwork, to make sure that the coefficients remain positive, we use a logarithmicfunction to define a new vector of variables. If there is not any prior information available regarding the parameter of interest, uninformative priors, such as simply beginning with a uniform distribution can be used as a starting point. However, informative priors can be used when there is sufficient prior knowledge about the parameter of interest. Having an informative prior typically increases the efficiency of the model by reducing the number of iterations required to converge on the correct distribution. In the current work, a normal prior, which is considered to be an informative prior is used as shown in figure below.

![priorlog](https://user-images.githubusercontent.com/60017299/143724101-7fffeccc-ba7e-4416-9b0c-72b2e930878f.png)

Finding the posterior is the most challenging part in Bayesian inference since the the partition function is not generally analytically available, except for some few cases. A common method to circumvent the difficulties associated with the direct calculation of the partition function is to use MCMC to sample from the posterior. In the current work, we use the No-U-Turn Sampler (NUTS) of PyMC3 to sample from the posterior. The open-source probabilistic programming package, PyMC3, was utilized to perform the Bayesian analysis, which is available for free use in Python. We use two methods to find the attenuation and diffusion coefficients. First, the maximum aposteriori estimate and then the mean of posterior distributions.

# Results

The same prior distribution was assumed for all spheroids. The posterior plots for the diffusion coefficients of different spheroids are shown in the below figure.

![D_dis](https://user-images.githubusercontent.com/60017299/143724220-8a48413d-1528-446f-a444-2a690c555d33.png)

According to these plots, although some distributions are relatively different for different spheroids, they can still be represented in a close range. The same distributions can be plotted for the attanuation coefficient as shown below. The distributions have almost the same standard deviations; however, with different means.

![a_dis](https://user-images.githubusercontent.com/60017299/143724335-ff8dad85-1b3f-45aa-9602-0e8c25fa575e.png)

The posterior predictions for the values of the concentration were also calculated using PyMC3 and the mean values are shown in the below figure. It is evident that for some spheroids there is a great match between model prediction and experimental data, while in others the model cannot strongly capture the data due to the noisy nature of the pixel intensities. However, for all of these models, we assigned a rather large standard deviation in the prior such that all the data and prediction lie in the 95% confidence interval.

![post_pre_1](https://user-images.githubusercontent.com/60017299/143724619-553d612f-f76d-43c8-9c92-ca1dd3c51d0c.png)

# Conclusion
In this study, MCMC was used to sample from the posterior and the mean values with the associated uncertainties were obtained. We obtained diffusion coefficients in the range of 0.3-0.6 um^2/s for each spheroid. These values are consistent with previously reported values for spheroids, implying the effectiveness of Bayesian analysis in solving inverse problems with noisy data. These values also showed that the diffusion coefficient of R6G was irrespective of TG2 level, but did differ between BC and noncancerous spheroids as well as between the nonmetastatic parental cells and the corresponding bone metastasis cells from the same BC progression series. This methodology can be applied to a range of cell types and fluorescent markers to compute diffusion rates of spheroids and other cell mass as long as the concentration of dye of interest in the media is known.
 
 



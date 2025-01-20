---
layout: distill
permalink: /raw2logit/
title: Dataset Drift Controls Using Raw Image Data and Differentiable ISPs
description: From Raw to Logit
nav: false
nav-order: b
date: 2021-10-15

authors:
  - name: Luis Oala<span>&#42;</span>
    url: "https://luisoala.net/"
    affiliations:
      name: Fraunhofer HHI
  - name: Marco Aversa<span>&#42;</span>
    url: "https://aiaudit.org/raw2logit/"
    affiliations:
      name: University of Glasgow
  - name: Kurt Willis
    url: "https://aiaudit.org/raw2logit/"
    affiliations:
      name: Fraunhofer HHI
  - name: Gabriel Nobis
    url: "https://aiaudit.org/raw2logit/"
    affiliations:
      name: Fraunhofer HHI
  - name: Yoan Neuenschwander
    url: "https://aiaudit.org/raw2logit/"
    affiliations:
      name: HEPIA/HES-SO
  - name: Michèle Buck
    url: "https://aiaudit.org/raw2logit/"
    affiliations:
      name: Klinikum rechts der Isar
  - name: Christian Matek
    url: "https://aiaudit.org/raw2logit/"
    affiliations:
      name: Helmholtz Zentrum Munich
  - name: Jérôme Extermann
    url: "https://aiaudit.org/raw2logit/"
    affiliations:
      name: HEPIA/HES-SO
  - name: Enrico Pomarico
    url: "https://aiaudit.org/raw2logit/"
    affiliations:
      name: HEPIA/HES-SO
  - name: Wojciech Samek
    url: "http://iphome.hhi.de/samek/"
    affiliations:
      name: Fraunhofer HHI
  - name: Roderick Murray-Smith
    url: "https://aiaudit.org/raw2logit/"
    affiliations:
      name: University of Glasgow
  - name: Christoph Clausen
    url: "https://aiaudit.org/raw2logit/"
    affiliations:
      name: Dotphoton AG
  - name: Bruno Sanguinetti
    url: "https://aiaudit.org/raw2logit/"
    affiliations:
      name: Dotphoton AG
  - name: <span>&#42;</span>Equal contribution
    url: ""
    affiliations:
      name:

bibliography: lens2logit.bib

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }

---
**Code** [![CODE](https://img.shields.io/static/v1.svg?label=github&message=raw2logit&color=blue)](https://github.com/aiaudit-org/raw2logit) **Data** [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.5235536.svg)](https://doi.org/10.5281/zenodo.5235536) **Experiment server** [![CODE](https://img.shields.io/static/v1.svg?label=mlflow&message=raw2logit&color=blue)](http://deplo-mlflo-1ssxo94f973sj-890390d809901dbf.elb.eu-central-1.amazonaws.com/#/)
<!---<d-cite key="gregor2015draw"></d-cite>--->

<!--- *This is an interactive companion to and resource collection for the manuscript ["From Lens to Logit: Addressing Camera Hardware-Drift Using Raw Sensor Data"](https://openreview.net/forum?id=DRAywM1BhU), submitted to the NeurIPS 2021 Datasets and Benchmarks Track.* --->

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/lens2logit/pmflow8.png" data-zoomable>
    </div>
</div>
<br>

To create an image, raw sensor data traverses complex image signal processing pipelines. These pipelines are used by cameras and scientific instruments to produce the images fed into machine learning systems. The processing pipelines vary by device, influencing the resulting image statistics and ultimately contributing to what is known as hardware-drift. However, this processing is rarely considered in machine learning modelling, because available benchmark data sets are generally not in raw format. Here we show that pairing qualified raw sensor data with an explicit, differentiable model of the image processing pipeline allows to tackle camera hardware-drift. Specifically, we demonstrate (1) the controlled synthesis of hardware-drift test cases, (2) modular hardware-drift forensics, as well as (3) image processing customization. We make available two data sets. The first, Raw-Microscopy, contains 940 raw bright-field microscopy images of human blood smear slides for leukocyte classification alongside 5,640 variations measured at six different intensities and twelve additional sets totalling 11,280 images of the raw sensor data processed through different pipelines. The second, Raw-Drone, contains 548 raw drone camera images for car segmentation, alongside 3,288 variations measured at six different intensities and also twelve additional sets totalling 6,576 images of the raw sensor data processed through different pipelines.

<!---## Preliminaries: How an image is made
The outputs of image acquisition systems is traditionally aligned with human visual perception \cite{palmer1999vision, rowlands2017physics}. However, the raw sensor image (RI) obtained from a camera before processing differs substantially from the processed RGB image that we usually have in mind when thinking of images. The RI appears like a grey scale image with a grid structure (see $\boldsymbol{x}_{raw}$ in the figure at the top of the page). This grid is given by the Bayer color filter mosaic, which lies over sensors \cite{bayer1976color}. Instead, the final RGB image that we, and likewise most machine learning models, usually receive % consume
is the result of a series of transformations applied to the RI. For every transformation, there are different possible algorithms, which can be linear or non-linear, local or non-local, reversible or irreversible. Starting from a single RI, all those possible combinations can generate an exponential number of possible images that are slightly different in terms of colors, lighting and blur - variations that contribute to hardware drift.
%
In \Cref{fig:pmflow} we show a conventional pipeline from the RI to the final RGB image. Here, we consider the most common and core transformations. \todo{TODO: the following sentence is not clear}%Also, in order to highlight the impact of the image processing in the neural network training, we selected a set of na$\text{\"i}$ve representations of the whole image processing. 
%For further information about each transformations see Appendix \todo{Add Appendix transformations}. 
We use $\state{\F}{i}$ to denote the $i^{th}$ transformation and $\state{\view}{i}$ (\textit{view}) for the output image of $\state{\F}{i}$.
%
The first step of the pipeline is the \textit{black level} correction $\state{\F}{\acrshort{bl}}$, which removes any constant offset. The image $\state{\view}{\acrshort{bl}}$ is a grey image with a Bayer filter pattern. A \textit{demosaicing} algorithm $\state{\F}{\acrshort{dm}}$ is applied to construct the full RGB color image \cite{li2008image}.
Given $\state{\view}{\acrshort{dm}}$, intensities are adjusted to obtain a neutrally illuminated image $\state{\view}{\acrshort{wb}}$ through a \textit{white balance} transformation $\state{\F}{\acrshort{wb}}$. By considering color dependencies, a \textit{color correction} transformation $\state{\F}{\acrshort{cc}}$ is applied to balance hue and saturation of the image.
Once lighting and colors are corrected, a \textit{sharpening} algorithm $\state{\F}{\acrshort{sh}}$ is applied to reduce image blurriness. This transformation lets the image appear more noisy. For this reason a \textit{denoising} algorithm  $\state{\F}{\acrshort{dn}}$ is applied afterwards \cite{goyal2020image,tian2020deep}. Finally,  \textit{gamma correction},  $\state{\F}{\acrshort{gc}}$, adjusts the linearity of the pixel values. For a precise description of these transformations, see \ref{subsubsec:the_static_pipeline}.
--->

In order to address camera hardware-drift we require two ingredients: **raw sensor data** and an **image processing model**. In the following we explain how the Raw-Microscopy and Raw-Drone datasets were collected for this study and how the processing model is built.

## Raw data
### Raw-Microscopy
Assessment of blood smears under a light microscope is a key diagnostic technique <d-cite key="Bain2005"></d-cite>. The creation of image datasets and machnine learning models on them has received wide interest in recent years <d-cite key="Scotti2011"></d-cite><d-cite key="Matek2019"></d-cite><d-cite key="Ayyappan2020"></d-cite>. Here, we use a bright-field microscope to image blood smear cytopathology samples. The light source is a halogen lamp equipped with a 0.55 NA condenser, and a pre-centred field diaphragm unit. We use filters at 450 nm, 525 nm and 620 nm to acquire the blue, green and red channels respectively. The condenser is followed by a 40 $$\times$$ objective with 0.95 NA (Olympus UPLXAPO40X). Slides can be moved via a piezo with 1 nm spatial resolution, in the three directions. We focus by maximizing the variance of the pixel values. Images are acquired is 16 bit, with a 2560 $$\times$$ 2160 pixels CMOS sensor (PCO edge 5.5). We measured the PSF to be 450 nm with 100 nm nanospheres. Mechanical drift was measured at 0.4 pixels per hour. Imaging was performed on de-identified human blood smear slides (Ma190c Lieder, J. Lieder GmbH & Co. KG, Ludwigsburg/Germany). All slides were taken from healthy humans without known hematologic pathology. Imaging regions were selected to contain single leukoytes in order to allow unique labelling of image patches, and regions were cropped to 256 $$\times$$ 256 pixels. All images were annotated by a trained hematological cytologist using the standard scheme of normal leukocytes comprising band and segmented neutrophils, typical and atypical lymphocytes, monocytes, eosinophils and basophils <d-cite key="longanbach2016rodak"></d-cite>. To soften class imbalance, candidates for rare normal leukocyte types were preferentially imaged, and enrich rare classes. Additionally, two classes for debris and smudge cells, as well as cells of unclear morphology were included. Labelling took place for all imaged cells from a particular smear at a time, with single-cell patches shown in random order. RI are generated using JetRaw Data Suite features. Blue, red and green channels are metrologically rescaled independently in intensity to simulate a standard RGB camera condition. From each channel, some pixels are discarded complementary on each channel in order to obtain a Bayer filter pattern.
### Raw-Drone
We used a DJI Mavic 2 Pro Drone, equipped with a Hasselblad L1D-20c camera (Sony IMX183 sensor) having 2.4 $\micro$m pixels in Bayer filter array. The objective has a focal length of 10.3 mm. We set the f-number $$N=8$$, to emulate the PSF circle diameter relative to the pixel pitch and ground sampling distance (GSD) as would be found on images from high-resolution satellites. The point-spread function (PSF) was measured to have a circle diameter of 12.5$\micro$m. This corresponds to a diffraction-limited system, within the uncertainty dominated by the wavelength spread of the image. Images were taken at 200 ISO, a gain of 0.528 DN/$e^-$. The 12-bit pixel values are however left-justified to 16-bits, so that the gain on the 16-bit numbers is 8.448 DN/$e^-$. The images were taken at a height of 250 m, so that the GSD is 6 cm. All images were tiled in 256  $$\times$$ 256 patches. Segmentation color masks were created to identify cars for each patch. From this mask, classification labels were generated to detect if there is a car in the image. The dataset is constituted by 548 images for the segmentation task, and 930 for classification. The dataset is augmented through JetRaw Data Suite, with 7 different intensity scales.

The second ingredient to our experiments is the **image processing model** which we describe next.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/lens2logit/ABpipelines_Microscopy.png" data-zoomable>
    </div>
</div>
<div class="caption">
    Static processing variations on the Raw Microscopy dataset.
</div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/lens2logit/ABpipelines_Drone.png" data-zoomable>
    </div>
</div>
<div class="caption">
     Static processing variations on the Raw Drone dataset.
</div>

## Raw2Logit - The image processing framework
Let $(X,Y):\Omega \to \mathbb{R}^{H,W}\times \mathcal{Y}$ be the raw sensor data generating random variable on some probability space $(\Omega, \mathcal{F},\mathbb{P})$, with $\mathcal{Y}=\{0,1\}^{K}$ for classification and $\mathcal{Y}=\{0,1\}^{H,W}$ for segmentation. Let $\Phi_{Task}:\mathbb{R}^{C,H,W}\to\mathcal{Y}$ be the task model determined during training. The inputs that are given to the task model $\state{\F}{Task}$ are the outputs of the image signal processing (ISP). We distinguish between the raw sensor image $\boldsymbol{x}$ and a *view* $\boldsymbol{v}=\state{\F}{Proc}(\boldsymbol{x})$ of this image, where $\state{\F}{Proc}\colon\R^{H,W}\to\R^{C,H,W}$ models the ISP. In contrast to the classical setting, this approach is more sensitive to the origin of distribution shifts, as outlined in our [formal companion](https://github.com/aiaudit-org/website/blob/master/assets/pdf/lens2logit-formalcompanion.pdf). We provide two explicit models for ISP: a static model $\Phi^{stat}\_{Proc}$ and a parametrized model $\Phi^{\theta}\_{Proc}$. 

### The static pipeline
Following the most common steps in ISP, we define the *static pipeline* as the composition
$$
\begin{equation*}
     \Phi^{stat}_{Proc} := \Phi_{GC} \circ \Phi_{DN}  \circ \Phi_{SH} \circ \Phi_{CC} \circ \Phi_{WB} \circ \Phi_{DM} \circ \Phi_{BL},
\end{equation*}
$$
mapping a raw sensor image to a RGB image. The static pipeline enables us to create (multiple) different views of the same raw sensor data by manually changing the configurations of the intermediate steps. Fixing the continuous features, but varying $\Phi_{DM}$, $\Phi_{SH}$ and $\Phi_{DN}$ results in the twelve different views visible further down in this post. 
For a detailed description of the static pipeline and its intermediate steps we refer to our [formal companion](https://github.com/aiaudit-org/website/blob/master/assets/pdf/lens2logit-formalcompanion.pdf). 

<iframe src="https://hf.space/gradioiframe/luisoala/raw2logit/+" onload='javascript:(function(o){o.style.height=o.contentWindow.document.body.scrollHeight+"px";}(this));' style="height:1200px;width:100%;border:none;overflow:hidden;"></iframe>

### The parametrized pipeline
For a fixed raw sensor image, the *parametrized pipeline* $\Phi^{\theta}\_{Proc}$ maps from a parameter space $\Theta$ to a RGB image. The parametrized pipeline is differentiable wrt. the parameters in $\boldsymbol{\theta}$. This enables us to backpropagate the gradient from the output of the task model through the ISP back to the raw sensor image. You can find more details in our [formal companion](https://github.com/aiaudit-org/website/blob/master/assets/pdf/lens2logit-formalcompanion.pdf).

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/lens2logit/blood-medium.gif" data-zoomable>
    </div>
</div>
<div class="caption">
     Gradients (top row) and generated views (bottom row) over time for the parametrized pipeline in order (demosaicing, color correction, sharpening, denoising, gamma correction) on the Raw-Microscopy dataset with a downstream ResNet task model. Note that compression of the video files for faster online viewing creates artifacts. Original, uncompressed image files can be accessed through <a href="http://deplo-mlflo-1ssxo94f973sj-890390d809901dbf.elb.eu-central-1.amazonaws.com/#/experiments/60
">the virtual lab log</a>.
</div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/lens2logit/drone-medium.gif" data-zoomable>
    </div>
</div>
<div class="caption">
     Gradients (top row) and generated views (bottom row) over time for the parametrized pipeline in order (demosaicing, color correction, sharpening, denoising, gamma correction) on the Raw-Drone dataset with a downstream U-Net++ task model. Note that compression of the video files for faster online viewing creates artifacts. Original, uncompressed image files can be accessed through <a href="http://deplo-mlflo-1ssxo94f973sj-890390d809901dbf.elb.eu-central-1.amazonaws.com/#/experiments/60
">the virtual lab log</a>.
</div>


With **raw data** and and a **controllable processing pipeline** in our hands we are able to do interesting things. We can synthesize different realistic views from our raw sensor data (like the ones shown above), perform hardware-drift forensics on machine learning model as well as customued image processing. 
<!-- If you curious about these applications and our results the [full paper](https://openreview.net/forum?id=DRAywM1BhU) is for you. --->


<!---
<div class="fake-img l-screen">
  <p><iframe src="https://gradio.app/g/aiaudit-org/lens2logit" style="border:solid 0px #777" width="{{ site.max_width }}" height="800" frameborder="0" scrolling="no"></iframe></p>
</div>
--->
<!--- height="600"
## Applications
### Controlled synthesis of hardware-drift test cases
The static processing pipeline allows us to generate different views of the same dataset for hardware-drift testing. We evaluate the performance of two task models under twelve different image processing configurations:
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/lens2logit/ABpipelines_Microscopy.png" data-zoomable>
    </div>
</div>
<div class="caption">
    Static processing variations on the Raw Microscopy dataset.
</div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/lens2logit/ABpipelines_Drone.png" data-zoomable>
    </div>
</div>
<div class="caption">
     Static processing variations on the Raw Drone dataset.
</div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/lens2logit/1.png" data-zoomable>
    </div>
</div>
<div class="caption">
    Cross-validation results are provided for both blood smears and drone datasets for 12 different processing pipelines. Each point is the average metrics value, with its standard deviation, over 5 k-fold dataset splitting. (Left) Models trained on a selected image processing configuration are evaluated on a different pipeline.  To understand critical hardware-shift drops, metrics values need to be compared with reference values on the diagonal where the model is trained and tested on the same pipeline. For the microscopy case we have a drop in performance for one particular configuration (ma,s,me) while in the drone case there is a more heterogeneous pattern. (Right) Models trained on different processing pipelines are evaluated with with common corruptions benchmark at severity 3. Similar results are obtained for different hardware-shift configurations.
</div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/lens2logit/1_1.png" data-zoomable>
    </div>
</div>
<div class="caption">
    Worst case train/test pipelines are compared. In both cases, our model leaked in performances with small changes on RGB channels. (Top) For the blood smears dataset, train/test pipeline shown are respectively me, u, me and ma, s, ga. We measured a 33% metrics (IoU/Accuracy) drop respect to the training pipeline. (Bottom) For the drone dataset, the model has been trained with ma, s, ga and tested with bi, u, me giving an average 20% drop. 
</div>

### Hardware-drift forensics
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/lens2logit/2.png" data-zoomable>
    </div>
</div>
<div class="caption">
    Attained accuracy on the test set after 20 epochs of adversarial optimization of the processing pipeline for varying regularization weight parameter $\lambda$. 
    The individual plots depict the various pipeline parameter selections.
</div>
### Image processing customization
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/lens2logit/3.png" data-zoomable>
    </div>
</div>
<div class="caption">
    Low intensity images processed by a static and a learned pipeline. The plots in the rightmost column display the mean of validation metrics over five cross validation runs. Error bars are reported as one standard deviation. Optimization step 1439 and 915 correspond to epoch 60 into training.
</div>
--->
## Resources
### Data
#### Access
If you use our code you can use the convenient cloud storage integration. Data will be loaded automatically.
We also maintain a copy of the entire dataset with a persistent and permanent identifier at Zenodo which you can find under identifier [10.5281/zenodo.5235536](https://doi.org/10.5281/zenodo.5235536).
#### License
The data is published under a [Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) which allows liberal copying, redistribution, remixing and transformation.
The authors bear all responsibility for the published data.
### Code
#### Access
All code is available at the [raw2logit repository](https://github.com/aiaudit-org/raw2logit).
#### License
The code is published under [MIT license](https://choosealicense.com/licenses/mit/) which permits broad commercial use, distribution, modification and private use.
### Virtual lab log
We maintain a collaborative virtual lab log at [this address](http://deplo-mlflo-1ssxo94f973sj-890390d809901dbf.elb.eu-central-1.amazonaws.com/#/). There you can browse experiment runs, analyze results through SQL queries and download trained processing and task models.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/lens2logit/mlflow.png" data-zoomable>
    </div>
</div>

<br>

For better overview we include a map between the names of experiments in the paper and names of experiments in the virtual lab log:

| Name of experiment in paper | Name of experiment in virtual lab log |
| :------------- | -----:|
| 5.1 Controlled synthesis of hardware-drift test cases | 1 Controlled synthesis of hardware-drift test cases (Train) , 1 Controlled synthesis of hardware-drift test cases (Test)|
| 5.2 Modular hardware-drift forensics | 2 Modular hardware-drift forensics |
| 5.3 Image processing customization | 3 Image processing customization (Microscopy), 3 Image processing customization (Drone) |

Note that the virtual lab log includes many additional experiments.
 

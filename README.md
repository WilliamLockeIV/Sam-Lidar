# Sam-Lidar
This code accompanies the paper **Leveraging SAM 2 and LiDAR for Automated Individual Tree Crown Delineation: A Comparative Evaluation of Prompting Methods**, currently under review in the journal _Information Geography_.

In the paper and code, we test the efficacy of the pre-trained, promptable Segment Anything Model 2 (SAM 2) for individual tree crown delineation (ITC delineation) using realistic prompting methods. These prompting methods consist of manual and automatically-drawn bounding boxes, with the optional inclusion of point prompts extracted from LiDAR point clouds. We evaluate these methods on a diverse subset of 592 trees from the NeonTreeEvaluation dataset using a suite of object-based and pixel-based metrics. We find that the selection of bounding box prompts significantly impacts the number of trees SAM 2 correctly segments, with the best performing prompts resulting in 92% F1 for object-based metrics and the worst performing prompts resulting in 35% F1. We also find that LiDAR data slightly improves SAM 2’s delineations, increasing object-based F1 by 1-4%. Pixel-based metrics, which are calculated only for trees with accurate bounding box prompts, are consistently between 78-87% F1, showing that, given a correct prompt, SAM 2 can accurately delineate tree crowns.

![Example Delineation](visualizations/Example%20Delineation.jpg)

## Running the Code
The Jupyter notebook in this repository, [Paper Methods.ipynb](notebooks/Paper_Methods.ipynb) allows you to rerun the experiments and visualize the results. It can be run in Google Colab (preferably with GPU support and High RAM for speed increases) by pressing the "Open in Colab" button, without requiring downloading this repository or the notebook itself. If you would like to save any changes you make to the notebook, you will need to either download the .ipynb file or create a copy in Colab.

We are working on creating an environment that will allow you to run the notebook and code locally. We will include instructions here when that is available.

## Data
The data used in this paper is adapted from the NeonTreeEvaluation dataset (Weinstein et al., 2021). We took 18 RGB images from this dataset, with accompanying LiDAR point clouds and hand-drawn bounding boxes around individual trees, and supplemented them with our own hand-drawn polygons more precisely delineating each individual tree crown. This subset of images with our manual delineations can be downloaded at Zenodo (https://doi.org/10.5281/zenodo.15354456).

## Methods
As described in the paper in greater detail, we use Meta's SAM 2 model to segment tree crowns from bounding box and LiDAR prompts. We do this by first taking the RGB image of a plot of trees, as below.

![Three example tree plots with low, medium, and high tree density](visualizations/Tree%20Plots.jpg)

We then add prompts to indicate the individual tree crowns for SAM 2 to delineate. One type of prompt is bounding boxes, which can be drawn by hand or via automatic algorithm around individual trees. In the paper, we use one set of hand-drawn boxes and two sets of automatically drawn boxes, the latter two using models DeepForest (Weinstein et al., 2020) and Detectree2 (Ball et al., 2023). 

![Bounding box prompts drawn by DeepForest](visualizations/Bounding%20Boxes.jpg)

We can then supplement these bounding box prompts with LiDAR data, which allows us to both filter out incorrectly placed bounding boxes and provide specific point prompts to SAM 2 to further refine its delineations.

![LiDAR point prompts](visualizations/LiDAR%20Points.jpg)

Finally, we feed the images and prompts to SAM 2, which outputs delineations for each individual tree crown as identified by bounding box and LiDAR prompts.

![SAM 2 delineations](visualizations/ITC%20Delineations.jpg)

To see more examples and read about the accuracy of these different prompting methods, please read the full paper and run the associated [Colab Notebook](notebooks/Paper_Methods.ipynb).

## References

Ball, J. G. C., Hickman, S. H. M., Jackson, T. D., Koay, X. J., Hirst, J., Jay, W., Archer, M., Aubry‐Kientz, M., Vincent, G., & Coomes, D. A. (2023). Accurate delineation of individual tree crowns in tropical forests from aerial RGB imagery using Mask R‐CNN. Remote Sensing in Ecology and Conservation, 9(5), 641–655. https://doi.org/10.1002/rse2.332

Weinstein, B. G., Graves, S. J., Marconi, S., Singh, A., Zare, A., Stewart, D., Bohlman, S. A., & White, E. P. (2021). A benchmark dataset for canopy crown detection and delineation in co-registered airborne RGB, LiDAR and hyperspectral imagery from the National Ecological Observation Network. PLOS Computational Biology, 17(7), e1009180. https://doi.org/10.1371/journal.pcbi.1009180

Weinstein, B. G., Marconi, S., Aubry‐Kientz, M., Vincent, G., Senyondo, H., & White, E. P. (2020). DeepForest: A Python package for RGB deep learning tree crown delineation. Methods in Ecology and Evolution, 11(12), 1743–1751. https://doi.org/10.1111/2041-210x.13472
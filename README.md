# pop-de
Population inference via density estimation.
A small Python library (pop-de) for estimating a probability density from data samples using Kernel Density Estimation (KDE), built for multi-dimensional problems like gravitational-wave population inference.

The core idea in three layers:
The base classes (density_estimate.py) fit a standard KDE — you give it your data points and it estimates a smooth density. SimpleKernelDensityEstimation uses scipy; VariableBwKDEPy uses KDEpy and, importantly, allows a different bandwidth for every data point.

On top of that, AdaptiveBwKDE (adaptive_kde.py) makes the KDE adaptive: it first fits a rough "pilot" density, then gives each point a bandwidth that's narrow in dense regions and wide in sparse regions, so it captures sharp peaks and smooth tails at the same time. A single parameter alpha controls how strongly it adapts (alpha=0 = ordinary fixed-bandwidth KDE).

The optimization classes then tune the free parameters automatically via cross-validation — holding out part of the data, scoring how well the KDE predicts it, and picking the bandwidth/alpha (or per-dimension scale factors) that score best.

The supporting piece (transform_utils.py) lets you transform each dimension before fitting — take logs (good for positive/skewed quantities), standardize, or rescale — and correctly maps the density back to the original units afterward, so different dimensions with wildly different scales all get treated fairly.

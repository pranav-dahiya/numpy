# Machine Learning needs Better Randomness Standards: Randomised Smoothing and PRNG-based attacks

This repository is the accompanying artifact to the paper titled __Machine Learning needs Better Randomness Standards: Randomised Smoothing and PRNG-based attacks__, accepted for publication at the USENIX Security Symposium '24, which can be found [here](https://sec24summerae.usenix.hotcrp.com/doc/sec24summerae-paper24-accepted_paper_pdf_format.pdf).

## Abstract

Randomness supports many critical functions in the field of machine learning (ML) including optimisation, data selection, privacy, and security. ML systems outsource the task of generating or harvesting randomness to the compiler, the cloud service provider or elsewhere in the toolchain. Yet there is a long history of attackers exploiting poor randomness, or even creating it – as when the NSA put backdoors in random number generators to break cryptography. In this paper we consider whether attackers can compromise an ML system using only the randomness on which they commonly rely. We focus our effort on Randomised Smoothing, a popular approach to train certifiably robust models, and to certify specific input datapoints of an arbitrary model. We choose Randomised Smoothing since it is used for both security and safety – to counteract adversarial examples and quantify uncertainty respectively. Under the hood, it relies on sampling Gaussian noise to explore the volume around a data point to certify that a model is not vulnerable to adversarial examples. We demonstrate an entirely novel attack, where an attacker backdoors the supplied randomness to falsely certify either an overestimate or an underestimate of robustness for up to 81 times. This backdoor was introduced in the numpy python library, which is implemented in this artifact. Using this code we demonstrated that the randomness tests suggested by NIST fail to detect the change in the PCG64 PRNG under some circumstances. We advocate updating the NIST guidelines on random number testing to make them more appropriate for safety-critical and security-critical machine-learning applications.

## Usage

To use this library, first create a Python virtual environment. Then build `numpy` according to the instructions [here](README_numpy.md). The backdoor relies on a text file, `attack.txt` in the current working directory, which can be used to set the parameters $\alpha$, $\beta$ and $\gamma$ as defined in the paper. 

To test whether the 

To run experiments with this version of `numpy` on randomised smoothing as shown in the paper:

1. Clone [`smoothing`](https://github.com/locuslab/smoothing).
2. In [`core.py`](https://github.com/locuslab/smoothing/blob/master/code/core.py), replace the usage of `PyTorch` in the method `_sample_noise` to `numpy`.
3. Sampling from the normal distribution using `numpy` can be done as follows.
```Python
rng = np.random.default_rng()
noise = torch.from_numpy(rng.normal(0.0, sigma, size=batch.size())).float().to('cuda')
```
4. Create a file `attack.txt` in the current working directory with three lines corresponding to each of the parameters $\alpha$, $\beta$ and $\gamma$ respectively. For example, to run with $\beta = 1$, the contents of `attack.txt` will look like:
```
-1
1
-1
```
5. Install the required packages for `smoothing` and use it as instructed in their [README](https://github.com/locuslab/smoothing/blob/master/README.md).

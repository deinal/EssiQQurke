# EssiQQurke
Tutorial scripts for 2022 Arnold Sommerfeld School Physics meets Artificial Intelligence

## Overview and Goal

The aim of this tutorial is to show you how to train a Restricted Boltzman Machine to reconstruct the wavefunction of an array of Rydberg atoms whose exact state is unknown. We will show an example with an array of 8 atoms. Each of the 8 atoms can be in the up state (1 state) or down state (0 state). 

## The Training Data

Nwow $\Omega$ and $\delta$ are $2$ parameters of the Rydberg Hamiltonian (https://arxiv.org/pdf/2107.00766.pdf). In this example, we fix $\Omega = 1.0$. Then for each $\delta$ in $\delta \in \{1.00, 1.02, 1.04, 1.06, 1.08, 1.10, 1.12, 1.14, 1.16, 1.18, 1.20\}$, we obtain $10,000$ measurements of the state of the $8$ atoms. Thus there are $11$ training datasets for each delta and each training dataset has $10,000$ rows (for the $10,000$ measurements) and $8$ columns as there are 8 Rydberg atoms in the array. The training data file for each $\delta$ is given in data\nY=8 directory in the _samples.csv files.

## Training the RBM
The RBM is trained using QuCumber. To train we need to specify the number of visible nodes and some hyperparameters including the number of hidden nodes. As we have an array of $8$ atoms, our inputs will have 8 elements. Hence there are $8$ visible nodes. The number of hidden nodes and the other hyperparameters can be varied for each $\delta$ to obtain better solutions. More information about the hyperparameters is given in the relevant section of the notebook. 

## Checking accuracy of Model
After training is done, we get weights and biases of our trained RBM. Recall that we do not know the actual wavefunction. Therefore we cannot compute fidelity, which is a standard matric to evaluate the performance of a model. However, from the original measurements one point function and two point function have been calculated. (Note: one point function and two point functions are described in detail in the relevant section of the notebook). Now from the trained RBM (for a particular $\delta$) we generate num_samples = $50,000$ samples. From those samples we calculate the one point and two point function data. It can be seen that the data we calculate for the one point and two point function are close to the data in the _1_pt_fn.csv and _2_pt_fn.csv files for a particular $\delta$. We use these 2 methods to check how well the distributiton of the samples generated by the RBM resembles the distribution of the measurement.
## Directory Structure
Rydberg_Reconstruction.ipynb is the main notebook which contains the wavefunction reconstruction example for an array of 8 atoms. 

data/ directory contains the training data for different systems which are labelled as 'nY= number of atoms in the array'. Now 'data/nY = num_of_atoms_in_array' directoru contains 3 sets of files for each $\delta$ which are the one point function, two point function and the samples.

output/ directory contains some of the outputs produced by the notebook which are:
1. reconstructedSample.txt : This contains the new sampled measurement data produced by the trained RBM.
2. reconstructedStateAmplitude.txt : This contains the amplitudes of the reconstructed quantum state in the computational basis. 
2. rydberg_data.pt : contains the parameters of the trained RBM.
## Remarks
In this tutorial we trained an RBM using measurement data and then sampled from the trained RBM to generate more data. The generated data was used to reconstruct the unknown wavefunction. Lastly note that sampling of higher order systems and amplitude computation may take some time and thus it may be wise to generate smaller samples of data $(\approx 1000)$ at the cost of accuracy.
# Heterogenous-HMM (HMM with labels)


This repository contains different implementations of the Hidden Markov Model with just some basic Python dependencies are used. The main contributions of this libraries with respect to other available APIs are:

- **Heterogeneous HMM (HMM with labels)**: here we implement this version of the HMM which allow us to manage each of the observations with different probability distributions (*also, the simpler cases: Gaussian and Discrete/multinomial HMMs are implemented*).

- **Missing values support**: our implementation supports both partial and complete missing data per observation.

- **Semi-Supervised HMM (Fixing discrete emission probabilities)**: in the Heterogenous-HMM model, it is possible to fix the emission probabilities of the discrete features: the model allow us to fix the complete B matrix of certain feature or just the emission probabilities of some of the states of one feature (while training the emission probabilities of the other states).

- **Model selection criteria**: both BIC and AIC are implemented.

Also, some others aspects like having multiple sequences or several features per observation are supported. This model is easily extendable with other types of probablistic models.



## Available models:

> Several implementations of the HMM have been developed, all these HMM models extend the *_BaseHMM* class.

- Multinomial HMMs (Discrete HMM): Hidden Markov Model with multinomial (discrete) emission probabilities.
- Gaussian HMMs: Hidden Markov Model with Gaussian emission probabilities.
- Heterogeneous HMM (HMM with labels): Hidden Markov Model with mixed discrete and gaussian emission probabilities.
- Semi-supervised HMM: in the Heterogeneous HMM, it is possible to fix the emission probabilities of the discrete features to guide the learning process of the model. *[An example can be found in the "hmm_tutorials.ipynb" notebook]*.

Now, a more detailed explanation of each of them is provided:





### 1. Multinomial HMM.

In the multinomial HMM, the emission probabilities are discrete, whetever it is binary or categorical.

**Parameters:**

- *n_states* (int) - the number of hidden states
- *n_emissions* (int) - the number of distinct observations
- *n_features* (list) - a list containing the number of different symbols for each emission
- *params* (string, optional) - controls which parameters are updated in the
training process; defaults to all parameters
- *init_params* (string, optional) - controls which parameters are initialised
prior to training; defaults to all parameters
- *init_type* (string, optional) - name of the initialisation
method to use for initialising the model parameters before training
- *pi_prior* (array, optional) - array of shape (n_states, ) setting the
parameters of the Dirichlet prior distribution for 'pi'
- *A_prior* (array, optional) - array of shape (n_states, n_states),
giving the parameters of the Dirichlet prior distribution for each
row of the transition probabilities 'A'
- *learn_rate* (float, optional) - a value from the (0,1) interval, controlling how much
the past values of the model parameters count when computing the new
model parameters during training; defaults to 0
- *missing* (int or NaN, optional) - a value indicating what character indicates a missed
observation in the observation sequences; defaults to NaN
- *verbose* (bool, optional) - flag to be set to True if per-iteration
convergence reports should be printed during training

### 2. Gaussian HMM.

In the Gaussian HMM, the emission probabilities are managed with gaussian probabilities distributions.

**Parameters:**

- *n_states* (int) - the number of hidden states
- *n_emissions* (int) - the number of distinct Gaussian observations
- *params* (string, optional) - controls which parameters are updated in the training process; defaults to all parameters
- *init_params* (string, optional) - controls which parameters are initialised prior to training; defaults to all parameters
- *init_type* (string, optional) - name of the initialisation method to use for initialising the model parameters before training; can be "random" or "kmeans"
- *covariance_type* (string, optional) - string describing the type of covariance parameters to use.  Must be one of: "diagonal", "full", "spherical" or "tied"; defaults to "diagonal"
- *pi_prior* (array, optional) - array of shape (n_states, ) setting the parameters of the Dirichlet prior distribution for 'pi'
- *A_prior* (array, optional) - array of shape (n_states, n_states), giving the parameters of the Dirichlet prior distribution for each row of the transition probabilities 'A'
- *means_prior, means_weight* (array, optional) - arrays of shape (n_states, 1) providing the mean and precision of the Normal prior distribution for the means
- *covars_prior, covars_weight* (array, optional) - shape (n_states, 1), provides the parameters of the prior distribution for the covariance matrix
- *min_covar* (float, optional)- floor on the diagonal of the covariance matrix to prevent overfitting. Defaults to 1e-3.
- *learn_rate* (float, optional) - a value from the $[0,1)$ interval, controlling how much the past values of the model parameters count when computing the new model parameters during training; defaults to 0
- *verbose* (bool, optional) - flag to be set to True if per-iteration convergence reports should be printed during training

### 3. Heterogeneous HMM.

In the Heterogeneous HMM, we can manage some of the features' emission probabilities with gaussian distributions and others with discrete distributions.

**Parameters:** 

The HeterogeneousHMM class uses the following arguments for initialisation:
- *n_states* (int) - the number of hidden states.
- *n_g_emissions* (int) - the number of distinct Gaussian observations.
- *n_d_emissions* (int) - the number of distinct discrete observations.
- *n_d_features* (list - list of the number of possible observable symbols for each discrete emission.
- *params* (string, optional) - controls which parameters are updated in the training process; defaults to all parameters.
- *init_params* (string, optional) - controls which parameters are initialised prior to training; defaults to all parameters.
- *init_type* (string, optional) - name of the initialisation method to use for initialising the model parameters before training; can be "random" or "kmeans".
- *nr_no_train_de* (int) - this number indicates the number of discrete emissions whose Matrix Emission Probabilities are fixed and are not trained; it is important to to order the observed variables such that the ones whose emissions aren't trained are the last ones. 
- *state_no_train_de* (int) - a state index for nr_no_train_de which shouldn't be updated; defaults to None, which means that the entire emission probability matrix for that discrete emission will be kept unchanged during training, otherwise the last state_no_train_de states won't be updated
- *covariance_type* (string, optional) - string describing the type of covariance parameters to use.  Must be one of: "diagonal", "full", "spherical" or "tied"; defaults to "diagonal".
- *pi_prior* (array, optional) - array of shape (n_states, ) setting the parameters of the Dirichlet prior distribution for 'pi'.
- *A_prior* (array, optional) - array of shape (n_states, n_states), giving the parameters of the Dirichlet prior distribution for each row of the transition probabilities 'A'.
- *means_prior, means_weight* (array, optional) - arrays of shape (n_states, 1) providing the mean and precision of the Normal prior distribution for the means.
- *covars_prior, covars_weight* (array, optional) - shape (n_states, 1), provides the parameters of the prior distribution for the covariance matrix.
- *min_covar* (float, optional)- floor on the diagonal of the covariance matrix to prevent overfitting. Defaults to 1e-3.
- *learn_rate* (float, optional) - a value from the $[0,1)$ interval, controlling how much the past values of the model parameters count when computing the new model parameters during training; defaults to 0.
- *verbose* (bool, optional) - flag to be set to True if per-iteration convergence reports should be printed during training.

### 4. Semi-supervised HMM.

Using the HeterogenousHMM it is possible to fix the emission probabilities of the discrete features by using the *'nr_no_train_de'" variable, which indicates the number of features we don´t want to be trainned by the model but the keep fixed to an original value set by the user and the *'variablestate_no_train_de'*, that can be used to fix just some of the states of that specific feature while training the emission probabilities of the others. **An example to clarify this can be found on the "hmm_tutorials.ipynb" notebook**.

## Folder Structure.

- /src: it contains all the classes that implement the models.
- /notebooks: it contains the "hmm_tutorials.ipynb" notebook, which contains an example code to use each of the available models.
- /test: it contains the testing files for each of the HMM models.

## Dependencies. 

The required dependencies are specified in *requirements.txt".

## Authors.

The current project has been developed by:

- Fernando Moreno Pino (http://www.tsc.uc3m.es/~fmoreno/, https://github.com/fmorenopino).
- Emese Sukei (https://github.com/semese).

## Acknowledgments.

- Advanced Signal Processing Course, by Prof. Dr. Antonio Artés-Rodríguez at Universidad Carlos III de Madrid.
-L. R. Rabiner, "A tutorial on hidden Markov models and selected applications in speech recognition," in Proceedings of the IEEE, vol. 77, no. 2, pp. 257-286, Feb. 1989.
- K.P. Murphy, "Machine Learning: A Probabilistic Perspective", The MIT Press ©2012, ISBN:0262018020 9780262018029
- O.Capp, E.Moulines, T.Ryden, "Inference in Hidden Markov Models", Springer Publishing Company, Incorporated, 2010, ISBN:1441923195

This model has been based in previous implementations:

- https://github.com/guyz/HMM
- https://github.com/hmmlearn

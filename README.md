# 💬 Automatic Speech Recognition of Isolated Digits using Hidden Markov Models

## 📌 Overview

This project was developed for the course Speech Science and Technology and implements an Automatic Speech Recognition (ASR) system capable of recognizing isolated spoken digits in portuguese from 0 to 9 using Hidden Markov Models (HMMs) and MFCC features.
The system was trained and evaluated using a custom-recorded dataset and achieved 98% accuracy on the validation set.

I also made a video explaining the concepts and the project (video in portuguese): https://www.youtube.com/watch?v=-1re2LmgcIA

## 🎤 Dataset

To build the dataset:

* Digits 0 through 9 were recorded

* 20 audio samples per digit were collected

* Total: 200 .wav files

* Sampling rate: 16 kHz

* Quantization: 16-bit

* Speaker: single speaker (my own voice)

### 🚗 Recording Environment

All recordings were performed inside a car to ensure more controlled acoustics, reduced background noise and lower reverberation. This helped improve signal quality and consistency across samples.

## 🧹 Preprocessing

### 📢 Audio Edition

Each audio file was manually edited in Audacity to:

* Remove silence at the beginning and end of the recording

* Preserve only the spoken portion

* No additional filtering or noise reduction was applied.

### ✂️ Data Split

For each digit:

* 15 samples were used for training

* 5 samples were used for validation

This resulted in:

* 150 training samples

* 50 test samples

An example of all spoken 'nine' is shown below. In this case, all signals are overposed.

![nine.png](/Images/nine.png)

## 🧠 Model Architecture

### 🔢 One HMM per Digit

A separate Hidden Markov Model was trained for each digit (0–9), resulting in 10 independent HMMs. This approach is standard for isolated-word recognition because: each digit has its own unique temporal and spectral structure, an HMM models the sequential evolution of acoustic features over time and training one model per digit allows each HMM to learn the specific temporal dynamics of that digit. 

During inference, the input signal is evaluated against all 10 models, and the digit corresponding to the highest likelihood is selected.

Formally:
<h3 align="center">
d̂ = argmax_d P(X | λ_d)
</h3>


Where:
* X = sequence of MFCC feature vectors
* λ_d = HMM trained for digit 

## ⚙️ Feature Extraction

### 🔪 Framing and Windowing

Each audio signal was segmented into frames using:

* Hanning window

* Window length: 10 ms

* Frame shift: 5 ms

This overlapping window approach ensures smooth temporal analysis of speech.

### 📊 MFCC Extraction

For each frame, 13 Mel-Frequency Cepstral Coefficients (MFCCs) were extracted.

MFCCs are widely used in speech recognition because they:

* Represent the short-term power spectrum of speech

* Use a Mel-scale filterbank to approximate human auditory perception

* Apply a logarithm and Discrete Cosine Transform (DCT) to decorrelate features

In summary, MFCCs provide a compact and perceptually meaningful representation of speech that works well with probabilistic models such as HMMs.

## 📘 Training

The models were implemented using the Python library:

`hmmlearn`: [https://hmmlearn.readthedocs.io/](https://hmmlearn.readthedocs.io/)

Each digit model was trained using the Baum–Welch algorithm (Expectation-Maximization), which estimates:

* State transition probabilities

* Emission probability distributions

* Initial state probabilities

## ☑️ Validation Procedure

For each validation audio file:

* MFCC features were extracted

* The log-likelihood of the feature sequence was computed for each of the 10 HMMs

* The digit corresponding to the highest likelihood was selected as the predicted label

The classifier therefore performs maximum likelihood classification.

## 📈 Results

* Total test samples: 50

* Correct classifications: 49

* Misclassifications: 1

* Accuracy: 98%

  ### Confusion Matrix

| True \ Pred | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|-------------|---|---|---|---|---|---|---|---|---|---|
| 0 | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 2 | 0 | 0 | 4 | 0 | 0 | 0 | 1 | 0 | 0 | 0 |
| 3 | 0 | 0 | 0 | 5 | 0 | 0 | 0 | 0 | 0 | 0 |
| 4 | 0 | 0 | 0 | 0 | 5 | 0 | 0 | 0 | 0 | 0 |
| 5 | 0 | 0 | 0 | 0 | 0 | 5 | 0 | 0 | 0 | 0 |
| 6 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 0 | 0 | 0 |
| 7 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 0 | 0 |
| 8 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 0 |
| 9 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 |


The single error appears in the confusion matrix at row 3 and column 7. This indicates one instance of digit 2 being classified as digit 6. The high accuracy demonstrates the effectiveness of HMMs in modeling the temporal structure of speech signals in controlled environments.

## 🔍 Discussion

The results highlight why Hidden Markov Models were historically dominant in speech recognition:

* They explicitly model temporal structure

* They handle variable-length sequences naturally

* They provide a probabilistic framework for classification

However, HMM-based systems also have limitations, since they rely on strong independence assumptions. Other limitation is that they require careful feature engineering (e.g., MFCCs) and their performance may degrade with speaker variability and noisy environments.

These limitations motivated the development of hybrid systems, such as:

* HMM + Deep Neural Networks (DNN-HMM)

* End-to-end deep learning models

Such approaches combine probabilistic temporal modeling with the representational power of deep networks, leading to state-of-the-art performance in modern ASR systems.

## ⛓️ Limitations

This system was trained using a single-speaker dataset, which limits its ability to generalize to different voices, accents, and speaking styles. Additionally, all recordings were collected in a controlled, low-noise environment, meaning performance may degrade under real-world acoustic conditions.

The dataset size is relatively small, reducing variability and robustness. Furthermore, the system is restricted to isolated digit recognition and does not handle continuous speech or larger vocabularies.

These constraints define the scope of the project and highlight opportunities for future improvements.

Future improvements could include:

* Multi-speaker dataset

* Noise robustness testing

* Inclusion of delta and delta-delta MFCCs

* Comparison with neural network models

## 💭 Conclusion

This project demonstrates a complete classical speech recognition pipeline:

* Data acquisition

* Signal preprocessing

* Feature extraction (MFCC)

* Probabilistic modeling (HMM)

* Maximum likelihood classification

* Performance evaluation

The system achieved 98% accuracy on isolated digit recognition, validating the effectiveness of HMM-based acoustic modeling in controlled conditions.
This repository has the final project in Speech Recognition of Speech Science and Technology graduated course, attended at Federal University of Minas Gerais. 




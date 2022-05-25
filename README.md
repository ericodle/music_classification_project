<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/github_username/repo_name">
    <img src="https://github.com/ericodle/Genre-Classification-Using-LSTM/blob/main/GRU_CM.jpg" alt="Logo" width="400" height="350">
  </a>

<h3 align="center">Music Genre Classification Using Neural Networks</h3>

  <p align="center">
  In this project, we explore various artifical neural network (ANN) approaches to achieve near-human accuracy in a music genre classification task. By converting raw .wav audio input into an array of MFCC values, we are able to achieve our best result (90.7% accuracy) using a Gated Recurrent Unit (GRU) model written in PyTorch. This repository serves as a source of supplementary material for a music genre classification conference paper currently under review. We herein archive our Python scripts and provide a sample Google Colab notebook for the reference of anyone interested.
    <br />
    <br />
    <a href="https://github.com/github_username/repo_name/issues">Report Bug</a>
    ·
    <a href="https://github.com/github_username/repo_name/issues">Request Feature</a>
  </p>
</div>


<!-- ABOUT THE PROJECT -->
## About this Project

This was my first project with Professor Rebecca Lin (Feng Chia University, Taiwan) during the Taiwan Experience Exchange Program. Classifying music genre was my initial experience both in writing/training ANNs from scratch as well as in audio signal analysis, and I had a lot of catching up to do. Through a wealth of online resources, particularly the MFCC tutorials provided by [Valerio Velardo](https://github.com/musikalkemist), we were able to train models with decent generaliation and test classification accuracy. 

Our results are based on the [GTZAN](http://marsyas.info/index.html) music genre dataset, which provides 10 human-classified genre folders: blues, classical, country, disco, hip-hop, jazz, metal, pop, reggae, and rock. Each genre folder contains 100 30-second audio clips of genre-specific songs in .wav format. Following previous work in this field, we extracted [Mel-frequency cepstral coefficients](https://en.wikipedia.org/wiki/Mel-frequency_cepstrum), or MFCCs, from each audio clip, divided the entire shuffled set into an 80:10:10 train/validation/test split, and played around with multi-layer perceptron, convolutional, and recurrent networks/hyperparamteters until we got a model that achieved at least 90% accuracy.

We hope this project inspires you to contribute to our project, incorporate our tools, and play around with ANN models yourself! 


<p align="right">(<a href="#top">back to top</a>)</p>


## Prerequisites

> __For this example, the working directory is the repository root directory.__ 

### Install dependencies

- Using pip

  ```sh
  # Install dependencies
  pip install -r dependencies.txt
  ```

### Download GTZAN and extract MFCCs

> The training/testing music used in this project comes from the GTZAN music genre dataset, which can be downloaded [here](https://www.kaggle.com/datasets/andradaolteanu/gtzan-dataset-music-genre-classification/download) from Kaggle. 
> You will need to enter some account login details per Kaggle's requirements before the download can begin. 
> Then, you must manually relocate the full dataset (parent folder containing 10 genre sub-folders, each with 100 music clips) into the "GTZAN_dataset" project folder.

```sh
# This script will extract MFCC's from each song clip and output the entire dataset as a genre-labeled JSON file.
./MFCC_extraction.py
```
> Note that the resulting JSON file is saved in the "MFCCs" folder as a JSON file about 640 MB in size.
> 
## Scripts

We provide several shell scripts for easy managing the experiments. (See
[here](scripts/README.md) for a detailed documentation.)

> __Below we assume the working directory is the repository root.__

### Train a new model

1. Run the following command to set up a new experiment with default settings.

   ```sh
   # Set up a new experiment
   ./scripts/setup_exp.sh "./exp/my_experiment/" "Some notes on my experiment"
   ```

2. Modify the configuration and model parameter files for experimental settings.

3. You can either train the model:

     ```sh
     # Train the model
     ./scripts/run_train.sh "./exp/my_experiment/" "0"
     ```

   or run the experiment (training + inference + interpolation):

     ```sh
     # Run the experiment
     ./scripts/run_exp.sh "./exp/my_experiment/" "0"
     ```

### Collect training data

Run the following command to collect training data from MIDI files.

  ```sh
  # Collect training data
  ./scripts/collect_data.sh "./midi_dir/" "data/train.npy"
  ```

### Use pretrained models

1. Download pretrained models

   ```sh
   # Download the pretrained models
   ./scripts/download_models.sh
   ```

   You can also download the pretrained models manually
   ([pretrained_models.tar.gz](https://docs.google.com/uc?export=download&id=19RYAbj_utCDMpU7PurkjsH4e_Vy8H-Uy)).

2. You can either perform inference from a trained model:

   ```sh
   # Run inference from a pretrained model
   ./scripts/run_inference.sh "./exp/default/" "0"
   ```

   or perform interpolation from a trained model:

   ```sh
   # Run interpolation from a pretrained model
   ./scripts/run_interpolation.sh "./exp/default/" "0"
   ```

## Outputs

By default, samples will be generated alongside the training. You can disable
this behavior by setting `save_samples_steps` to zero in the configuration file
(`config.yaml`). The generated will be stored in the following three formats by
default.

- `.npy`: raw numpy arrays
- `.png`: image files
- `.npz`: multitrack pianoroll files that can be loaded by the
  _[Pypianoroll](https://salu133445.github.io/pypianoroll/index.html)_
  package

You can disable saving in a specific format by setting `save_array_samples`,
`save_image_samples` and `save_pianoroll_samples` to `False`  in the
configuration file.

The generated pianorolls are stored in .npz format to save space and processing
time. You can use the following code to write them into MIDI files.

```python
from pypianoroll import Multitrack

m = Multitrack('./test.npz')
m.write('./test.mid')


## Repository Files

- blues.00000.wav

This .wav file is provided as a test music clip. The file itself was taken from the same download of the GTZAN dataset as used throughout this project, and the excerpt is from the classic John Lee Hooker song "One Bourbon, One Scotch, One Beer." It is classified in GTZAN as Blues.

- [ ] MFCC_primer.ipynb

This Jupyter Notebook with direct link to a ready-to-use Google Colab environment is intended to answer the questions "What is an MFCC?" and "How did we get our MFCCs?"

- [ ] MFCC_extraction.py

This script extracts MFCCs from the GTZAN dataset music files and saves them in JSON format. The resulting file is about 640 MB in size, and contains an ordered list of 13 MFCC values per segment of each song within the dataset. Moreover, this data is labeled with values 0 through 9 corresponding to one of the ten genres present.

- [ ] network_train_and_test.py

coming soon!

- [ ] models.py

coming soon!

- [ ] live_runs

This folder contains a collection of Jupyter Notebooks (linked to Google Colab) that were saved and uploaded to GitHub immediately after running. In their currently saved state, they serve as a record of the experimental runs on which we base our results. Of course, users are welcome to play around with these scripts and try to beat our top test accuracy of 90.7%!

- [ ] GRU_CM

We herein provide a representative graphic for this README file and, by extension, project. This CM (confusion matrix) obtained upon testing our GRU model serves the additional function of showing our best achieved performance.

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- CONTRIBUTING -->
## Contributing

Contributions make the open source community great. Everyone has a unique combination of skills and experience. Your input is **highly valued**.
If you have ideas for improvement, please fork the repo and create a pull request. 
If this is your first pull request, just follow the steps below:

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- LICENSE -->
## License

Distributed under the GNU Lesser General Public License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#top">back to top</a>)</p>





<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/github_username/repo_name.svg?style=for-the-badge
[contributors-url]: https://github.com/github_username/repo_name/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/github_username/repo_name.svg?style=for-the-badge
[forks-url]: https://github.com/github_username/repo_name/network/members
[stars-shield]: https://img.shields.io/github/stars/github_username/repo_name.svg?style=for-the-badge
[stars-url]: https://github.com/github_username/repo_name/stargazers
[issues-shield]: https://img.shields.io/github/issues/github_username/repo_name.svg?style=for-the-badge
[issues-url]: https://github.com/github_username/repo_name/issues
[license-shield]: https://img.shields.io/github/license/github_username/repo_name.svg?style=for-the-badge
[license-url]: https://github.com/github_username/repo_name/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/linkedin_username
[product-screenshot]: images/screenshot.png

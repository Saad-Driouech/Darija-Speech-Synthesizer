# Darija-Speech-Synthesizer
A deep learning-based speech synthesizer for Moroccan Darija  

Before starting the preprocessing pipeline, you may want to first filter by speake. To do so, run the notebook: _FilteringSpeaker.ipynb_. The output is a directory containing the audios of the chosen speaker and a file containing transcriptions (in our case, Loubna was the chosen speaker).  

Now you well need to convert mp3 files to wav files. To do so run the notebook: _MP3ToWav.ipynb_.  

## Steps to implement the preprocessing pipeline

1- Start by removing emojis and punctuation marks  
2- Diacritize the text: To do so, you will need the API provided by QCRI  
3- Perform transliteration  

The three previous steps are incorporated in the preprocessing notebook: _preprocessing_darija.ipynb_  

4- Perform phonetization by running the notebook _phonetizer.ipynb_.  
**Be careful, the cell where to stop at this stage is indicated in the notebook**  
The output of the phonetizer consists of two files: pronunciation dictionary _dict_ and pronunciation of each utterance _utterance_pronunciation_. We will need dict as input for the aligner.  
**NB: I modified scripts of the original phonetizer and addedd new scripts. You can download the phonetizer scripts with my modification from [here](https://drive.google.com/drive/folders/1Ryw8GHCD0FS0B33hEywm6UhZpKOqyj1O?usp=sharing)**.

5- Perform alignment  
You will need to install HTK on a linux machine. [Steps to install HTK on Ubuntu](https://gist.github.com/laic/39b8b2e156c39c778888aa825aee9877).  
After successful installation of HTK, you will need to install prosodylab aligner to use HTK from this [github repo](https://github.com/nawarhalabi/Prosodylab-Aligner). you will need to create a folder for your data which will contain the lab and wav files (each lab file contains transcript of one audio).  
Now, you will also need the dictionary as output by the aligner. The next step is to run the following command:  
`python3 -m aligner -r lang-mod.zip -a data/ -d lang.dict` where:  
_lang-mod.zip_ is the HMM language model (in our case we used a model built for arabic). It is available in the prosodylab aligner github repo and is named _bootstrap.zip_.  
_data_ a folder containing lab and wav files  
_lang.dict_ the dictionary  
The aligner mah flag any formatting errors in the dictionary. These must be corrected manually. Once no more errors are available, on the next run, the aligner will return a list of words out of dictionary (OOV). This list must be fed to the phonetizer and run the before last cell in the notebook _phonetizer.ipynb_. Append the output _phonetizedOOV.txt_ to the corrected dictionary and run the last cell of the phonetizer notebook. The ouput will given as input to the aligner (you will probably need to perform corrections again)    
After going through all this and correcting the newly augmented dictionary, the aligner should start aligning the spoken utterances to their transcipts. This step may take an hour to complete depending on the size of the dataset.

Now, all the preprocessing pipeline has been followed. Before starting training, we will need to filter the wav and lab files to keep only the one that have their corresponding textgrid files (for some reason the aligner ignores some files when aligning). To do so, run the notebook _filter lab wav textgrid.ipynb_.

## Training FastSpeech2

To start training, you will need to run the notebook _Training_FastSpeech2.ipynb_.  
The scripts needed for training can be downloaded from [here](https://drive.google.com/drive/folders/1pK3FbMlXuTGC0cseaPdP7xqxuKxA_Jv6?usp=sharing).  
Once the training is done, you can move to synthesis.

## Synthesizing speech

To synthesize speech you just need to run the synthesis notebook _Inference_FastSpeech2.ipynb_.  
The scripts needed to run the notebook can be downloaded from [here](https://drive.google.com/drive/folders/1UM22IgwB_LU4b3BK9InDyuNoiHRITJ2D?usp=sharing). I made changes and fixed bugs in the original repo which can be found [here](https://github.com/ARBML/klaam). That is why I am sharing the one with my modifications.

If all the previous steps were completed with no issues, you should get your audio as output.

Do not hesitate to contact me if you face any problems. It will probably save you some time as I probably would have gone through the bug.

# Darija-Speech-Synthesizer
A deep learning-based speech synthesizer for Moroccan Darija

Before starting the preprocessing pipeline, you may want to first filter by speake. To do so, run the notebook: _FilteringSpeaker.ipynb_. The output is a directory containing the audios of the chosen speaker and a file containing transcriptions (in our case, Loubna was the chosen speaker).

Now you well need to convert mp3 files to wav files. To do so run the notebook: _MP3ToWav.ipynb_.

## Steps to implement the preprocessing pipeline

1- Start by removing emojis and punctuation marks
2- Diacritize the text: To do so, you will need an API key from QCRI (Owners of the diacritizer)
3- Perform transliteration

The three previous steps are incorporated in the preprocessing notebook: _preprocessing_darija.ipynb_

4- Perform phonetization by running the notebook 

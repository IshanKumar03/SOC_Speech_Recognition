The code initializes empty lists paths and labels.
 It walks through the directory C:\Users\ishan\Downloads\dataverse_files using os.walk(). This function iterates through each file and subdirectory within the specified root directory.
For each file found (filenames), it appends the full file path to paths and extracts the label from the filename:
It splits the filename by underscore ('_'), taking the last part (filename.split('_')[-1]).
Then, it removes the file extension by splitting on the period ('.') and taking the first part (label.split('.')[0]).
Finally, it converts the label to lowercase (label.lower()) and appends it to the labels list.
Then it stores the recordings of different emotions stored in paths to df['speech'] and labels to df['label']
The first reading is a .txt file so that datapoint is dropped as it inteferes while training the model
waveshow(data, sr, emotion):
This function takes audio data (data), the sampling rate (sr), and the emotion label (emotion) as inputs.
It plots the waveform of the audio data using librosa.display.waveshow.
The plot is displayed with a title showing the emotion label.
plt.show() is called to display the plot.
spectogram(data, sr, emotion):
This function takes the same inputs as waveshow.
It computes the Short-Time Fourier Transform (STFT) of the audio data using librosa.stft(data).
The amplitude of the STFT is converted to decibels using librosa.amplitude_to_db(abs(x)).
It then plots the spectrogram using librosa.display.specshow with time on the x-axis and frequency (in Hz) on the y-axis.
A color bar is added to indicate the amplitude levels.
plt.show() is called to display the plot.
The code sets the emotion of interest to 'angry'.
It retrieves the file path of the first audio file labeled with this emotion from the DataFrame df.
The audio file is loaded using librosa.load, which returns the audio data and its sampling rate.
The waveform and spectrogram of the audio data are plotted using the waveshow and spectogram functions, respectively.
The audio file is then played using IPython.display.Audio.
extract_mfcc(filename): This function takes a filename (path to an audio file) as input.
Feature Extraction:
Loading Audio: librosa.load(filename, duration=3, offset=0.5) loads the audio file specified by filename.
duration=3 specifies the duration of audio to load (3 seconds).
offset=0.5 specifies the start time (offset) from which to load the audio (0.5 seconds).
This means the function loads 3 seconds of audio starting from the 0.5-second mark.
Computing MFCCs: librosa.feature.mfcc(y=y, sr=sr, n_mfcc=40) computes MFCCs from the loaded audio.
y is the audio time series.
sr is the sampling rate of y.
n_mfcc=40 specifies the number of MFCCs to compute (40 coefficients).
Averaging MFCCs: np.mean(..., axis=0) calculates the mean of MFCCs across time.
librosa.feature.mfcc(...).T transposes the MFCC matrix.
np.mean(..., axis=0) computes the mean along the columns (axis=0), resulting in a 1D array of averaged MFCC values.
MFCC is Mel-Frequency Cepstral Coefficients.
Mel Scale Conversion: MFCCs are derived by first converting the frequency scale of the audio spectrum from linear (Hz) to the mel scale, which is more aligned with human auditory perception.
Logarithmic Compression: After mel scale conversion, the power spectrum is converted to a logarithm to mimic the logarithmic perception of loudness by humans.
 MFCCs reduce the dimensionality of the feature space while retaining critical information about the spectral envelope of the audio signal and are widely used in speech processing
 The mfcc features are stored in Xmfcc of dataframe df
 df[['label']] extracts the 'label' column from the DataFrame df. This column contains categorical labels (e.g., classes or categories).
enc.fit_transform(...) fits the encoder (enc) to the data (determines categories) and transforms the categorical labels into one-hot encoded vectors.
Then a LSTM(long short term memory model) is built using suitable activation functions to deploy deep learning for speech emotion recognition
The training and testing data is divided in a ratio of 4:1 and the model is trained on the training data with Xmfcc and y as input.Training loss cam out to be 0.0102 and testind dataset loss to be 0.0459

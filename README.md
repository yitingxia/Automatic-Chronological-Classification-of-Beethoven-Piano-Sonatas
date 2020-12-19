# Automatic Chronological Classification of Beethoven's Piano Sonatas

Beethoven’s piano sonatas can be classified into early works, middle works and late works according to their composition periods. The process of using deep learning networks to complete the chronological classification task are as follows:
- 1. Pre-processing: convert the sonata MIDI files into natural language sequences
- 2. mLSTM (multiplicative Long Short Term Memory) Model: generate a characteristic vector (named C-vector with 4096 elements) of each sample
- 3. Softmax Regression: classify each C-vector
The predicted labels of the Softmax regression are the results of this classification task.

## Pre-processing
Similar data representation scheme in [12] is adopted to convert the sonatas' MIDI files into natural language sequences in sequential order. The representation scheme is listed as follows:
- (1)“n_[pitch]” : pitch is an integer between 0 and 127 (inclusive), namely, pitch=1, 2,...,127.
- (2)“d_[duration]_[dot]” : duration can be breve, whole, half, quarter, eighth, sixteenth, and thirty-second notes; dot can be 0, 1, 2, 3.
- (3)“v_[velocity]” : velocity is a multiple of 4 between 4 and 128 (inclusive), namely, velocity=4, 8,..., 128.
- (4)“t_[tempo]” : tempo (BPM) is a multiple of 4 between 24 and 160 (inclusive), namely, tempo=24, 28,..., 160.
- (5)“.” : The end of time step. The length of each time step is the same as that of a sixteenth note.
- (6)“\n” : The end of the music segment.
For example, the first measure from Beethoven Piano Sonata No.9 in E Major (Op.14 No.1) movement 2, as illustrated in Fig. 1, can be transformed into the natural language sequence as follows:
> t_80 v_64 d_quarter_1 n_47 v_64 d_half_1 n_50 v_64 d_half_1 n_54 v_64 d_quarter_1 n_59 . . . . . . v_64 d_eighth_0 n_46 v_64 d_eighth_0 n_58 . . v_64 d_quarter_0 n_47 v_64 d_quarter_0 n_59 . . . .

## mLSTM Model
After pre-processing, MIDI files were converted into natural language sequences. Feeded by the character in the sequence of the current time step, the mLSTM model is trained to predict the character of the next time step. This is the training process of the mLSTM model. 
It is notable that once the the model is well-trained, by passing a whole music sample (here it is a sequence of characters) into the trained mLSTM model, the vector involved in the final predicting process (the final hidden state) can be assumed to have the characteristics of this music sample (I will explain this in the following sections). Let's call this vector the "Characteristic-vector", namely, the "C-vector".

## Softmax Regression
Beethoven piano sonatas are converted into natural language sequences during the pre-processiong process. Feeding into the trained mLSTM model above, several C-vectors are gained. Then these C-vectors are classified by Softmax regression. The results are showed as follows.
We can see that the proposed mLSTM model with Softmax regression can achieve the classification accuracy of 90% in average, which indicates that mLSTM model is effective in feature extraction, and the C-vectors do contain music characteristics. So what exactly is in the C-vector? I didn't figure this out, but we can have a closer look.

## Analysis

We calculate the average vector of the C-vectors generated from samples of each period (by averaging the elements of the corresponding positions in the vectors), and plot the histogram of the average C-vectors in blue, as shown in Fig. 6 below. The corresponding probably density curve of each average vector by kernel density estimation is also drawn, shown as orange solid lines. 

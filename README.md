# A fully recurrent feature extraction for speech enhancement
This is a Tensorflow implementation of the ```SE-FFTNet``` architecture suggested in <a href="https://www.isca-speech.org/archive/Interspeech_2019/pdfs/2622.pdf"> this paper</a>, where the model has a wider dilation patter at the beginning which then decreases over the layers. 
This new dilation pattern helps to learn bettern representations of the speech and noise characteristics, and have shown better quality enhancement over the SEGAN <a href="https://arxiv.org/abs/1703.09452">[1]</a> and SE-WaveNet <a href="https://arxiv.org/abs/1706.07162">[2]</a>, while faster to train and test.<br>

Few samples from the trained model are displayed <a href="https://www.csd.uoc.gr/~shifaspv/IS2019-demo">here</a>, along with the other models.

## Brief description of the model architecture
![1 1](https://user-images.githubusercontent.com/33422097/84161101-9708fc00-aa77-11ea-9b55-573f05b6bd81.jpg)
As can be seen in the figure, the ```SE-FFTNet``` is of noncausal type that takes the noisy speech segment(<b> [x<sub>t-r</sub>, ...., x<sub>t+r</sub>] </b>) as input, with <b> r</b> is the size of receptieve filed, to generate the current sample prediction in time domain. The model doesn't have any recursion, the loss function are computed with taking the absolute difference between the predicted and target samples, refer to <a href="https://www.isca-speech.org/archive/Interspeech_2019/pdfs/2622.pdf">Eq.(2)</a> in the paper.

## Implemented On
Python - 3.6.8 <br>
Tensorflow - 1.14.0 <br>

## Data set
The model displayed was trained on the Noisy speech database for training speech enhancement algorithms (NSDTSEA), which is publically available at <a href="https://datashare.is.ed.ac.uk/handle/10283/1942">here</a>.

Extract the data into ```./data/NSDTSEA``` directory.
## Description of the configuration file variables
<table>
  <tr>
    <th>"Name"</th>
    <th>"Discription"</th>
  </tr>
  
  <tr>
    <th>train_id_list:</th>
      <td>list of training wave files ID</td>
  </tr>
    <tr>
    <th>valid_id_list:</th>
      <td>list of validation files ID</td>
  </tr>
  <tr>
    <th>n_channels</th>
    <td>number of channels in each layer</td>
  </tr>
<tr>
    <th>dilations</th>
    <td>dilation rate starting from the begining layer</td>
  </tr>
  <tr>
    <th>target_length</th>
      <td> total samples generated in a single forward epoch</td>
  </tr>
    <tr>
    <th>filter_length</th>
    <td>convolutiona filter width: 3 for non-causal architecture </td>
  </tr>
  <tr>
    <th>Regarin</th>
      <td>The level to which wave files are RMS normalised </td>
  </tr>
</table>

The **train_id_list** and **valid_id_list** are generated by ```./data/generate_wave_id_list.py``` file.
## Training the model

Go to the ```./src``` folder and run the ```train.py``` or copy the command below to command line 

```
python train.py
```

Optionally, you can resume the training that could not have been completed, by passing the second argument ```model_id```

```
python train.py --model_id=saved_model_id
```

Trained models are saved to the ```./saved_models``` directory

## Testing the model

You may either use the already trained model which is in the ```./saved_models```, or could have your own trained model.

Go to the ```./src``` folder, and compile the ```generate.sh``` file with first argument as the ```model_id```. 

```
./generate.sh saved_model_id
```

A new folder named ```./outputs/saved_model_id``` will be created and saved the output sample.
User can manually edit the wave file ID inside the ```generate.sh```, to generate over multiple files.



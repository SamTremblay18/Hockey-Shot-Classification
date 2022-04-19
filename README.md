# Hockey Shot Classification
Google Collab Pro+ was used for this project.

To run this code, you will need to gain access to the Google drive containing all the data and the following notebooks.

In order to properly understand this project, open the following notebooks to follow along with the content of this ReadMe. 

This project has been divided into multiple notebooks:
- Participants_dictionary.ipynb
- Preprocessing.ipynb
- CNN_Method1_ALL.ipynb (Reframe to 576, all sensors configuration)
- CNN_Method2_HANDS.ipynb (Reframe to 576, hands sensor configuration)

## 1 - Participants' dictionary
This notebook is very simple and is not optimized by any means. It has been created before I had better knowledge of python and I did not spent more time on it since it takes approximately 5 hours to open all the excel sheets.

- Functions
  - The first function loads a participant's excel file containing the sensor free acceleration sheet and the markers (label) sheet.
  - The next two functions are to save and open pickle files, which are very useful as it stores and opens data very quickly

- Opening participants' dataset and saving to pickle
  - This is the section that would need a function to run all this chunk of code instead of manually inputing the participants dataset.

- Creating the participants' and markers' dictionaries
  - Again, manually opening the participants from pickle and then creating their own dictionary for easier in the future.

- To get a better view of the datasets, here's an example of Participant 1's sensor free acceleration data in the participants dictionnary:
  - The first column is the frame at which data has been collected (240 Hz = 240 frames per second). The next 51 columns are the free sensor acceleration data for the 17 Xsens sensors, in the X,Y and Z-axis (17 x 3 = 51). The last column represent the label of the task being performed at the specific frame.
<img width="1383" alt="Screen Shot 2022-03-29 at 1 38 36 PM" src="https://user-images.githubusercontent.com/83588457/160672310-83af7b7a-63aa-435f-a150-ae389726fdca.png">


- To help manipulate these enormous datasets, a markers' dictionary has been created with more information about each task being performed for a specific participant: 
  - The first column represent the frame number at which the trial is starting. The second is the label at which the trial starts. This label will remain the same for the amount of frames written in the thrid column; trial length. These three columns will be crucial in the preprocessing steps later.
<img width="329" alt="Screen Shot 2022-03-29 at 1 39 38 PM" src="https://user-images.githubusercontent.com/83588457/160674402-c8daba95-c640-48fc-852b-7502fadaa51f.png">



## 2 - Preprocessing
This notebook is dedicated to all the preprocessing steps required before feeding the data in a CNN model. There are 2 different preprocessing method used, more explanation at their own section and the technical details are commented directly in the notebook.

- It is important to note that the objective of this preprocessing stage is to create tensors of shape [trials, frames, channels] to be a proper input for the CNNs. Each participant tensor is comprised of:
   - 10 backhand shots (labeled BH, encoded as 0)
   - 10 one-timer shots (labeled OT, encoded as 1)
   - 15 trials of other (labeled Other, encoded as 2), which are a random mix of stick-handling, pre-shot skating and post-shot skating
   - 5 passes (labeled Pass, encoded as 3)
   - 15 trials of rest (labeled Rest, encoded as 4), which was 1 single resting trial that has been split into 15 trials to fit the required tensor shape
   - 15 slap shots (labeled SS, encoded as 5)
   - 15 wrist shots (labeled WS, encoded as 6)
   - Total = 85 trials, with few participants having +/- 1-2 trials

- Functions
  - You will find the technical details about the functions commented directly in the notebook.

- Method 1: reframe to 576 and all sensors configuration
  - This approach was the first discussed and the most simple to manipulate the data. The 576 frames was determined by finding the slowest (most frames) shooting/pass trial and used as a reference. All shooting/pass trials under 576 frames were then added frames from the pre-shot, so that all trials were of 576 frames. The assumption was that the small addition of pre-shot (skating and stick-handling before the shot) was going to be negligible and would not negatively affect the model's accuracy while keeping 100% of the shooting/passing data.
  - After opening the data and the markers, a function (Preprocess_1) was created to run all the preprocessing step and it was stored into the P_processed1 dictionary.

- Method 2: reframe to 576 and hands sensor configuration (BSC)
  - This approach is the same as the first one with only one exception, it used only the two hand's sensor.
  

## 3 - FCN for both sensor configuration
- The FCN model used was inspired by hfawaz https://keras.io/examples/timeseries/timeseries_classification_from_scratch/ and originally designed by Wang et al., (2016) in https://arxiv.org/abs/1611.06455
- The hyperparameters were found via random search using KerasTuner https://keras.io/keras_tuner/




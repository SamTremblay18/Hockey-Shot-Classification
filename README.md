# Hockey Shot Classification
Google Collab Pro+ was used for this project.

To run this code, you will need to gain access to the Google drive containing all the data and the following notebooks.

In order to properly understand and run this project, it has been divided into multiple notebooks:
- Participants' dictionary
- Preprocessing
- CNN models
  - Method 1 (Reframe to 576, all sensors configuration)
  - Method 2 (Reframe to 576, biomechanics sensor configuration)
  - Method 3 (Resampling, biomechanics sensor configuration)

## 1 - Participants' dictionary
This notebook is very simple and is not optimized by any means. It has been created before I had better knowledge of python and I did not spent more time on it since it takes approximately 5 hours to open all the excel sheets. (Will be updated in the future)

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
This notebook is dedicated to all the preprocessing steps required before feeding the data in a CNN model. There are 3 different preprocessing method used, more explanation at their own section and the technical details are commented directly in the notebook.

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
  - This approach was the first discussed and the most simple to manipulate the data. The 576 frames was determined by finding the slowest (most frames) shooting/pass trial and used as a reference. All shooting/pass trials under 576 frames were then added frames from the pre-shot, so that all trials were of 576 frames. The assumption was that the small addition of pre-shot (skating and stick-handling before the shot) was going to be negligible and would not negatively affect the model's accuracy while keeping 100% of the data.
  - After opening the data and the markers, a function (Preprocess_1) was created to run all the preprocessing step and it was stored into the P_processed1 dictionary.
  - The rest of this method is not optimized as there is a lot of manipulation done by hand, but it was created early in my process of learning python. It is fairly simple as I'm doing the final manipulations (split, shuffling, swap axes) on the tensors to have the required shape.
  - Lastly, it was saved as a pickle file for future CNN input

- Method 2: reframe to 576 and biomechanics sensor configuration
  - This approach is the same as the first one with only one exception, it used the biomechanics sensor configuration. This approach was only selecting the sensors from both hands, both feet and the pelvis, as suggested in the literature (Jang et al., 2018). 
 
- Method 3: resampling and biomechanics sensor configuration
  - This method was recently designed, therefore, it still needs some optimization. The idea is to resample the trials using a signal resampling function instead of adding frames at the beginning of the trials. This means that only the data from the shot is part of the trials, instead of having some undesired signal with the shot (method 1 & 2). Also, an estimate was made to obtain the resampling number of 400 frames, so a deeper analysis is required to find the ideal resampling rate. It also uses the biomechanics sensor configuration. 

## 3 - CNN Method 1
I'm just staring to test different models' architechture. The first CNN was to compare the result with the other 2 preprocessing method. The goal is to keep on building with Chollet's method.
## 4 - CNN Method 2
Only Gab's model has been tested for comparison with other preproccesing method.
## 5 - CNN Method 3
Same as method 2

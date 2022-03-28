# Hockey Shot Classification
Google Collab Pro+ was used for this project.

You will need to have access to the Google drive containing all the data and the following notebooks in order to run the code.

In order to properly understand and run this project, it has been divided into multiple notebooks:
- Participants' dictionary
- Preprocessing
- CNN models
  - Method 1 (Reframe to 576, all sensors configuration)
  - Method 2 (Reframe to 576, biomechanics sensor configuration)
  - Method 3 (Resampling, biomechanics sensor configuration)

## 1 - Participants' dictionary
This notebook is very simple and not optimized by any means. It has been created before I had better knowledge of python and I did not spent more time on it since it takes approximately 5 hours to open all the excel sheets. (Will be updated in the future)

- Functions
  - The first function loads a participant's excel file containing the sensor free acceleration sheet and the markers (label) sheet.
  - The next two functions are to save and open pickle files, which are very useful as it stores and opens data very quickly

- Opening participants' dataset and saving to pickle
  - This is the section that would need a function to run all this chunk of code instead of manually inputing the participants dataset.

- Creating the participants' and markers' dictionaries
  - Again, manually opening the participants from pickle and then creating their own dictionary for easier in the future.

## 2 - Preprocessing
This notebook is dedicated to all the preprocessing steps required before feeding a CNN model. There are 3 different preprocessing method used, more explanation at their own section.

- Functions

## 3 - CNN Method 1

## 4 - CNN Method 2

## 5 - CNN Method 3

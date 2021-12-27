---
title: Transcribe handwriting and text with Microsoft Azure Cognitive Services Computer Vision
authors:
- Jeff Blackadar
date: 2021-12-11
reviewers:
- 
layout: lesson
difficulty: 2
---

__Draft__

## Introduction

Handwritten documents are often valuable sources for historians. However, transcribing handwitten text is time consuming. This lesson demonstrates a method to use a web-based service to transcribe handwriting, Microsoft Azure Cognitive Services Computer Vision. While far from perfect, it can be a useful tool to reduce the time required to transcribe handwriting and continues to improve. Azure's Computer Vision is a ready to use service and requires no pre-training to prepare it. The steps below will describe how to register for access, connect and transcribe handwriting in an image.

When there are several technology choices available to do optical character recognition (OCR), why would you want other one? At the time of writing, Azure Computer Vision has an advantage over other methods where it can transcribe Latin alphabet handwriting without custom training by the user.

## Prerequisites

+ A computer with Python installed. 
+ An internet connection.
+ A credit card. (There is a free tier of service. The credit card is not charged if the number of files processed is below 5000 each month.)
+ A telephone number.
+ Google Colab was used to write this lesson. If you choose to use Google Colab to program Python, a Google account is required.

## Procedure
We'll transcibe handwriting in an image by following these steps:
1. Register for a Microsoft account.
2. Create a "Computer Vision" Resource in Azure to perform transcription.
3. Store a secret Key and Endpoint to access Computer Vision from your machine.
4. Install Azure Computer Vision on your machine.
5. Transcribe handwriting in an image on a website.
6. Transcribe handwriting in an image stored on your machine.

### 1. Register for a Microsoft account
1. Go to [https://portal.azure.com](https://portal.azure.com)
2. If you have an account with Microsoft or Github, log in and skip to 2. Setup Transcription: Create a Resource in Azure, below. 
3. If you don't have an account, register by clicking "No account? _Create one!_".
4. Input your e-mail address.
5. Check your e-mail inbox for a verification code and input this into the web browser.

### 2. Create a "Computer Vision" Resource in Azure to perform transcription
1. Go to https://portal.azure.com/
2. Click + Create a resource. You will need to do this twice. The first time is to set up your payment method as noted in the steps below.

![+ Create a resource](/images/step2-2.png)

3. In the "Search services and marketplace" box, type Computer Vision and search. When the search results open, click "Create" under the heading "Computer Vision".
4. Click "Start with an Azure free trial".
5. Input a telephone number to verify your identity.
6. Input your contact information and credit card number.
7. Click + Create a resource (for the second time). This will create the instance of Computer Vision for you to use.
 
![+ Create a resource](/images/step2-2.png)

8. In the "Search services and marketplace" box, type Computer Vision and search. When the search results open, click "Create" under the heading "Computer Vision".
9. In the _Create Computer Vision_ screen, _Basics_ tab, _Project Details_ section, set these fields:
	+ _Subscription_: Azure subscription 1
	+ _Resource group_: click _Create new_
	+ For _Name_ input resource_group_transcription. Click OK.

 ![+ Resource group | Create new](/images/step2-9.png)

10. In the _Instance Details_ section:
	 + Select a Region near to you. This is where the instance is hosted.
	 + Name the instance. Choose a unique name that is unique with letters or hyphens only. Input _computer-vision-transcription-uuu_, where uuu is your initials or something unique. I used _computer-vision-transcription-jhb_.
	 + Set _Pricing tier_ to Free F0. (__Important__)
11. Read the _Responsible AI Notice_ and check the box.
12. Click _Review + create_
13. Wait for a message to say _Your deployment is complete_
14. Click _Go to resource_
15. Once we see the resource screen for _computer-vision-transcription-jhb_ we can store the keys and endpoint we'll need to access this service from your computer.

### 3. Store a secret Key and Endpoint to access Computer Vision from your machine
To use the service we need to send a Key to an Endpoint. As it says on Azure: "Do not share your keys. Store them securely– for example, using Azure Key Vault. We also recommend regenerating these keys regularly. Only one key is necessary to make an API call."

To reduce the risk of inadvertently sharing your secret key we'll store it in a separate file in a different folder from the rest of the program we're writing. This helps protect your key. For example, if you check your code into a repository like GitHub, you can avoid checking in your secret key along with your code. If you don't use GitHub, don't worry, just follow these instructions to store your key.

We'll store the key in a small JSON file. JSON has a specific format that is readable by Python and is used frequently for a variety of purposes. 
We'll open a Notebook on Google Colab and make a separate folder for your key's JSON file. You can use Python on your personal computer in a similar manner.

1. Go to: https://colab.research.google.com/ (or another Python environment of your choice, such as Anaconda.)
2. Click _New Notebook_.
3. Give the Notebook a title: "Transcribe handwriting and text with Microsoft Azure Cognitive Services.ipynb"
4. For Google Colab only, perform the steps below. (If you use a Python environment on your machine like Anaconda, you won't have to connect to a disk drive. For more information about using Anaconda, see the lesson by Quinn Dombrowski, Tassie Gniady, and David Kloster, "Introduction to Jupyter Notebooks.[^1])
+ Input this code into a cell and run it by clicking the Run Cell button with a triangle.
```
# Mount Google Drive
from google.colab import drive
drive.mount('/content/drive')
```
+ To connect to Google Drive, follow the prompts, click _Connect to Google Drive_, choose your account, read the conditions and if you agree, click _Allow_.
+ Once this is complete, The message _Mounted at /content/drive_ indicates this is complete.
+ Click the Files icon on the left to show Google Drive.
+ If necessary, click the Refresh icon.
![Google Colab](/images/step3-8.png)

#### 3.A. Save Key and Endpoint in a JSON file
1. This code below will create a file to store your Key and Endpoint so it can be used by your program. The file is JSON, a conventional format that organizes the data to make it readable by computers and humans. In your Python notebook, create a new cell. (_+ Code_ in Google Colab.) Copy the code below into your new notebook cell.
```

# DELETE this cell when done
# Run this to set up a directory and JSON file for a key and endpoint in a place separate from the program
import os
import pandas as pd

directory_config = "/content/drive/MyDrive/azure_config/" # Change this to suit your machine
if not os.path.exists(directory_config):
    os.makedirs(directory_config)
print(directory_config)

# Copy KEY 1 and Endpoint from computer-vision-transcription-jhb | Keys and Endpoint
key = 'b-f-9-7-0-8-4-8-b-7-a-6-6-8-1-9-' # Change this to your Key
endpoint = 'https://computer-vision-transcription-jhb.cognitiveservices.azure.com/' # Change this to your Endpoint

data_config = [{'COMPUTER_VISION_SUBSCRIPTION_KEY': key, 'COMPUTER_VISION_ENDPOINT': endpoint}]

# Creates Pandas DataFrame
df_config = pd.DataFrame(data_config)
path_config = os.path.join(directory_config,"cv.json")
df_config.to_json(path_config)
if(os.path.exists(path_config)):
    print("Success.",path_config, "exists.")
else:
    print("Failure.",path_config, " does not exist.")
	
```

 2. In the Azure Portal, open the Keys and Endpoint page of your computer-vision-transcription-jhb
 
 ![Keys and Endpoint](/images/step3a-3.png)
 
 3. Copy KEY 1 and paste it into the program above. In line 12, replace b-f-9-7-0-8-4-8-b-7-a-6-6-8-1-9-  with your key.
 
 ```
 key = 'b-f-9-7-0-8-4-8-b-7-a-6-6-8-1-9-' # Change this to your Key
 ```
  
 3. Copy Endpoint and paste it into the program above. In line 13,  replace https://computer-vision-transcription-jhb.cognitiveservices.azure.com/  with your endpoint.
 
 ```
 endpoint = 'https://computer-vision-transcription-jhb.cognitiveservices.azure.com/' # Change this to your Endpoint
 ```

4. Run the cell.
5. You should expect these results:
+ A print of "Success. /content/drive/MyDrive/azure_config/cv.json exists.""
+ The file cv.json has been created.
+ Open cv.json. It should look similar to this:

```
{"COMPUTER_VISION_SUBSCRIPTION_KEY":{"0":"b-f-9-7-0-8-4-8-b-7-a-6-6-8-1-9-"},"COMPUTER_VISION_ENDPOINT":{"0":"https:\/\/computer-vision-transcription-jhb.cognitiveservices.azure.com\/"}}
```

6. Delete this cell. Now that cv.json is created we no longer need this code and don't want to leave the Key here. 
7. Regenerating your keys using the button on the Keys and Endpoint page is a good way to keep keys secure. When your key changes, just edit cv.json or re-run the program above.

#### 3.B. Read Key and Endpoint from a JSON file
1. This code below will read the file you created above to store your Key and Endpoint in an environment variable so that it can be accessed by the program. Create a new cell and copy the code below into your notebook.

```
import os
import pandas as pd

directory_config = "/content/drive/MyDrive/azure_config/" # Change this to suit your machine

path_config = os.path.join(directory_config,"cv.json")

# Verify the configuration file exists
if(os.path.exists(path_config)):
    print("Success.",path_config, "exists.")
else:
    print("Failure.",path_config, "does not exist.")

# Read the JSON file into a DataFrame
df_config = pd.read_json(path_config)
#print(df_config['COMPUTER_VISION_SUBSCRIPTION_KEY'].iloc[0])
#print(df_config['COMPUTER_VISION_ENDPOINT'].iloc[0])

# Store as enivonmental variables
os.environ['COMPUTER_VISION_SUBSCRIPTION_KEY'] = df_config['COMPUTER_VISION_SUBSCRIPTION_KEY'].iloc[0]
os.environ['COMPUTER_VISION_ENDPOINT'] = (df_config['COMPUTER_VISION_ENDPOINT'].iloc[0])

# Do some basic validation
if len(os.environ['COMPUTER_VISION_SUBSCRIPTION_KEY']) == 32:
    print("Success, COMPUTER_VISION_SUBSCRIPTION_KEY is loaded.")
else:
    print("Error, The COMPUTER_VISION_SUBSCRIPTION_KEY is not the expected length, please check it.")
```

2. Run this cell.  The expected result is to see this printed:

```
Success. /content/drive/MyDrive/azure_config/cv.json exists. 
Success, COMPUTER_VISION_SUBSCRIPTION_KEY is loaded.
```

If you see error messages, check that cv.json is visible to the program and that the Key is correct.

### 4. Install Azure Computer Vision on your machine[^2]
1. Create a new cell in your notebook, paste in this code and run it. It will install what is required to connect to Azure Cognitive Services Computer Vision. You only need to do this once on your machine. If you are using Google Colab, you will need to do this once per session.
```
# Install what is required to connect to Azure Cognitive Services Computer Vision
# Run this once on your machine. If you are using Google Colab, run this once per session.
!pip install --upgrade azure-cognitiveservices-vision-computervision
```

2. Create another new cell in your notebook, paste in this code and run it. It will:
+ Import the required libraries.
+ Get your Computer Vision subscription key from your environment variable.
+ Same thing with your Endpoint.
+ Authenticate with Azure Cognitive Services.

```
# Run this once per session

# Import the required libraries
from azure.cognitiveservices.vision.computervision import ComputerVisionClient
from azure.cognitiveservices.vision.computervision.models import OperationStatusCodes
from azure.cognitiveservices.vision.computervision.models import VisualFeatureTypes
from msrest.authentication import CognitiveServicesCredentials
import sys

# Get your Computer Vision subscription key from your environment variable.
if 'COMPUTER_VISION_SUBSCRIPTION_KEY' in os.environ:
    subscription_key = os.environ['COMPUTER_VISION_SUBSCRIPTION_KEY']
else:      
    print("\nSet the COMPUTER_VISION_SUBSCRIPTION_KEY environment variable.\n**Restart your shell or IDE for changes to take effect.**")
    sys.exit()

# Get your Computer Vision endpoint from your environment variable.
if 'COMPUTER_VISION_ENDPOINT' in os.environ:
    endpoint = os.environ['COMPUTER_VISION_ENDPOINT']
else:
    print("\nSet the COMPUTER_VISION_ENDPOINT environment variable.\n**Restart your shell or IDE for changes to take effect.**")
    sys.exit()

# Authenticate with Azure Cognitive Services.
computervision_client = ComputerVisionClient(endpoint, CognitiveServicesCredentials(subscription_key))

```

### 5. Transcribe handwriting in an image on a website.

This section will allow you to transcribe handwriting of an image on a website. This requires the URL for the image. For this example, we'll use ![http://jeffblackadar.ca/captain_white_diary/page_images/td_00044_b2.jpg](http://jeffblackadar.ca/captain_white_diary/page_images/td_00044_b2.jpg). http://jeffblackadar.ca/captain_white_diary/page_images/td_00044_b2.jpg


1. Open a new cell in your notebook, paste in the code block below and run it. 

```
import time
# This section is taken directly from:
# https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/ComputerVision/ComputerVisionQuickstart.py


# <snippet_read_call>
print("===== Read File - remote =====")
# Get an image with text. Set the url of the image to transcribe.
read_image_url = "http://jeffblackadar.ca/captain_white_diary/page_images/td_00044_b2.jpg"

# Call API with URL and raw response (allows you to get the operation location). Call Azure using computervision_client with the URL.
read_response = computervision_client.read(read_image_url,  raw=True)
# </snippet_read_call>
  
# <snippet_read_response>
# Get the operation location (URL with an ID at the end) from the response
read_operation_location = read_response.headers["Operation-Location"]
# Grab the ID from the URL
operation_id = read_operation_location.split("/")[-1]

# Call the "GET" API and wait for it to retrieve the results 
while True:
    read_result = computervision_client.get_read_result(operation_id)
    if read_result.status not in ['notStarted', 'running']:
        break
        time.sleep(1)

# Print the detected text, line by line
if read_result.status == OperationStatusCodes.succeeded:
    for text_result in read_result.analyze_result.read_results:
        for line in text_result.lines:
            print(line.text)
            print(line.bounding_box)
print()

# </snippet_read_response>

```
[^3]
2. This code will:
+ Set the url of the image to transcribe
```
read_image_url = "http://jeffblackadar.ca/captain_white_diary/page_images/td_00044_b2.jpg"
```

+ Call Azure using computervision_client with the URL.
```
read_response = computervision_client.read(read_image_url,  raw=True)
```

+ Read the results line by line
+ If successful, print the text of each line as well as the coordinates of a rectangle in the image where the text is located.



### 6. Transcribe handwriting in an image stored on your machine.

This section will allow you to transcribe handwriting of an image stored on your machine. It's a lot like the above section. You must have an image saved on your computer. For this example, you can download an image and save it. Here is an example image to download: http://jeffblackadar.ca/captain_white_diary/page_images/td_00044_b2.jpg.

1. Select or create a directory for your image. If you are working on Google Colab, the working directory /content/ may be used.
2. Download an example image and save it to the directory.
3. Create a new cell in your notebook, paste in the code block below. 

```
images_folder = "/content/"

print("===== Read File - local =====")
# Set the path to the image
read_image_path = os.path.join (images_folder, "td_00044_b2.jpg")

# Open the image
read_image = open(read_image_path, "rb")

  
# Call API with image and raw response (allows you to get the operation location). Call Azure using computervision_client with the image.
read_response = computervision_client.read_in_stream(read_image, raw=True)

# Get the operation location (URL with ID as last appendage)
read_operation_location = read_response.headers["Operation-Location"]

# Take the ID off and use to get results
operation_id = read_operation_location.split("/")[-1]

# Call the "GET" API and wait for the retrieval of the results

while True:
    read_result = computervision_client.get_read_result(operation_id)
    if read_result.status.lower () not in ['notstarted', 'running']:
        break
        print ('Waiting for result...')
        time.sleep(10)

# Print results, line by line

if read_result.status == OperationStatusCodes.succeeded:
    for text_result in read_result.analyze_result.read_results:
        for line in text_result.lines:
            print(line.text)
            print(line.bounding_box)
print()
```
[^3]

4. The code will set the path to the image and read it. To do this:
+ Change the line images_folder = "/content/" to the folder you are using.

```
images_folder = "/content/"
```

+ Change "td_00044_b2.jpg" to the name of the file you are using.
```
# Set the path to the image
read_image_path = os.path.join (images_folder, "td_00044_b2.jpg")

# Open the image
read_image = open(read_image_path, "rb")
```

5. The code will also:
+ Call Azure using computervision_client with the image.

```
read_response = computervision_client.read_in_stream(read_image, raw=True)
```

+ Read the results line by line
+ If successful, print the text of each line as well as the coordinates of a rectangle in the image where the text is located.
6. Run the cell to read the handwriting in the image. 



## Summary
You have connected to Azure Cognitive Services Computer Vision and transcribed the text of an image on a website and an image on your computer. With this code, you can add more steps to process multiple images and store the transcribed text in a file or database.

## Bibliography

Library and Archives Canada. William Andrew White fonds, R15535-0-8-E, "Diary 1917" [http://collectionscanada.gc.ca/pam_archives/index.php?fuseaction=genitem.displayItem&lang=eng&rec_nbr=4818067](http://collectionscanada.gc.ca/pam_archives/index.php?fuseaction=genitem.displayItem&lang=eng&rec_nbr=4818067)

Quinn Dombrowski, Tassie Gniady, and David Kloster, "Introduction to Jupyter Notebooks," _Programming Historian_ 8 (2019), https://doi.org/10.46430/phen0087.

Graham, Shawn. Detecting and Extracting Hand-written text. Jan 28, 2020. https://shawngraham.github.io/dhmuse/detecting-handwriting/. Accessed 25 December, 2021.

Cognitive-services-quickstart-code, June 22, 2021, https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/quickstarts-sdk/python-sdk. Accessed 25 December, 2021.

## Footnotes

[^1]: Quinn Dombrowski, Tassie Gniady, and David Kloster, "Introduction to Jupyter Notebooks," _Programming Historian_ 8 (2019), https://doi.org/10.46430/phen0087.
[^2]: Cognitive-services-quickstart-code, June 22, 2021, https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/quickstarts-sdk/python-sdk. Accessed 25 December, 2021.
[^3]: Cognitive-services-quickstart-code, https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/quickstarts-sdk/python-sdk. Accessed 25 December, 2021.
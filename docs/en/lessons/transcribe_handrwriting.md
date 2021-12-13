---
title: Transcribe handwriting and text with Microsoft Azure Cognitive Services
authors:
- Jeff Blackadar
date: 2021-12-11
reviewers:
- 
layout: lesson
difficulty: 2
---


## Introduction

You can make headers, and see that they show up with the same styling that a published lesson has.

## Prerequisites

+ A computer with Python installed. Google Colab was used to write this lesson.
+ An internet connection.
+ A credit card.
+ A telephone number.
+ If you choose to use Google Colab to program Python, a Google account is required.

## Procedure
We'll transcibe a handwritten document by following three steps:
1. Register for a Microsoft account.
2. Obtain a key for the service and save it.
3. Connect to the service and transcribe the text on an image.
	1. An image saved on a disk drive
	2. An image on a web site

### 1. Register for a Microsoft account
1. Go to [https://portal.azure.com](https://portal.azure.com)
2. If you have an account with Microsoft or Github, log in and skip to 2. Setup Transcription: Create a Resource in Azure, below. 
3. If you don't have an account, register by clicking "No account? _Create one!_".
4. Input your e-mail address.
5. Check your e-mail inbox for a verification code and input this into the web browser.
### 2. Setup Transcription: Create a "Computer Vision" Resource in Azure
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
	1. _Subscription_: Azure subscription 1
	2. _Resource group_: click _Create new_
	3. For _Name_ input resource_group_transcription. Click OK.
 ![+ Resource group | Create new](/images/step2-9.png)
 10. In the _Instance Details_ section:
	 1. Select a Region near to you. This is where the instance is hosted.
	 2. Name the instance. Choose a unique name that is unique with letters or hyphens only. Input computer-vision-transcription-uuu, where uuu is your initials or something unique.
	 3. Set _Pricinet tier_ to Free F0.
11. Read the _Responsible AI Notice_ and check the box.
12. Click _Review + create_
13. Wait for a message to say _Your deployment is complete_
14. Click _Go to resource_
15. Once we see the resource screen for _computer-vision-transcription-jhb_ we can store the keys and endpoint we'll need to access this service from your computer.
### 3. Setup Transcription: Store Keys and Endpoint
To use the service we need to send a Key to an Endpoint. As it says on Azure: "Do not share your keys. Store them securely– for example, using Azure Key Vault. We also recommend regenerating these keys regularly. Only one key is necessary to make an API call."

To reduce the risk of inadvertently sharing keys we'll store them in a separate file in a different folder from the rest of the program we're writing. This say, if you check your code into a repository like GitHub, you won't check in your key along with your code. If you don't use GitHub, don't worry, just follow these instructions to store your keys.

We'll store the keys in a small JSON file. JSON has a specific format that is readable by Python and is used frequently for a variety of purposes. 
We'll open a Notebook on Google Colab and make a separate folder for your key JSON file. You can use Python on your personal computer in a similar manner.

1. Go to: https://colab.research.google.com/
2. Click New Notebook
3. Give the Notebook a title: "Transcribe handwriting and text with Microsoft Azure Cognitive Services.ipynb"
4. Input this code into a cell and run it by clicking the Run Cell button with a triangle.
```
# Mount Google Drive
from google.colab import drive
drive.mount('/content/drive')
```
5. To connect to Google Drive, follow the prompts, click _Connect to Google Drive_, choose your account, read the conditions and if you agree, click _Allow_.
6. Once this is complete, The message _Mounted at /content/drive_ indicates this is complete.
7. Click the Files icon on the left to show Google Drive.
8. If necessary, click the Refresh icon.
![Google Colab](/images/step3-8.png)


 


## Another Sample Header

Everything appears to work when I put another sample header, too. This will allow reviewers to examine a lesson and see more or less exactly what it will look like.[^1]

## Tables

Here's a sample table from a different lesson:


| Command | What It Does |
|---------|--------------|
| `pwd` | Prints the 'present working directory,' letting you know where you are. |
| `ls` | Lists the files in the current directory
| `man *` | Lists the manual for the command, substituted for the `*`
| `cd *` | Changes the current directory to `*`
| `mkdir *` | Makes a directory named `*`
| `open` or `explorer` | On OS X, `open` followed by a file opens it; in Windows, the command `explorer` followed by a file name does the same thing.
| `cat *` | `cat` is a versatile command. It will read a file to you if you substitute a file for `*`, but can also be used to combine files.
| `head *` | Displays the first ten lines of `*`
| `tail *` | Displays the last ten lines of `*`
| `mv` | Moves a file
| `cp` | Copies a file
| `rm` | Deletes a file
| `vim` | Opens up the `vim` document editor.

## Summary


## Bibliography


## Footnotes


[^1]: I say "more or less" so as not to court the vengeance of the Markdown gods!
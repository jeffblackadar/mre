# Selenium

From: https://www.browserstack.com/guide/record-and-playback-in-selenium

__This example shows the use of personally identifiable information. Use this at your own risk. Always protect your information. Delete this script when you are done with it or modify accordingly.__

## Prerequisites
+ Chrome browser

## Installation
+ Install Selenium extension from chrome web store
https://chrome.google.com/webstore/detail/selenium-ide/mooikfkahbdckldjjndioackbalphokd
+ Relaunch Chrome

## Make a Selenium project
+ Open the Selenium extension in Chrome

![](sel_1.png)

+ Record a new test in a new project

![](sel_2.png)

+ Name your new project. Example: look_for_openings. Click ok.

![](sel_3.png)

+ Input the BASE URL. Example: https://covid-19.ontario.ca/
+ Input TEST CASE NAME. Example: enter_info_1

![](sel_4.png)

+ Click the red REC button to start recording.
+ Click Book and appointment on-line
+ Click _I want to book my_ :Booster dose of the vaccine
+ Enter postal code
+ Enter Green Ontario photo health card
+ Enter Day and Month of last vaccine
+ Answer: _Did you get a non-Health Canada authorized vaccine_
+ Enter _Birth year_
+ Click Get results
+ Click: A the provincial online booking system
+ Click: Book appointment
+ Click: checkbox: _I confirm that I read, understood, and accept the full Terms of use.
+ Click button: _Download your enhanced vaccine certifiate or book your vaccine appointment_
+ Enter: _Green health card number_
+ Enter: _2-letter version code_
+ Enter: _9-character code on the back of the health card_
+ Enter: _Date of birth_
+ Enter: _Postal code_
+ Click: _continue_
+ Save project. Select a private place to save it.+ 

## Run tests
+ Click "play" button to Run current test

## Delete test
When you're done with this, delete the test, it has personally identifiable information in it.
+ Delete the test in Selenium
+ Delete the test .side file. 
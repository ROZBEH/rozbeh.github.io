---
layout: post
title:  "Google Colab Introduction"
date:   2023-12-18 22:36:56 -0800
excerpt: "Google Colab Introduction"
---

In this session we are going to talk about the basics Colab. These basics include:
* Using Colab enivironment as command line
* Reading file from local google drive
* Reading file from your local machine
* Downloading file to your local machine!
* Using GPU!
* Notes!


## Using Colab environment as command line

By using excalamation mark in front of any command Colab will treat it as terminal command. For example one could use !ls to list the files in the current directory. Here are few examples:

1. !wget "http://datasets.d2.mpi-inf.mpg.de/MPIIGaze/MPIIGaze.tar.gz"
  for downloading the file
2. !unzip myfile.zip
  for unzipping myfile.zip file
3. !pip install -U -q PyDrive
  Installing Python libraries
4. !apt-get install tar
  Installing command line dependencies
5. !rm Test.py
  removing the file Test.py from server

## Reading file from google drive into Colab environment

If you are working with Python and you have a lot of files to deal with, it is more convinient to upload your files to Google Drive and read them from there. First you need to give access to Colab in order to read your files. I should also mention that Colab file should be stored in the same Google account as Google Drive. First use the following command to use the python utility for accessing Google drive!

``!pip install -U -q PyDrive``

Use the following code snippet in order to give access to your Google Drive.



```
import os
from pydrive.auth import GoogleAuth
from pydrive.drive import GoogleDrive
from google.Colab import auth
from oauth2client.client import GoogleCredentials

auth.authenticate_user()
gauth = GoogleAuth()
gauth.credentials = GoogleCredentials.get_application_default()
drive = GoogleDrive(gauth)
```

Once you give access to Colab, now it's time to specify the name of the folder that you want to read its containing files. For now we just focus on the files inside a folder NOT folders inside folder. The alphanumerical code like `1pqZR3iYuwvVAw6BhSXHymQysIQVG8bnb` specifies the link that shows in your browser when you are inside the specific folder. If you are inside a folder in Google Drive and look the browser URL, some codes like this will be at the end of URL.

```
local_download_path = os.path.expanduser('~/data')
try:
  os.makedirs(local_download_path)
except: pass

file_list = drive.ListFile(
    {'q': "'1pqZR3iYuwvVAw6BhSXHymQysIQVG8bnb' in parents"}).GetList()
```

Once you read this specific folder, all the files will be stored inside file_list. In order to read some specific files you can commands like the following one. This command will download the files from your Google Drive to the local folder on server.

```
for f in file_list:
  print('title: %s, id: %s' % (f['title'], f['id']))
  fname = os.path.join(local_download_path, f['title'])
  print('downloading to {}'.format(fname))
  f_ = drive.CreateFile({'id': f['id']})
  f_.GetContentFile(fname)
```

Once the file is downloaded to the local folder on server which is usually on a folder like `/content/data/`

A little description about the following code snippet. Suppose we used the previous code snippet downloaded the `dev_brown.txt` file from Google Drive to local folder on server `/content/data/dev_brown.txt`, in order to read the file content and we just have to use the two following lines of python code.

```
text_file = open ("/content/data/dev_brown.txt",'r')
lines = text_file.readlines()
```

## Reading file from your local machine to Colab environment

If you have a file in your local machine and you want to upload it to the server local drive, use the following code snippet!



```
from google.Colab import files
uploaded = files.upload()
for fn in uploaded.keys():
  print('User uploaded file "{name}" with length {length} bytes'.format(
      name=fn, length=len(uploaded[fn])))
```

## Downloading file to your local machine
If you want to save some files from server into your local machine, use the following commands.

```
from google.Colab import files
with open('example.txt', 'w') as f:
  f.write('some content')
files.download('example.txt')
```

## Using GPU!
If you would like to use GPU in your Python code. Go to Runtime on the menu and choose `Change runtime type` and then choose GPU in the `Hardware accelerator`.

## Notes

* If you want to connect the Colab to your local machine, try to use this link.
  https://research.google.com/colaboratory/local-runtimes.html

* It is not to connect to a workspace on google Colab while another code is still running on the same work space! You should wait for the previous one to finish and then run your code!
* You have 12 hours to finish your task on Colab otherwise all of your files and results will be deleted.

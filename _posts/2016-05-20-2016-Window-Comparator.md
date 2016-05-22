---
layout: post
title: "Python Script for Renaming .jpg Files"
date: 2016-05-19
---

# Python Script to Rename .jpg Files

Trying to go back and locate all of ones pictures in a single place can be a daunting task.  I for one found myself going computer to computer, doing a globals search for .jpgs, and getting a long list of files that met this search criteria.  

I especially found myself doing this when trying to get someone's pictures off of broken or corrupt hard drive.  Leaving someone with thousands of files to sift through isn't ideal.  On a hunch I opened a .jpg file taken from a digital camera in a text editor, and found there is indeed plain text header information, including the date and time the picture was taken on the camera.  

I found that the format of this date and time was consistent across the two digital cameras that I have used. 

Using Python I made a simple script that traverses down a heiarchy of folders, finds .jpgs, then copies these files to a single folder and changes their name to the date they were taken.  


This code can be found on my github page [GitHub Page](https://github.com/jwb556)

The code itself, only uses standard (stock) Python modules.  The code is run from the command line, by first putting the script file in the top level heiachy folder to search in.  

---


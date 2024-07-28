---
title: "Performing digital forensics on Kali"
date: 2024-07-28T11:07:30-04:00
categories:
  - Tutorial
tags:

  - Forensics
  - Kali
---

### A continuation of "Setting up digital forensics on Kali hosted on Proxmox" for performing the forensics. 

#### Once you have your case all setup. Go and find the drive in question and click on "Analyze"

![](/assets/images/Analyze.png)

#### Then go ahead and verify the image integrity and hash it if not already done to make sure that the image is not modified during the investigation 

![](/assets/images/Hashing.png)

#### From here go ahead and hit Analyze to go and look through the files

We can use several features such as file analysis, keyword search, file type, image details, meta data, and data unit

![](/assets/images/Analyze_options.png)

The official documentation is [here](http://sleuthkit.org/autopsy/help/) with some more info [here](http://www.sleuthkit.org/informer/).

#### Some fun things I like to look at
* C:/Users
  * From here I can look at all the users on the host and poke around their desktop and documents
* Another option can be under the "File name search" you can enter regex for high value files 
  * Here is some example Regex for some files that may be of interest 
  * ```\.(docx?|pdf|xlsx?|pptx?|txt|rtf|zip|rar|7z|gz|tar|db|sql|mdb|accdb|jpe?g|png|psd|ai||py||bat|sh|pem|crt|key|p12|csv|pst|eml|msg)$```
    * The office documents are the most interesting to look at 

#### How to get Drives for Digital forensics

* Real drive
  * Goodwill
  * Estate Sales
  * Used desktops 
  * Old Personal drives

* Images
  * [Digital Corpora](https://digitalcorpora.org)
  * [Computer Forensic Reference DataSet Portal](https://cfreds.nist.gov)


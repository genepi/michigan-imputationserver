# Frequently Asked Questions

## I did not receive a password for my imputation job
Michigan Imputation Server 2 generates a random password for each imputation job. This password is not stored on the server at any time. If you did not receive a password, please check your email’s SPAM folder. Please note that we are unable to re-send the password.

## Unzip command is not working
Please check the following points: (1) When selecting AES256 encryption, use 7z to unzip your files (Debian: `sudo apt-get install p7zip-full`). For our default encryption, all common programs should work. (2) If your password includes special characters (e.g., \\), enclose the password in single or double quotes when extracting it from the command line (e.g., `7z x -p"PASSWORD" chr_22.zip`).

## Extending expiration date or reset download counter
Your data is available for 7 days. If you need an extension, please let [us](/contact) know.

## How can I improve the download speed?
[aria2](https://aria2.github.io/) attempts to utilize your maximum download bandwidth. Please remember to increase the -k parameter significantly (`--min-split-size=SIZE`). Otherwise, you may hit the Michigan Imputation Server 2 download limit for each file (thanks to Anthony Marcketta for pointing this out).

## Can I download all results at once?
We provide wget commands for all results. Please open the Results tab. The last column in each row contains direct links to all files.

## Can I set up Michigan Imputation Server 2 locally?
The workflow is based on Nextflow and can be downloaded from [here](https://github.com/genepi/imputationserver2) 

## Your web service looks great. Can I set up my own web service as well?
All web service functionality is provided by [Cloudgene](http://www.cloudgene.io/). Please contact us if you wish to set up your own service.

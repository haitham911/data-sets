# Readme

- The purpose of this repository is to organize and map data sets that can be used for testing different applications. 
- The structure to follow when new files are uploaded is creating a folder for each file type (if it does not exist).
- Additional mapping can be added to this readme to map to separate storage of data sets.
- Above dataset currently contains 88 distinctive files across 15 folders/15 different filetypes,  53MB in total.

# Datasets with malicious content
- https://github.com/filetrust/malicious-test-files -  **DO NOT CLONE THIS TO YOUR LOCAL PC**
   - Number of files: > 3,400
   - Total size: UNKNOWN
- https://github.com/k8-proxy/k8-test-data/tree/master/TestFiles/VirusShare
   - Number of files: 5 zip files with numerous malicious files inside
   - Total size: UNKNOWN

# Other datasets
- https://k8-mass-download.s3.eu-west-2.amazonaws.com/gov_Files.zip
   - Number of files: 9,824
   - Total size: 6.87GB
- s3://k8-test-data/gov_uk
   - Number of files: 11k
   - Total size: 11.9GB
- CDR-Plugin-test-files: s3://cdr-plugin-test-files
   - Folder **100** :         100  files, 60MB,  different files types
   - Folder **500** :         500  files, 375MB, subset of gov-uk
   - Folder **1000**:         1000 files, 604MB, subset of gov-uk
   - Folder **2000**:         2000 files, 1GB,   subset of gov-uk
   - Folder **all-clean-73**: 73   files, 40MB,  folder 100 that contains just clean files
   - Folder **hard-to-process**: 3 files, -, files that GW SDK has trouble processing, worth of investigating 
   - Folder **high_threat_govUk**: 88 files, -, govUK files marked as high threat ones after processing
   - https://cdr-plugin-test-files.s3-eu-west-1.amazonaws.com/gov-docs-ok-554.zip
   - https://cdr-plugin-test-files.s3-eu-west-1.amazonaws.com/gov-docs-ok-2492.zip
   - https://cdr-plugin-test-files.s3-eu-west-1.amazonaws.com/gov-uk-ok-9124.zip
   - s3://cdr-plugin-test-files/35_OK.zip, all clean files, 34.2GB taken from the link below
- https://downloads.digitalcorpora.org/corpora/files/govdocs1/zipfiles/
   - You can download one zip at the time on your local or use wget within remote CDR VM (ex. `wget https://digitalcorpora.s3.amazonaws.com/corpora/files/govdocs1/zipfiles/000.zip`)
   - To download all ~350GB of files run `aws s3 cp s3://digitalcorpora/corpora/files/govdocs1/zipfiles/ . --recursive`
- Websites that can be scrarped: 
   - http://www.bitsavers.org/bits/
   - https://wikileaks.org/sony/docs/01


# How to scarpe website
- Use `WGET` for [scarping](https://apple.stackexchange.com/questions/100570/getting-all-files-from-a-web-page-using-curl)
- Run one of below commands to scarp website. First will scarp whole website, second one just files that have pdf, jpg and png:
  ```
  sudo wget -r -np -nd -k <WEBSITE URL>
  sudo wget -r -np -nd -k  -e robots=off -A pdf,jpg,png <WEBSITE URL>
  ```
  
- Before downloading the website you can first spider the website then find links associated with the files you want to test and then download them
  ```
  sudo -w 1 --random-wait --no-check-certificate -e robots=off -o output.log --spider -r <WEBSITE URL>
  grep -oP "http\S+\.(pdf|doc|docx|docm|xls|xlsx|xlsm|ppt|pptx|jpg|gif|png)" 1.log | sort | uniq > links.txt
  wget -i links.txt --no-check-certificate -e robots=off
  ```

- After downloading the files you can check if additional cleaning is needed (removing the files that are not supported or files without extensions and similar)
- Websites that can be scrarped: 
   - http://www.bitsavers.org/bits/
   - https://wikileaks.org/sony/docs/01
   - or any other you would like

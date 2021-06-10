# Readme

- The purpose of this repository is to organize and map data sets that can be used for testing different applications. 
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
- https://downloads.digitalcorpora.org/corpora/files/govdocs1/zipfiles/
   - You can download one zip at the time on your local or use wget within remote CDR VM (ex. `wget https://digitalcorpora.s3.amazonaws.com/corpora/files/govdocs1/zipfiles/000.zip`)
   - To download all ~350GB of files run `aws s3 cp s3://digitalcorpora/corpora/files/govdocs1/zipfiles/ . --recursive`
- CDR-Plugin-test-files: s3://cdr-plugin-test-files
   - Folder **100** :         100  files, 60MB,  different files types
   - Folder **500** :         500  files, 375MB, subset of gov-uk
   - Folder **1000**:         1000 files, 604MB, subset of gov-uk
   - Folder **2000**:         2000 files, 1GB,   subset of gov-uk
   - Folder **hard-to-process**: 3 files, -, files that GW SDK has trouble processing, worth of investigating 
   - Folder **high_threat_govUk**: 88 files, -, govUK files marked as high threat ones after processing
   - **Failed files from govUk dataset**: s3://cdr-plugin-test-files/govUk_failed.zip
   - FFolder **clean_files**: multiple zip files, all files within this folder are clean (successfully rebuilt)
      - Folder **all-clean-73**: 73 files, 40MB, folder 100 that contains just clean files
      - **s3://cdr-plugin-test-files/clean_files/919_clean.zip**, folder 1000 that contains just clean files
      - **s3://cdr-plugin-test-files/clean_files/1907_clean.zip**, https://github.com/filetrust/test-files-generatorfolder 2000 that contains just clean files
      - **s3://cdr-plugin-test-files/clean_files/gov-docs-ok-554.zip**
      - **s3://cdr-plugin-test-files/clean_files/gov-docs-ok-2492.zip**
      - **s3://cdr-plugin-test-files/clean_files/gov-uk-ok-9124.zip**
      - **35_OK.zip**, all clean files, 34.2GB taken from the https://downloads.digitalcorpora.org/corpora/files/govdocs1/zipfiles/, s3://cdr-plugin-test-files/35_OK.zip
   
   - Scarped websites: 
   
| Source | S3 URL | Total files | Total size |
| --- | --- | --- | --- |
| [bitsavers](http://www.bitsavers.org/bits/)| [url to download zip](https://cdr-plugin-test-files.s3-eu-west-1.amazonaws.com/bitsavers.zip) | 1267 | 835MB |
| [wikileaks](https://wikileaks.org/sony/docs/01) | [url to download zip](https://cdr-plugin-test-files.s3-eu-west-1.amazonaws.com/wikileaks.zip) | 4009 | 1.1GB |
| [gwsolutions](https://glasswallsolutions.com/) | [url to download zip](https://cdr-plugin-test-files.s3-eu-west-1.amazonaws.com/gwsolutions.zip) | 490 | 56MB |
| [fticonsulting](https://www.fticonsulting.com/) | [url to download zip](https://cdr-plugin-test-files.s3-eu-west-1.amazonaws.com/fticonsulting.zip) | 2823 | 1.3GB |
| [digitalcorpora](https://downloads.digitalcorpora.org/corpora/files/govdocs1/zipfiles/) | [url to download zip](https://cdr-plugin-test-files.s3-eu-west-1.amazonaws.com/35GB_OK.zip) | - | 35GB |



# How to scarpe website
- Use `WGET` for [scarping](https://apple.stackexchange.com/questions/100570/getting-all-files-from-a-web-page-using-curl)
- Run one of below commands to scarp website. First will scarp whole website, second one just files that have pdf, jpg and png:
  ```
  sudo wget -r -np -nd -k <WEBSITE URL>
  sudo wget -r -np -nd -k  -e robots=off -A pdf,jpg,png <WEBSITE URL>
  ```

- In case of lots of 503 errors add `-U "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.79 Safari/537.36"` to above commands
- If you want to run `wget` in background add `-b`
  
- Before downloading the website you can first spider the website then find links associated with the files you want to test and then download them
  ```
  sudo wget 1 --random-wait --no-check-certificate -e robots=off -o output.log --spider -r <WEBSITE URL>
  grep -oP "http\S+\.(pdf|doc|docx|docm|xls|xlsx|xlsm|ppt|pptx|jpg|gif|png)" 1.log | sort | uniq > links.txt
  wget -i links.txt --no-check-certificate -e robots=off
  ```

- After downloading the files you can check if additional cleaning is needed (removing the files that are not supported or files without extensions and similar)
- Websites that can be scrarped: 
   - http://www.bitsavers.org/bits/
   - https://wikileaks.org/sony/docs/01
   - or any other you would like

# Script for generating random files
- https://github.com/filetrust/test-files-generator
- Files generated via above script are located in: **s3://cdr-plugin-test-files/random_generated_pdfs/** folder

# How to track high risk files to the url we downloaded them from
- Download files from specific website
- Run file processing via [CDR Platform](https://filetrust.github.io/cdr-plugin-folder-to-folder/)
- Check Threat Dashboard after the run and filter the results with threat level being set to high
- Download csv with the list of files
- Type the exact name in Browser and that should lead you back to the beggining 
- Example of govUK list of files with high threat level: [discover-file-analysis.txt](https://github.com/k8-proxy/data-sets/files/6552642/discover-file-analysis.txt)

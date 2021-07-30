# Option 2: Data Wrangling TMC Junior Engineer Skills
## Table of Contents
1. Overview
2. Instruction on Operating Script
3. Approach
4. Explanation of Program Function and Decisions
5. Future Improvements
6. Helpful Links and References 

## 1.) Overview
A TMC member from Texas needs help with analysis from two CSV files. The data is messy and formatted incorrectly. For the file to be imported into TMC’s redshift database the following corrections were made:
 
* No duplicate column names
* No white space in the column headers
* No following characters: [ ? ,  ! . & $ %  ] in column names
* Column names converted to lowercase
* Blank rows deleted from the CSV files 

The file was also automated using local files for the uploading process. Therefore, this script can be used on future datasets with similar formatting issues. The script was written with the python’s pandas library, glob and os modules. The programing editor was spyder (python 3.8) from the anaconda.navigator. 

## 2.) Instruction on Operating Script

1. Place the CSV files in the current working directory. You can also save them in any file but when you run the program the files MUST be in the current working directory. 

2. Run the program

3. A new file will be saved into the current working directory called MasterCSV.CSV. This file is the clean file and can be imported into TMC’s redshift.

## 3.) Approach

1. I started off by merging the csv files together. I did this by looping through the current working directory for CSV files and simply appending them. 

```
master_df = pd.DataFrame()

for file in os.listdir(os.getcwd()):
    if file.endswith('.csv') :
        master_df = master_df.append(pd.read_csv(file))
```    

2. I proceeded to figure out how to edit the data. 
```
df = pd.read_csv('./data_wrangle_file1.csv')

df.columns = df.columns.str.replace('[?,!,&,$,%]','')
df.columns = df.columns.str.replace(' ','')
df.columns = df.columns.str.lower()
```  
3. I then tried to combine the two techniques by merging the CSV file first and then editing them. That did not work. As I was researching pandas and through trial and error, I found that the combining functions in pandas are best used after the messy files are edited.

4.  Therefore, I decided to edit the CSV files first, then merge them into one CSV file. This yielded the correct result.

```
import glob
import os
import pandas as pd

os.listdir(os.getcwd()) 
filelist = glob.glob('*.csv') 

dfs= []    

for file in filelist:
    df = pd.read_csv(file)
    df.columns = df.columns.str.replace('[?,!,&,$,%]','')
    df.columns = df.columns.str.replace(' ','')
    df.columns = df.columns.str.lower() 
    df = df.dropna(how='all')
    dfs.append(df)

master_df = pd.concat(dfs, ignore_index=True) 
master_df.to_csv('MasterCSV.CSV', index = False)
```
## 4.) Explanation of Program Function and Decisions

In this section, the program functionality will be dissected to show understanding of the script mechanics as well as some simple explanation to why these techniques were used.
```
import glob
import os
import pandas as pd
```
* The imported libraries and modules. Without this, the program would create an error that says “NameError: name of ‘the library or module’ is not defined. Pandas was used because it can manipulate data quickly and easily.  

```
os.listdir(os.getcwd()) 
filelist = glob.glob('*.csv') 
```

* listdir() returns the names in the directory in the given designated path. Here the path is os.getcwd() which returns the current working directory. This is why the program MUST be run in the current working directory. Only can be used with os and glob.

```
dfs= []  
```
* Designated variable of an empty list for placing the edited DataFrame objects into. A list was used because .concat() was used to merge the two CSV files together. 

```
for file in filelist:  
```
* A for loop that will repeat any edits needed to any given amount a CSV files in the current working directory. The for loop was used because the script needed to repeat for a given amount of CSV files in the given location.

```
df = pd.read_csv(file)
```
* DataFrame that will read CSV files in the given location. Only can be used with pandas.  

```
df.columns = df.columns.str.replace('[?,!,&,$,%]','')
```
* The edit to the column labels of the DataFrame where the strings ? ,  ! . & $ %  are replaced by a blank space string. Only can be used with pandas.  

```
df.columns = df.columns.str.replace(' ','')
```
 * The edit to the column labels of the DataFrame where the strings of  a white space is replaced by a blank space string. Only can be used with pandas.  

```
df.columns = df.columns.str.lower() 
```
* The edit to the column labels of the DataFrame where the strings are converted into lowercase letters. Only can be used with pandas.  

```
df = df.dropna(how='all')
```
* The edit to the index of the DataFrame where all the values in a row with NA are removed. Only can be used with pandas.  

```
dfs.append(df)
```
* Appended list from line 8. This append added the information of the edited CSV files.Only can be used with pandas.   

```
master_df = pd.concat(dfs, ignore_index=True)  
```
* Variable that concatenated the appended list information from line 16. By default, concat with concatenate over the vertical. Stacking the data one on top of the other with matched column labels. It was decided to use concat because there were no duplicates in the index. Only can be used with pandas.  

```
master_df.to_csv('MasterCSV.CSV', index = False)
```
* Writes a CSV file called MasterCSV.CSV. Only can be used with pandas 

## 5.) Future Improvements 

1.  Clean up and add fail safes to the script. 

2. Creating a script that allows the master CSV file to be excluded from being edited and then added to the merged CSV files. Possible use of an if statement.  

3. Automate the upload of the CVS file to Redshift
    * I wasn’t able to complete this task on time but I did start the research.
        * I found a youtube video going over the process.
           * https://www.youtube.com/watch?v=dqkoBrgFSus 
        * Looked through the Parsons package and found the method to upload a table to S3.  
           * https://move-coop.github.io/parsons/html/aws.html#aws-s3

## 6.) Helpful Links and References 

https://pandas.pydata.org/

https://wiki.python.org/moin/
https://wiki.python.org/moin/

www.geeksforgeeks.org

https://stackoverflow.com/

https://vita.had.co.nz/papers/tidy-data.pdf (Tidy Data by Hadley Wickham.) 

Pandas 1.x Cookbook by Matt Harrison, Theodore Petrou

Data Wrangling with Python by Jacqueline Kazil, Katharine Jarmul 
* (Fun Fact: the author Jacqueline Kazil is on a slack channel called pyladies.slack.com and she is willing to give references )

Python Data Analytics with Pandas, NumPy, and Matplotlib by Fabio Nelli

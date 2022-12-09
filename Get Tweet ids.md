# -
顺利毕业啊……
import pandas as pd
import numpy as np
import time
import os
def count(str): 
    n = str.count('/') #check string structure
    return n
    def cuttweetid(str):
    ID = str.split("#")[0].split("/")[5] #These url strings share similar feactures: has one #, and five /
    return ID
    def getid(dataframe):
    IDs = []
    for i in dataframe["Mention URL"]:
        try:
            if i != 'Deleted or protected mention':
                IDs.append(cuttweetid(i))
            else:
                IDs.append('Deleted or protected mention')
        except Exception as e:
            IDs.append('Error')
            print(e)
    
    return IDs
    newfilename_list = []
path = '.././covid datasets/Synthesio_covid_English_National/archive/data_checked/March/' #March, April, May
id_path = '.././covid datasets/Synthesio_covid_English_National/archive/data_checked/Tweet IDs/'
for root, dirs, files in os.walk(path):
    for filename in files:
        
        if "export" in filename and "._" not in filename:
            tic = time.time()
            data = pd.read_csv(path + filename, encoding ='latin-1')
            results = getid(data)
         
            newfilename = filename.replace('export_mentions','tweetids')[:-4]
            print(newfilename)
            newfilename_list.append(newfilename)
            
            
            with open(id_path + newfilename + ".txt", "w") as output:
                output.write(str(results) + '\n')
            
            print("Length is consitent: ", len(results) == len(data))
            print("No Error occurs: ", 'Error' not in results) #check if there is an error
            toc = time.time()
            print("Now is ", time.ctime(toc), ", we spent ", round(toc - tic, 2),  " s for the last processing task.")
            

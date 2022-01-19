import os
import string
import shutil
import random
import pandas as pd
from glob import glob


# Dummy data creation scripts
# -----------------------------------
# Creates random/bad jpg files with A/B/C folders with filenames like AB.jpg, YU.jpg etc...

filenames = [random.choice(string.ascii_letters).upper()+random.choice(string.ascii_letters).upper()+".jpg" for x in range(0,40)]
for file in filenames:
    foldername = file.split(".")[0][0]
    os.makedirs(foldername, exist_ok=True)
    with open(foldername + "/" + file, 'w') as f:
        f.write("test")
filename_df = pd.DataFrame(data={"filename" : filenames})
filename_df.head()


## USE FROM HERE
# ==================

BASE_PATH = "./" # Add basepath here where the core folder and class structure is present
os.makedirs(BASE_PATH, exist_ok=True)
files_in_folders = glob(f"{BASE_PATH}/*/*.jpg", recursive=True) # Get files recursively (if) present

# Builds dictionary of classnames for individual files
DICT_MAP = {}
for folder in os.listdir(BASE_PATH):
    if os.path.isdir(folder):
        for file in os.listdir(os.path.join(BASE_PATH, folder)):
            DICT_MAP[file] = os.path.join(BASE_PATH, folder)
DICT_MAP




filename_df.head()



filename_df['CLASS'] = filename_df['filename'].map(DICT_MAP)
filename_df.head()


# Create New Folder structure with files from CSV
NEW_PATH = "./NEW_STRUCTURE"
os.makedirs(NEW_PATH, exist_ok=True)
for idx, row in filename_df.iterrows():
    filename = row['filename']
    source_class_folder = row['CLASS']
    os.makedirs(os.path.join(NEW_PATH, source_class_folder), exist_ok=True)
    shutil.copy(os.path.join(source_class_folder, filename),
                os.path.join(NEW_PATH, source_class_folder, filename))






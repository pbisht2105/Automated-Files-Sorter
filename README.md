# Automated File Sorter
![File Sorter Project cover]()


## Goal of the Project
The goal of this project is to automate the process of sorting files in a directory into organized subfolders based on their file extensions. It will help manage and clean up directories by automatically moving files to their appropriate folders such as "MS Office files", "Pictures", "Videos", "Documents", etc. Additionally, it handles file conflicts by renaming files that already exist in the target folder.

## Features
- **File Sorting**: Automatically sorts files into folders based on their file extensions (e.g., `.csv`, `.jpg`, `.mp4`, etc.).
- **Conflict Resolution**: If a file with the same name already exists in the target folder, it appends a timestamp to the file's name to avoid overwriting.
- **Folder Creation**: Automatically creates target folders if they don't already exist.
- **Customizable**: Easily change the file extensions and folder names as per your needs.

## Technologies Used
- **Python**: Used to write the file sorting script.
- **os module**: Used for interacting with the operating system (for file and directory management).
- **shutil module**: Used for moving files between directories.
- **datetime module**: Used for generating timestamps when renaming files.

## Code

```python
import os
import shutil
from datetime import datetime 

# Define paths and file extensions
path = "C:\\Users\\user\\Downloads\\New folder\\"  # Your source directory
extensions = [".csv", ".xlsx", ".docx", ".pptx"]
imgext = [".jpeg", ".jpg", ".png"]
video_extensions = [".webm", ".mkv", ".flv", ".vob", ".ogv", ".ogg", ".drc", ".gif", ".gifv", ".mng", 
                    ".avi", ".MTS", ".M2TS", ".TS", ".mov", ".qt", ".wmv", ".yuv", ".rm", ".rmvb", ".viv", 
                    ".asf", ".amv", ".mp4", ".m4p", ".m4v", ".mpg", ".mp2", ".mpeg", ".mpe", ".mpv", ".m2v", 
                    ".svi", ".3gp", ".3g2", ".mxf", ".roq", ".nsv", ".f4v", ".f4p", ".f4a", ".f4b"]
foldernames = ["MS Office files", "txt files", "Pictures", "PDF Files", "Softwares", "WinRAR Files", "Videos", "Zip Files", "Other"]

# Create folders if they don't exist
for folder in foldernames:
    folder_path = os.path.join(path, folder)
    if not os.path.exists(folder_path):
        os.makedirs(folder_path)
        print(f"Created folder: {folder_path}")

# Get a list of files in the source directory
filename = os.listdir(path)

# Function to move files to the appropriate folder
for file in filename:
    # Skip directories
    file_path = os.path.join(path, file)
    if os.path.isdir(file_path):
        continue

    # Determine target folder based on file extension
    target_folder = None
    if any(file.endswith(ext) for ext in extensions):
        target_folder = "MS Office files"
    elif any(file.endswith(img) for img in imgext):
        target_folder = "Pictures"
    elif any(file.endswith(vid) for vid in video_extensions):
        target_folder = "Videos"
    elif file.endswith(".exe"):
        target_folder = "Softwares"
    elif file.endswith(".pdf"):
        target_folder = "PDF Files"
    elif file.endswith(".txt"):
        target_folder = "txt files"
    elif file.endswith(".zip"):
        target_folder = "Zip Files"
    else:
        target_folder = "Other"

    # Construct target file path
    target_folder_path = os.path.join(path, target_folder)
    target_file_path = os.path.join(target_folder_path, file)

    # If the file doesn't exist in the target folder, move it
    if not os.path.exists(target_file_path):
        shutil.move(file_path, target_file_path)
        print(f"Moved: {file} to {target_folder}")
    else:
        # If the file exists, append a timestamp to the filename to avoid overwriting
        base, ext = os.path.splitext(file)
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        new_file_name = f"{base}_{timestamp}{ext}"
        new_target_file_path = os.path.join(target_folder_path, new_file_name)
        
        # Check if the new file name exists, and append a timestamp again if needed
        while os.path.exists(new_target_file_path):
            timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
            new_file_name = f"{base}_{timestamp}{ext}"
            new_target_file_path = os.path.join(target_folder_path, new_file_name)
        
        shutil.move(file_path, new_target_file_path)
        print(f"File conflict resolved: {file} renamed to {new_file_name} and moved to {target_folder}")


Code Explanation in Layman's Terms:
Setting Paths and File Types:

The script specifies a folder (path = "C:\\Users\\user\\Downloads\\New folder\\") where the files you want to organize are located.
It also defines which types of files go into which folder. For example, .csv, .xlsx, .docx, and .pptx files will go into "MS Office files", while .jpeg, .jpg, and .png files will go into "Pictures".
Folder Creation:

The script checks if the folders ("MS Office files", "Pictures", "Videos", etc.) already exist. If they don’t, it creates them automatically.
Identifying Files to Move:

It then gets a list of all files in the specified folder (path). For each file:
If the file is a directory, it is skipped.
It checks the file extension (like .csv, .jpg, .mp4) and decides which folder to move the file into based on its extension.
Moving Files:

If the file doesn't exist in the target folder, it moves the file there.
If the file already exists in the target folder, it adds a timestamp to the file’s name to make it unique (like file1_20231227_123456.csv). If a file with the new name already exists, it appends another timestamp to make it unique.
Output:

The script prints messages telling you which files were moved and which ones were renamed due to conflicts.
How It Works Example:
You have a file document1.csv in your Downloads folder.
The script checks the file and sees that it's a .csv file, so it moves it to the "MS Office files" folder.
If you have another document1.csv, the script renames it to something like document1_20231227_123456.csv to avoid overwriting the first file.
How to Use This Code:
Make sure Python is installed on your computer.
Copy and paste this code into a Python file, for example organize_files.py.
Adjust the path variable to point to the folder where your files are located.
Run the script, and it will organize the files in your folder into the appropriate subfolders based on file type.
This script helps keep your files organized automatically by their types, and it ensures there are no filename conflicts.

## Author

This project was created and maintained by [Pankaj Singh Bisht](https://github.com/pbisht2105).

You can contact me at: [pbisht2105@gmail.com](mailto:pbisht2105@gmail.com).

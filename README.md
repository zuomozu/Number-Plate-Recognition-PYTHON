# Number-Plate-Recognition-PYTHON
Python code for Detection and Recognition of Number Plates from parking lot camera 

# Introduction

This is a project that captures car number plates from a parking video and stores them in a database. The project uses the CV2 and EasyOCR libraries for image processing and text extraction respectively. The project is written in Python.

# Requirements

To run this project, you need to have the following installed:

Python 3.x
CV2 library
EasyOCR library
Pandas library
A database (MySQL, PostgreSQL, SQLite, etc.)
Usage

Clone the repository to your local machine.
Open the project folder in your code editor or IDE.
Make sure all the required libraries are installed.
In the main.py file, update the video file path and database connection details.
Run the main.py file.
The program will extract the car number plates from the video and store them in the database.
Contributing

If you want to contribute to this project, you can do so by submitting a pull request or opening an issue on the project's Github page.

# Flow

1. We open the video from our library and convert into frames, so we can loop throug it.
2. We use OpenCV concepts (gradient) to find rectangle contours for the number plate from a particular region.
3. After Finding the number plate we mask it and use easyOCR to extract the text from it.
4.


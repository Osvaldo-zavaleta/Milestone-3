# Milestone-3

import os
import openai
import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt
from openai import OpenAI

OPENAI_API_KEY=("your_open_api_key_here")

client = OpenAI()

# Import necessary libraries

import easyocr
import cv2
import matplotlib.pyplot as plt


# Function to extract text from an image
def extract_text_from_image(image_path):
    # Initialize the EasyOCR reader
    reader = easyocr.Reader(['en'])  # Specify the language(s)
    
#read the image
img = cv2.imread(image_path)

#Perform OCR on the image
results = reader.readtext(img)

#display the results
for (bbox, text, prob) in results:
(top_left, top_right, bottom_right, bottom_left) = bbox
        print(f'Text: {text}, Probability: {prob:.2f}')

# Draw bounding box around detected text
cv2.rectangle(img, (int(top_left[0]), int(top_left[1])), 
(int(bottom_right[0]), int(bottom_right[1])), 
(0, 255, 0), 2)

# Show the image with detected text
plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
plt.axis('off')
plt.show()

# Example usage
extract_text_from_image('path_to_your_image.jpg')

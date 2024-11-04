python --version
pip install streamlit easyocr requests pillow openai
streamlit run app.py
import streamlit as st
import easyocr
import requests
from PIL import Image
import io
import numpy as np
import openai

# Set your OpenAI API key
openai.api_key = "your_open_api_key_here"

# Function to extract text from an image URL
def extract_text_from_image(image_url):
    # Initialize the EasyOCR reader
    reader = easyocr.Reader(['en'])

# Download the image
response = requests.get(image_url)
img = Image.open(io.BytesIO(response.content))

#perform OCR on the image
results = reader.readtext(np.array(img))

#extract text and probabilities 
    extracted_text = ""
    for (bbox, text, prob) in results:
        extracted_text += f'Text: {text}, Probability: {prob:.2f}\n'

return extracted_text, img

# Function to summarize text using OpenAI
def summarize_text(text):
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
{"role": "user", "content": f"Please summarize the following text:\n\n{text}"}
        ]
    )
    return response['choices'][0]['message']['content']

# Streamlit app
st.title("OCR and Text Analysis with OpenAI")

# Input for image URL
image_url = st.text_input("Enter Image URL:", "https://cdn.discordapp.com/attachments/1295748105546891401/1302854084184772679/Water-Test-Result-1-1024x576.jpg?ex=6729a0eb&is=67284f6b&hm=b68d19552dd599582a18b709235ada55dfe6cb092166be8dcaa9bb61b338c8e3&")

if st.button("Extract Text"):
    with st.spinner("Extracting text..."):
        extracted_text, img = extract_text_from_image(image_url)
        st.image(img, caption='Uploaded Image', use_column_width=True)
        st.subheader("Extracted Text:")
        st.write(extracted_text)

#summarie the extracted text
if st.button("Summarize Text"):
    with st.spinner("Summarizing..."):
summary = summarize_text(extracted_text)
st.subheader("Summary:")
st.write(summary)
plt.axis('off')
plt.show()

# Example usage
extract_text_from_image('path_to_your_image.jpg')

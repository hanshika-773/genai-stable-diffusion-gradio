## Prototype Development for Image Generation Using the Stable Diffusion Model and Gradio Framework

### AIM:
To design and deploy a prototype application for image generation utilizing the Stable Diffusion model, integrated with the Gradio UI framework for interactive user engagement and evaluation.

### PROBLEM STATEMENT:
Generating high-quality and creative images from text descriptions is a challenging task. This project aims to create an easy-to-use application that can turn simple prompts into realistic images using the Stable Diffusion model. It also provides an interactive interface for users to try and evaluate the results.
### DESIGN STEPS:


### STEP 1:
Get the API key and set up access to the image generation model.

### STEP 2:
Write a function that takes a prompt and gets the generated image from the model.

### STEP 3:
Build a simple user interface using Gradio where users can type a prompt and see the generated image.

### PROGRAM:
```
import os
import io
import IPython.display
from PIL import Image
import base64 
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']

# Helper function
import requests, json

#Text-to-image endpoint
def get_completion(inputs, parameters=None, ENDPOINT_URL=os.environ['HF_API_TTI_BASE']):
    headers = {
      "Authorization": f"Bearer {hf_api_key}",
      "Content-Type": "application/json"
    }   
    data = { "inputs": inputs }
    if parameters is not None:
        data.update({"parameters": parameters})
    response = requests.request("POST",
                                ENDPOINT_URL,
                                headers=headers,
                                data=json.dumps(data))
    return json.loads(response.content.decode("utf-8"))

import gradio as gr 

def base64_to_pil(img_base64):
    base64_decoded = base64.b64decode(img_base64)
    byte_stream = io.BytesIO(base64_decoded)
    pil_image = Image.open(byte_stream)
    return pil_image

def generate(prompt):
    output = get_completion(prompt)
    result_image = base64_to_pil(output)
    return result_image

gr.close_all()
demo = gr.Interface(fn=generate,
                    inputs=[gr.Textbox(label="Your prompt")],
                    outputs=[gr.Image(label="Result")],
                    title="Image Generation",
                    description="Generate any image",
                    allow_flagging="never",
                    examples=["A cat sitting on the table"])

demo.launch(share=True)
```

### OUTPUT:

![Screenshot 2025-05-21 230443](https://github.com/user-attachments/assets/31a493ab-0e93-40c3-b6e8-9b4235aeae7e)

### RESULT:
A prototype application for image generation was successfully designed and deployed using the Stable Diffusion model. The application was integrated with the Gradio UI framework, enabling interactive user engagement and effective evaluation of the model’s output.

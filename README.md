## Prototype Development for Image Generation Using the Stable Diffusion Model and Gradio Framework
## NAME : SANDHIYA SREE
## REGISTER NO : 212223220093
### AIM:
To design and deploy a prototype application for image generation utilizing the Stable Diffusion model, integrated with the Gradio UI framework for interactive user engagement and evaluation.

### PROBLEM STATEMENT:
Develop an accessible and user-friendly platform for generating high-quality images from text prompts using the Stable Diffusion model. The application must allow real-time user interaction, customizable settings for image generation, and facilitate evaluation and feedback from users.

### DESIGN STEPS:


Step 1: Model Setup
Install the required libraries: transformers, diffusers, torch, and gradio.
Load the Stable Diffusion model using the diffusers library.
Step 2: Define Image Generation Function
Create a Python function to take a text prompt as input and generate an image using the model.
Allow optional parameters like image resolution, number of inference steps, and guidance scale for flexibility.
Step 3: Integrate with Gradio UI
Design a Gradio interface to accept user input and display generated images.
Configure sliders or text boxes for user-controlled parameters (e.g., resolution, style).
Deploy the application on a local server or a cloud platform.

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

#A helper function to convert the PIL image to base64 
# so you can send it to the API
def base64_to_pil(img_base64):
    base64_decoded = base64.b64decode(img_base64)
    byte_stream = io.BytesIO(base64_decoded)
    pil_image = Image.open(byte_stream)
    return pil_image

def generate(prompt, negative_prompt, steps, guidance, width, height):
    params = {
        "negative_prompt": negative_prompt,
        "num_inference_steps": steps,
        "guidance_scale": guidance,
        "width": width,
        "height": height
    }
    
    output = get_completion(prompt, params)
    pil_image = base64_to_pil(output)
    return pil_image

    gr.close_all()
demo = gr.Interface(fn=generate,
                    inputs=[
                        gr.Textbox(label="Your prompt"),
                        gr.Textbox(label="Negative prompt"),
                        gr.Slider(label="Inference Steps", minimum=1, maximum=100, value=25,
                                 info="In how many steps will the denoiser denoise the image?"),
                        gr.Slider(label="Guidance Scale", minimum=1, maximum=20, value=7, 
                                  info="Controls how much the text prompt influences the result"),
                        gr.Slider(label="Width", minimum=64, maximum=512, step=64, value=512),
                        gr.Slider(label="Height", minimum=64, maximum=512, step=64, value=512),
                    ],
                    outputs=[gr.Image(label="Result")],
                    title="Image Generation with Stable Diffusion",
                    description="Generate any image with Stable Diffusion",
                    allow_flagging="never"
                    )

demo.launch(share=True)

demo.close()

```

### OUTPUT:

<img width="1146" height="616" alt="Screenshot 2025-11-14 103958" src="https://github.com/user-attachments/assets/cb5f263b-e3b9-49bd-b693-a7178ab5cb92" />

<img width="1158" height="608" alt="Screenshot 2025-11-14 111021" src="https://github.com/user-attachments/assets/6f1c01b7-c954-4b63-aff4-8bf2d6b1f693" />

### RESULT:
The program successfully generates images based on the user’s prompt. The user can edit settings like inference steps, guidance scale, and image size. The final image is displayed instantly on the screen through the Gradio interface.



### RESULT:
Hence to design and deploy a prototype application for image generation utilizing the Stable Diffusion model, integrated with the Gradio UI framework for interactive user engagement and evaluation is verified.

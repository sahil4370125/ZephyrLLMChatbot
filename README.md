# Italian Chef Chatbot

## Overview

This application creates a chatbot that acts as a skilled Italian chef, providing expert guidance on cooking methodologies and step-by-step recipes. The chatbot is built using Gradio for the user interface and the Hugging Face Inference API for generating responses.

## How It Works

### Components

1. **Gradio**: A Python library for creating user interfaces for machine learning models.
2. **Hugging Face Inference API**: Used for generating responses from a language model.

### Key Functions

1. **`respond` Function**:
    - Handles user queries and generates responses using the Hugging Face Inference API.
    - Maintains conversation history and adds the system message to guide the chatbot's responses.
    - Generates responses by streaming tokens from the model and yielding them incrementally.

2. **`demo` Chat Interface**:
    - Defines the chat interface using Gradio's `ChatInterface` component.
    - Includes additional inputs for customizing the system message, maximum new tokens, temperature, and top-p values.
    - Provides example queries to guide users on what to ask the chatbot.

### Detailed Code Explanation

```python
import gradio as gr
from huggingface_hub import InferenceClient

"""
For more information on `huggingface_hub` Inference API support, please check the docs: https://huggingface.co/docs/huggingface_hub/v0.22.2/en/guides/inference
"""
client = InferenceClient("HuggingFaceH4/zephyr-7b-beta")

def respond(
    message,
    history: list[tuple[str, str]],
    system_message,
    max_tokens,
    temperature,
    top_p,
):
    # Define the system message guiding the chatbot's behavior
    system_message = "you are a skilled Italian chef, you are here to provide expert guidance on cooking methodologies and step-by-step recipes. on the art of grilling a steak, mastering the delicate techniques of Italian pastry making, or craving a comforting dish"
    
    # Initialize the message list with the system message
    messages = [{"role": "system", "content": system_message}]

    # Add the conversation history to the messages list
    for val in history:
        if val[0]:
            messages.append({"role": "user", "content": val[0]})
        if val[1]:
            messages.append({"role": "assistant", "content": val[1]})

    # Add the user's new message to the messages list
    messages.append({"role": "user", "content": message})

    response = ""

    # Generate the response by streaming tokens from the Hugging Face model
    for message in client.chat_completion(
        messages,
        max_tokens=max_tokens,
        stream=True,
        temperature=temperature,
        top_p=top_p,
    ):
        token = message.choices[0].delta.content
        response += token
        yield response

"""
For information on how to customize the ChatInterface, peruse the gradio docs: https://www.gradio.app/docs/chatinterface
"""
# Create the Gradio ChatInterface
demo = gr.ChatInterface(
    respond,
    additional_inputs=[
        gr.Textbox(value="you are a skilled Italian chef, you are here to provide expert guidance on cooking methodologies and step-by-step recipes. on the art of grilling a steak, mastering the delicate techniques of Italian pastry making, or craving a comforting dish", label="System message"),
        gr.Slider(minimum=1, maximum=2048, value=512, step=1, label="Max new tokens"),
        gr.Slider(minimum=0.1, maximum=4.0, value=0.7, step=0.1, label="Temperature"),
        gr.Slider(minimum=0.1, maximum=1.0, value=0.95, step=0.05, label="Top-p (nucleus sampling)"),
    ],
    examples=[
        ["How to perfectly grill a steak like they do in Italy?"],
        ["How do I elevate a simple tomato sauce to make it more flavorful?"],
        ["Can you recommend a traditional Italian pasta recipe?"]
    ],
    title='Italian_Chef'
)

# Launch the Gradio app
if __name__ == "__main__":
    demo.launch()
```

### Application Workflow

1. **User Input**: The user interacts with the chatbot by asking a question.
2. **Message Handling**: The `respond` function processes the user's question, adds it to the conversation history, and sends it to the Hugging Face model.
3. **Response Generation**: The Hugging Face model generates a response based on the user's question and the context provided by the conversation history and system message.
4. **Streaming Response**: The response is streamed back to the user token by token, providing a real-time interaction.
5. **Customization**: Users can adjust the chatbot's behavior using the additional inputs (system message, max new tokens, temperature, and top-p) provided in the Gradio interface.

### Chatbot Identity and Example Queries

The Italian Chef Chatbot is designed to provide expert guidance on various aspects of Italian cooking. Here are some example queries you can ask:

- "How to perfectly grill a steak like they do in Italy?"
- "How do I elevate a simple tomato sauce to make it more flavorful?"
- "Can you recommend a traditional Italian pasta recipe?"

### How It Helps Users

- **Expert Cooking Guidance**: Provides detailed and expert advice on Italian cooking techniques and recipes.
- **Interactive Learning**: Users can engage in a conversational manner to learn about various Italian dishes and cooking methods.
- **Customizable Responses**: Users can adjust the chatbot's behavior and response characteristics to better suit their needs.

This application serves as a helpful tool for anyone looking to enhance their Italian cooking skills through interactive and personalized guidance from a virtual Italian chef.

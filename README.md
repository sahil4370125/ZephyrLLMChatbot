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

# NarrativeGPT Project

## Overview

NarrativeGPT is a Streamlit-based web application designed to analyze user interactions and generate personalized narratives based on their conversation history. The project leverages natural language processing (NLP) techniques to understand user preferences, sentiments, and personality traits, and integrates these insights into a cohesive narrative.

## Features

- **Chat Interface**: Users can interact with the AI through a chat interface. The application captures and displays the conversation history.
- **Personalized Narrative Generation**: The application generates and updates a personalized narrative for the user based on their chat history.
- **Advanced NLP Analysis**: Utilizes NLP techniques to extract key themes, emotions, and personal traits from user conversations.
- **Persistent Memory**: Stores user narratives and updates them as new information is gathered from ongoing interactions.
- **Quality Assurance**: Ensures coherence and continuity in the narrative, avoiding inconsistencies and aligning with the user's overarching story.
- **User Feedback Incorporation**: Optionally integrates user feedback to refine the narrative further.

## Installation

To run this project locally, follow these steps:

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/narrative-gpt.git
   cd narrative-gpt
   ```

2. **Install Dependencies**:
   Make sure you have Python installed. Then, install the required packages:
   ```bash
   pip install streamlit clarifai python-dotenv
   ```

3. **Set Up Environment Variables**:
   Create a `.env` file in the project directory and add your Clarifai Personal Access Token (PAT):
   ```env
   CLARIFAI_PAT=your_clarifai_pat
   ```

4. **Run the Application**:
   ```bash
   streamlit run app.py
   ```

## Usage

1. **Open the Application**:
   After running the above command, open your web browser and navigate to `http://localhost:8501`.

2. **Interact with the AI**:
   Use the chat input at the bottom of the page to send messages to the AI. The conversation history will be displayed on the page.

3. **View Personalized Narrative**:
   The AI will analyze the conversation and generate a personalized narrative based on the extracted information. This narrative is updated continuously as more interactions occur.

## Code Explanation

### `app.py`

The main application file contains the following key components:

1. **Imports and Initialization**:
   ```python
   import streamlit as st
   from clarifai.client.model import Model
   from dotenv import load_dotenv
   import os

   st.title("NarrativeGPT")  

   load_dotenv()
   model_url = "https://clarifai.com/openai/chat-completion/models/GPT-4"
   pat = os.getenv("CLARIFAI_PAT")
   model = Model(model_url)
   ```

2. **System Prompt Definition**:
   The system prompt provides a structure for generating narratives:
   ```python
   system_prompt = """..."""
   ```

3. **Session State Initialization**:
   Ensures chat history is maintained across interactions:
   ```python
   if "messages" not in st.session_state:
       st.session_state.messages = []
   ```

4. **Display Chat History**:
   Displays previous chat messages on the application:
   ```python
   for message in st.session_state.messages:
       with st.chat_message(message["role"]):
           st.markdown(message["content"])
   ```

5. **User Input Handling**:
   Captures user input and processes it to generate AI responses:
   ```python
   if prompt := st.chat_input("What's up?"):
       with st.chat_message("user"):
           st.markdown(prompt)
       st.session_state.messages.append({"role": "user", "content":prompt})

       with st.chat_message("assistant"):
           message_placeholder = st.empty()
           full_response = ""
           inference_params = dict(
               temperature=0.2,
               system_prompt="you are normal gpt"
           )

           result = model.predict_by_bytes(
               prompt.encode(),
               input_type="text",
               inference_params=inference_params
           )

           full_response += result.outputs[0].data.text.raw
           message_placeholder.markdown(full_response)
       st.session_state.messages.append({"role":"assitant", "content":full_response})
   ```

## Contributing

If you would like to contribute to this project, please fork the repository and submit a pull request. For any issues or suggestions, feel free to open an issue on the repository.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## Acknowledgements

This project uses the following third-party libraries:

- [Streamlit](https://streamlit.io/)
- [Clarifai](https://www.clarifai.com/)
- [python-dotenv](https://github.com/theskumar/python-dotenv)

---

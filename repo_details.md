# Dataherald Community App Repository Details

## Overview

The Dataherald Community App is a Streamlit-based web application that allows users to interact with the Dataherald engine through a user-friendly interface. It provides features such as asking questions, adding golden records, viewing and removing golden records, table scanning, and viewing tables.

## Framework

The application uses the Streamlit framework.

### What is Streamlit?

Streamlit is an open-source Python library that makes it easy to create and share beautiful, custom web apps for machine learning and data science. In simpler terms, it's a tool that allows you to turn Python scripts into interactive web applications with minimal effort. You don't need to know HTML, CSS, or JavaScript to build a Streamlit app.

Key features of Streamlit:

*   **Simplicity:** Streamlit's API is designed to be intuitive and easy to learn, allowing you to build apps with just a few lines of code.
*   **Interactivity:** Streamlit apps are inherently interactive, allowing users to explore data and models through sliders, buttons, text inputs, and more.
*   **Automatic Updates:** Streamlit automatically updates the app whenever the underlying Python code changes, making development fast and iterative.
*   **Deployment:** Streamlit apps can be easily deployed to the cloud, allowing you to share your work with others.

## Integration with Other Frameworks

### Streamlit and Flask

While Streamlit is designed to be a standalone framework for building web applications, it is possible to integrate it with a Flask app. However, this typically involves running Streamlit as a separate process and using an iframe or proxy to embed the Streamlit app within the Flask app. This approach can add complexity to the application architecture.

### Streamlit and Langchain

Streamlit and Langchain are different tools with different purposes. Streamlit is a framework for building web applications, while Langchain is a framework for building applications powered by language models. While both can be used in the same project, they serve different roles. You might use Langchain to process natural language input and generate responses, and then use Streamlit to create a user interface for interacting with the Langchain application.

## Transitioning to LangChain

### Streamlit App Overview

1.  **Main Features and LangChain Integration:** The Streamlit app provides a user-friendly interface for interacting with the Dataherald engine, allowing users to ask questions in natural language and receive answers based on SQL queries. These features can be integrated into LangChain by using LangChain's language model capabilities to process user input and generate SQL queries, and then using LangChain's data connection capabilities to execute the queries and retrieve results.
2.  **Prioritized API Calls/Functions:** The API calls to the Dataherald engine for answering questions (`answer_question` in `Home.py`) and retrieving database connections (`get_all_database_connections` in `Home.py` and other pages) should be prioritized for LangChain integration. These functions are essential for the core functionality of the app.
3.  **Key Components for Refactoring:** The frontend setup (Streamlit components) and backend communication (API calls using `requests`) will need to be refactored to integrate seamlessly with LangChain. The Streamlit components will need to be replaced with LangChain's UI elements or adapted to work with LangChain's output format. The API calls will need to be replaced with LangChain's data connection and language model integration capabilities.

### Code Structure and Adaptation

1.  **Streamlit App Structure:** The Streamlit app is structured with a main page (`Home.py`) and several subpages in the `pages/` directory. Each page uses Streamlit components to create the user interface and interacts with the Dataherald engine through API calls. The `Home.py` file handles the main question answering functionality, while the other pages provide features for database management, golden record management, and instructions. The code seems relatively straightforward, but the transition to LangChain will require significant modifications to the frontend and backend logic.
2.  **Dependencies/Libraries:** The Streamlit app uses the `requests`, `streamlit`, `pandas`, and `pathlib` libraries. The `requests` library will likely be replaced by LangChain's data connection capabilities. The `streamlit` library will need to be adapted or replaced with LangChain's UI elements. The `pandas` library may still be useful for data manipulation and visualization.
3.  **User Input to SQL Queries:** The Streamlit app relies on the Dataherald engine to convert user input into SQL queries. This process can be replicated in LangChain by using LangChain's language model integration capabilities to process user input and generate SQL queries. The specific implementation will depend on the chosen language model and the desired level of control over the query generation process.

### API Integration and Query Handling

1.  **Database/Table Selection:** The app determines which database to query based on the `DEFAULT_DATABASE` variable in `Home.py` and the user's selection in the "Database Information" page (`pages/1_🗃️_Database_Info.py`). This logic can be implemented in LangChain by providing a similar mechanism for specifying the database connection and table names.
2.  **Error Handling/Response Validation:** The Streamlit app uses `try-except` blocks to handle API errors and displays error messages to the user. This error handling should be replicated in LangChain to ensure robustness and provide informative feedback to the user.
3.  **Data Visualizations:** The Streamlit app uses `pandas` DataFrames and Streamlit's built-in charting capabilities to display data visualizations. LangChain may not directly handle data visualizations, so additional components or libraries may need to be integrated to support this functionality.

### Model Interaction and Response Generation

1.  **Natural Language Response Generation:** The Streamlit app relies on the Dataherald engine to generate natural language responses after executing SQL queries. This process can be replicated in LangChain by using LangChain's language model integration capabilities to process the query results and generate coherent and correct responses.
2.  **Expected Outputs:** The expected outputs from the SQL queries include numbers, tables, and potentially charts. These outputs need to be integrated into the LangChain workflow and formatted appropriately for display to the user.
3.  **AI Models/Techniques:** The Streamlit app leverages the Dataherald engine, which likely implements AI models and other techniques for improving SQL queries and interpreting responses. These techniques can be leveraged in LangChain by integrating the Dataherald engine's API or by implementing similar models and techniques within LangChain.

### Testing and Future Improvements

1.  **Testing Plan:** After transitioning the app to LangChain, it should be tested thoroughly with a variety of edge cases and use cases, focusing on the accuracy of the generated SQL queries, the correctness of the responses, and the robustness of the error handling.
2.  **Areas for Optimization:** The API calls to the Dataherald engine may be a bottleneck in the Streamlit app. Converting to LangChain could allow for more efficient data access and query processing.
3.  **Prioritized Features/Enhancements:** After the initial transition, the following features should be prioritized:
    *   Improved natural language response generation.
    *   Integration of data visualization capabilities.
    *   Support for more complex SQL queries.
    *   Active learning and feedback mechanisms to improve the accuracy of the language model.

## Integrating Streamlit and LangChain

1.  You cannot directly "turn Streamlit into Python code" for LangChain. Extract the core logic and adapt it to work within a LangChain application.
2.  Yes, you can use Streamlit and LangChain together. Use Streamlit to create the UI for your LangChain application.
3.  Benefits: Streamlit provides a user-friendly interface, interactive components, and rapid prototyping, while LangChain offers powerful language model capabilities and data integration.

## API Interactions

The application uses the `requests` library to make API calls to the Dataherald engine.

### Request Bodies

Request bodies are constructed as Python dictionaries and then serialized to JSON using the `json` parameter in the `requests.post` method.

Example from `🏠_Home.py`:

```python
    "llm_config": {
        "llm_name": "gpt-4-turbo-preview"
    },
    "prompt": {
        "text": question,
        "db_connection_id": db_connection_id,
    }
}
```

This request body is then sent to the `/api/v1/stream-sql-generation` endpoint.

### API Endpoints

*   `/api/v1/database-connections`: Used to get all database connections.
*   `/api/v1/stream-sql-generation`: Used to answer questions.
*   `/api/v1/golden-sqls`: Used to add, view, and delete golden records.
*   `/api/v1/instructions`: Used to add, view, update, and delete instructions.
*   `/api/v1/table-descriptions/sync-schemas`: Used to scan tables.
*   `/api/v1/table-descriptions`: Used to list table descriptions.

## Page Construction

The pages are constructed using Streamlit components. Each page uses `st.title`, `st.header`, `st.subheader`, `st.write`, `st.text_input`, `st.selectbox`, `st.form`, `st.form_submit_button`, `st.dataframe`, and other Streamlit functions to create the user interface.

### Key Streamlit Components

*   `st.title`: Used to display the title of the page.
*   `st.header`: Used to display a header.
*   `st.subheader`: Used to display a subheader.
*   `st.write`: Used to display text.
*   `st.text_input`: Used to get text input from the user.
*   `st.selectbox`: Used to display a select box.
*   `st.form`: Used to create a form.
*   `st.form_submit_button`: Used to create a submit button for a form.
*   `st.dataframe`: Used to display a Pandas DataFrame.
*   `st.session_state`: Used to store data between page interactions.

### Page Structure

*   `🏠_Home.py`: This is the main page of the application. It allows users to ask questions and get answers from the Dataherald engine.
*   `pages/1_🗃️_Database_Info.py`: This page allows users to connect to a database, scan tables, and view table descriptions.
*   `pages/2_🧈_Golden_Record_Management.py`: This page allows users to add, upload, view, and delete golden records.
*   `pages/3_📜_Instructions.py`: This page allows users to add, view, update, and delete instructions.
*   `pages/4_📖_Help.py`: This page provides help and information about the application.

## Key Files

*   `🏠_Home.py`: Main application file.
*   `requirements.txt`: Lists the Python packages required to run the application.
*   `images/`: Contains images used in the application.
*   `pages/`: Contains the Python files for the different pages of the application.

## Usage

1.  Clone the repository:

    ```bash
    git clone https://github.com/Dataherald/streamlit-app.git
    ```
2.  Install the required packages:

    ```bash
    pip install -r requirements.txt
    ```
3.  Run the application:

    ```bash
    streamlit run 🏠_Home.py
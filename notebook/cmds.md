## Setting up the Virtual Environment

This project uses a virtual environment to manage dependencies and ensure a consistent development environment. Follow these steps to set it up:

1.  **Navigate to the project directory:**
    ```bash
    cd your-project-directory
    ```

2.  **Create the virtual environment:**
    ```bash
    python3 -m venv venv
    # Or, if you're using an older Python 3 version:
    # python -m venv venv
    ```

3.  **Activate the virtual environment:**

    * **macOS/Linux:**
        ```bash
        source venv/bin/activate
        ```
    * **Windows (Command Prompt):**
        ```bash
        venv\Scripts\activate
        ```
    * **Windows (PowerShell):**
        ```powershell
        .\venv\Scripts\Activate.ps1
        ```

    You should see `(venv)` at the beginning of your terminal prompt, indicating that the virtual environment is active.

4.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
    *(Note: This assumes you have a `requirements.txt` file listing your project's dependencies. If not, you'll need to install them individually using `pip install <package-name>`.)*

### What is a Virtual Environment?

A virtual environment is an isolated Python environment that allows you to install packages and dependencies for a specific project without affecting other Python projects or your global Python installation. This helps to:

* **Avoid dependency conflicts:** Different projects might require different versions of the same libraries. Virtual environments keep these dependencies separate.
* **Ensure reproducibility:** By using a `requirements.txt` file (generated using `pip freeze > requirements.txt`), you can easily recreate the exact same environment on another machine.
* **Keep your global Python installation clean:** You avoid cluttering your main Python installation with project-specific packages.

### Important Considerations:

* **`.gitignore`:** Make sure to add the virtual environment directory (`venv` or whatever you named it) to your `.gitignore` file. This prevents the virtual environment and its contents from being committed to your repository, as it can be large and platform-specific.

    ```
    venv/
    # Or, if you named it differently:
    # .venv/
    # env/
    ```

* **`requirements.txt`:** It's good practice to generate a `requirements.txt` file after installing your project's dependencies within the activated virtual environment. This file lists all the installed packages and their versions, allowing others (or your future self) to easily set up the same environment.

    ```bash
    pip freeze > requirements.txt
    ```

    Keep this file updated whenever you install or uninstall packages.

## Adhoc commands runs

1. Install dependencies

```
pip install --quiet --upgrade langchain-text-splitters langchain-community langgraph
```

2. Install and Setup LangSmith

<p>

<h3> Without langchain</h3>

1. Generate an API key

2. Install dependencies
pip install -U langsmith

3. Configure environment to connect to LangSmith.

LANGSMITH_TRACING=true
LANGSMITH_ENDPOINT="https://api.smith.langchain.com"
LANGSMITH_API_KEY="<your-api-key>"
LANGSMITH_PROJECT="pr-impassioned-macrame-69"
OPENAI_API_KEY="<your-openai-api-key>"

4. Run any LLM, Chat model, or Chain. Its trace will be sent to this project.

import openai
from langsmith.wrappers import wrap_openai
from langsmith import traceable

# Auto-trace LLM calls in-context
client = wrap_openai(openai.Client())

@traceable # Auto-trace this function
def pipeline(user_input: str):
    result = client.chat.completions.create(
        messages=[{"role": "user", "content": user_input}],
        model="gpt-3.5-turbo"
    )
    return result.choices[0].message.content

pipeline("Hello, world!")


<hr>

<h3>With LangChain</h3>

1. pip install -U langchain langchain-openai
4. Code change
from langchain_openai import ChatOpenAI

llm = ChatOpenAI()
llm.invoke("Hello, world!")
</p>
<hr>

## Install the 3 key components

pip install -qU "langchain[openai]"
pip install -U langchain langchain-openai
pip install -qU langchain-core


### once it has been run

go to https://smith.langchain.com/ and check the trace, under the project
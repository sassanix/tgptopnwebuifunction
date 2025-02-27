
# 🔌 tgpt-open-webui-integration

**Connect tgpt to Open WebUI for Enhanced Model Access!** 🚀

This integration bridges the gap between [tgpt](https://github.com/aandrew-me/tgpt) and [Open WebUI](https://github.com/open-webui), allowing you to seamlessly use tgpt's functionalities, including access to models like Phind, directly within your Open WebUI interface.

## ✨ Features

*   **Integrate tgpt with Open WebUI:** Use tgpt's command-line power within the familiar Open WebUI environment.
*   **Access Phind and more:** Leverage tgpt's model support, including Phind and other models accessible through tgpt, in Open WebUI.
*   **Simple Setup:** Designed for users who already have both tgpt and Open WebUI installed.

## 🛠️ Installation

1.  **Prerequisites:**
    *   Ensure you have [tgpt](https://github.com/aandrew-me/tgpt) installed and configured.
    *   Ensure you have [Open WebUI](https://github.com/open-webui) installed and running.
2.  **Copy the Integration Code:**
    *   Save the provided Python code as a `.py` file (e.g., `tgpt_integration.py`).
    *   Place this file in the appropriate directory within your Open WebUI installation to register it as a custom function or extension. (Consult Open WebUI documentation for the specific location for custom integrations).

## ⚙️ Configuration

*   **TGPT Binary Path:**
    *   The integration assumes `tgpt` is located at `/usr/local/bin/tgpt`.
    *   If your `tgpt` binary is in a different location, you will need to modify the `TGPT_PATH` variable in the `Valves` class within the Python code.

    ```python
    class Valves(BaseModel):
        TGPT_PATH: str = Field(
            default="/usr/local/bin/tgpt", description="Path to the tgpt binary"
        )
    ```
    *   **To change the path:** Edit the `default` value in the code to the correct path of your `tgpt` executable.

## 🚀 Usage

1.  **Select the `TGPT Integration` function** within Open WebUI when choosing a model or function for chat interactions.
2.  **Interact as usual:**  Type your prompts in the Open WebUI chat interface.
3.  **tgpt processes the query:** The integration will send your query to the `tgpt` command-line tool in the backend.
4.  **Response in Open WebUI:** The response from `tgpt` will be displayed in your Open WebUI chat window.

## 📜 Code Snippet

```python
"""
title: tgpt
author: Sassanix
author_url: https://github.com/sassanix/tgptopnwebuifunction
funding_url: https://github.com/sassanix/tgptopnwebuifunction
version: 0.1
"""
import subprocess
from pydantic import BaseModel, Field
from typing import Union, Generator, Iterator
class Pipe:
    class Valves(BaseModel):
        TGPT_PATH: str = Field(
            default="/usr/local/bin/tgpt", description="Path to the tgpt binary"
        )
    def __init__(self):
        self.type = "pipe"
        self.id = "tgpt_integration"
        self.name = "TGPT Integration"
        self.valves = self.Valves()
    def pipe(self, body: dict) -> Union[str, Generator, Iterator]:
        query = body.get("messages", [{}])[-1].get("content", "")
        if not query:
            return "No query provided."
        try:
            # Use subprocess to call tgpt with the query
            result = subprocess.run(
                [self.valves.TGPT_PATH, "--quiet", query],
                stdout=subprocess.PIPE,
                stderr=subprocess.PIPE,
                text=True,
            )
            if result.returncode != 0:
                return f"Error executing tgpt: {result.stderr.strip()}"
            return result.stdout.strip()
        except Exception as e:
            return f"Error: {str(e)}"
```

## 👨‍💻 Author

*   **Sassanix**
    *   [GitHub](https://github.com/sassanix)
    *   [Project Repository](https://github.com/sassanix/tgptopnwebuifunction)

## ⚖️ License

*   [Specify License here, e.g., MIT License] (Add your project's license information here)

---

**Enjoy using tgpt within Open WebUI!** 🎉
```

# 🔌 tgpt-open-webui-integration

**Connect tgpt to Open WebUI for Enhanced Model Access!** 🚀

Integrates [tgpt](https://github.com/aandrew-me/tgpt) with [Open WebUI](https://github.com/open-webui) to use tgpt models like Phind within Open WebUI. See README for installation, configuration, and usage.

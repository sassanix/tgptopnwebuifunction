# tgptopnwebuifunction
Allows for tgpt to be interacted within openwebui.


This is for those who have tgpt: https://github.com/aandrew-me/tgpt and open-webui: https://github.com/open-webui installed.

This allows you to talk to phind models and others through openwebui.
"""
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
"""

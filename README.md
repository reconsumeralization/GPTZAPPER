GPTZAPPER 🤖
GPTZAPPER is an automation pipeline that connects OpenAI's ChatGPT with GitHub via Zapier. It fetches new content written by ChatGPT and commits it automatically to a specified GitHub repository. Now, your chatbot musings can effortlessly find their way to your codebase.

How it Works 🛠
ChatGPT Trigger: A Zapier trigger is set for new content written by ChatGPT.
Python Zap: The Python code in this repo gets activated, fetching and appending the chat content to an existing GitHub file.
GitHub Commit: A new commit is made to your GitHub repo with the updated content.
Prerequisites 📋
A GitHub account and repository
A Zapier account
OpenAI API key for ChatGPT
Installation 🔧
Clone this repository:

git clone https://github.com/reconsumeralization/GPTZAPPER.git
Install the required Python packages:

pip install -r requirements.txt
Usage 🚀
Replace the placeholders for github_token, repo_name, file_path with your GitHub Token, Repository Name, and File Path respectively.

To test, run the function with sample chat content:

chat_to_github_repo("Sample chat content", "YOUR_GITHUB_TOKEN", "your_username/your_repo", "path/to/file.md")
Contributing 🤝
If you find a bug or have a suggestion, please open an issue. Contributions are welcome!

License 📄
This project is licensed under the MIT License - see the LICENSE file for details.

# GPTZAPPER
A Python GPTZAPPIER Automation for chatgpt.com - Zap - GitHub Repo - Based Repo Skeleton

import json
import requests

def chat_to_github_repo(chat_content, github_token, repo_name, file_path):
    api_url = f"https://api.github.com/repos/{repo_name}/contents/{file_path}"
    headers = {'Authorization': f'token {github_token}'}

    # Fetch existing file
    r = requests.get(api_url, headers=headers)
    existing_file = json.loads(r.text)

    # Decode existing content and append new chat content
    existing_content_decoded = base64.b64decode(existing_file['content']).decode('utf-8')
    updated_content = existing_content_decoded + "\n" + chat_content

    # Update the file
    payload = {
        "message": "ChatGPT content update",
        "content": base64.b64encode(updated_content.encode('utf-8')).decode('utf-8'),
        "sha": existing_file['sha']
    }

    r = requests.put(api_url, headers=headers, json=payload)
    return json.dumps({"status": r.status_code, "response": json.loads(r.text)})

# Sample call: chat_to_github_repo("New chat content here", "YOUR_GITHUB_TOKEN", "your_username/your_repo", "path/to/file.md")

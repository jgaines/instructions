# Instructions

A meta-collection of my own instruction files for copilot as well as links to
other collection I've run into here and there.  I'm doing this to track my own
exploration of copilot-instructions.md files.

The documentation and instruction files are geared towards Github Copilot as that's what I'm using.  The content of the instruction files should be applicable to any chat/agent system.

## Why use Instructions

Providing instructions to an AI agent guide it by providing contextual details
about your repository such as the workflow your team follows, tools and other
project specific details such as coding style, frameworks used or project
specific rules.

## How

1. **Install GitHub Copilot Extension**  
   - Open VS Code.
   - Go to the Extensions view (`Ctrl+Shift+X`).
   - Search for "GitHub Copilot" and install the official extension.

2. **Sign In to GitHub**  
   - After installation, you'll be prompted to sign in with your GitHub account.
   - Follow the authentication steps in your browser.

3. **Enable Copilot for Your Workspace**  
    - Open the command palette (`Ctrl+Shift+P`).
    - Type "Copilot: Enable" and select it to activate Copilot in your current workspace.

4. **Add a `copilot-instructions.md` File**  
   - In your repository, create a `.github` directory if it doesn't exist.
   - Create a file named `copilot-instructions.md` inside the `.github` directory.
   - Add project-specific instructions, workflows, coding styles, or any context you want Copilot to consider.

5. **(Optional) Use Copilot Chat**  
   - Install the "GitHub Copilot Chat" extension for conversational AI assistance within VS Code.

6. **Restart VS Code (if needed)**  
   - Sometimes a restart is required for all features to activate.

7. **Start Coding**  
   - Copilot will now use your `copilot-instructions.md` (and other context) to provide more relevant suggestions as you code.

> **Tip:** Keep your `copilot-instructions.md` updated as your project evolves for best results.

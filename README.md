# Instructions

A meta-collection of my own instruction files for copilot as well as links to
other collection I've run into here and there.  I'm doing this to track my own
exploration of copilot-instructions.md files.

The documentation and instruction files target Github Copilot as that's what I'm
using.  The content of the instruction files should be applicable to most chat
or agent systems.

## Why use Instructions

Providing instructions to an AI agent guide it by providing contextual details
about your repository such as the workflow your team follows, tools and other
project specific details such as coding style, frameworks used or project
specific rules.

## How to Get Started with GitHub Copilot

1. **Install GitHub Copilot Extensions**  
   - Open VS Code.
   - Go to the Extensions view (`Ctrl+Shift+X`).
   - Search for "GitHub Copilot" and install the official extension.
   - (Optional) Install the "GitHub Copilot Chat" extension for conversational
     AI within VS Code and [agent
     mode](https://code.visualstudio.com/blogs/2023/11/15/agents-public-preview)
     to allow you to interact with AI-powered agents directly in your editor.

2. **Sign In to GitHub**  
   - After installation, the system will prompt you to sign in with your GitHub
     account.
   - Follow the authentication steps in your browser.

3. **Configure Copilot Settings (if needed)**  
   - Copilot activates by default after installation and sign-in.
   - If needed, you can check settings via `File > Preferences > Settings` and search for "copilot".
   - Ensure `github.copilot.enable` has a value of `true` for the languages you
     want.
   - Verify `github.copilot.chat.codeGeneration.useInstructionFiles` has a value
     of `true` to use your `.github/copilot-instructions.md` file.

4. **Add a `copilot-instructions.md` File**  
   - In your repository, create a `.github` directory if it doesn't exist.
   - Create a file named `copilot-instructions.md` inside the `.github`
     directory.
   - Add project-specific instructions, workflows, coding styles, or any context
     you want Copilot to consider.

5. **Restart VS Code (if needed)**  
   - Sometimes a restart may be necessary for all features to activate.

6. **Start Coding**  
   - Copilot will now use your `copilot-instructions.md` (and other context) to
     provide more relevant suggestions as you code.  I have found that
     mentioning the instructions to Copilot in the first prompt of a session or
     if you've updated your instructions may be necessary.  You can also use the
     Add Context and pick copilot-instructions.md to remind it of the context.

## Tips

- Embed a validation step in your workflow to ensure that the instructions
  provided in `copilot-instructions.md` work effectively.
- Keep your `copilot-instructions.md` updated as your project evolves for best
  results.
- Use clear and concise language in your instructions to ensure Copilot
  understands your requirements.
  
## Searching for More Instructions

If you're looking for more examples of `copilot-instructions.md` files, you can
search GitHub repositories using the following query:

- Github [repositories](https://github.com/search?q=copilot-instructions.md)
  with `copilot-instructions.md`, once you get to the results, you can use the
  left sidebar to change the search to code and filter by language or other
  criteria.

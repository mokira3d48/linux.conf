The `->` turning into an arrow symbol in the Zed editor is caused by a font feature called **ligatures**. You can remove this behavior by disabling ligatures in your settings.

### ⚙️ How to Disable Ligatures

1.  **Open Settings**: Use the shortcut `Cmd + ,` (macOS) or `Ctrl + ,` (Linux/Windows) to open your `settings.json` file.
2.  **Add Configuration**: Insert the following configuration to disable the specific OpenType features responsible for ligatures:
    ```json
    {
      "buffer_font_features": {
        "calt": false,
        "liga": false
      }
    }
    ```
    *   `calt` stands for **Contextual Alternates**. This is the primary feature that enables sequences like `->` to become an arrow.
    *   `liga` stands for **Standard Ligatures**. This handles other common combinations like `fi` or `fl` being joined into a single character.

After saving the file, characters like `->`, `!=`, and `=>` should display as their individual text characters.

---
title: Gui
displaytext: GUI
layout:  null
tab: true
order: 4
tags: SecOpsTM-tag
---

### Web-based Graphical User Interface (GUI) Mode (Optional)

Launch the interactive web editor to visualize your threat model in real-time, edit Markdown content, and export various formats directly from your browser.

**Launch the GUI:**
    ```bash
    python -m threat_analysis --gui
    ```
    The console will display the address (e.g., `http://127.0.0.1:5001`) where you can access the GUI in your web browser.

![Loading a template](assets/images//gui_example.gif)    

> **Note:** When using `--gui`, the `--model-file` option can be used to load an initial threat model as a template into the editor. If not provided, the GUI will start with an an empty editor.

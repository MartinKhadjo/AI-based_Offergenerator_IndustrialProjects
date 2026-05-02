# Deployment & Configuration

This document describes how to install, configure and run the Offer Generator.  The software is developed in Python and packaged for Windows via an installer but can also be run from source.

## Installation

### Windows installer

For end users, the Offer Generator is distributed as an executable installer (`PEM_Offer_Generator_Setup.exe`).  Running the installer prompts you to select the installation directory, output directory for generated PPTX files, host address and port, and API credentials【144255623908017†L123-L134】.  During setup you may provide:

* **Installation directory** – where the application’s code and static files will reside.
* **Output directory** – the folder in which generated presentations are saved (defaults to `C:\offers`).  This directory can be a local path or a network share; ensure write permissions【144255623908017†L160-L167】.
* **Host & port** – by default the application binds to `0.0.0.0:5000`, allowing access from other machines on the network【144255623908017†L129-L132】.
* **Azure API key and endpoint** – the installer allows you to enter your Azure OpenAI API base URL and API key; this information is stored securely in `.env`【144255623908017†L123-L134】.

After installation, configuration files and templates are stored under `C:\ProgramData\PEMOfferGenerator`【144255623908017†L141-L148】.

### Running from source

Developers may run the Offer Generator from source.  Clone the private repository and install dependencies listed in `requirements.txt` (Flask, python‑pptx, openai, jinja2, etc.).  Copy the PPTX templates and mapping JSON into a `templates/` folder.  Create a `.env` file with the necessary settings (see below).  Then run `python app.py` to start the server.  When serving to a network, bind to `0.0.0.0` and ensure firewall rules permit inbound connections on the selected port.

## Configuration

The application reads settings from a `.env` file located in `C:\ProgramData\PEMOfferGenerator`.  The file is loaded at startup and can be edited to adjust behaviour.  Key parameters include:

| Variable | Purpose |
|---------|---------|
| `OPENAI_PROVIDER` | Selects the AI provider (`azure` or `openai`)【144255623908017†L150-L159】. |
| `AZURE_OPENAI_API_BASE` | Base URL for Azure OpenAI【144255623908017†L150-L159】. |
| `AZURE_OPENAI_API_KEY` | API key for Azure OpenAI【144255623908017†L150-L159】. |
| `AZURE_OPENAI_API_VERSION` | API version.  Default is `2024-02-15-preview`【144255623908017†L150-L159】. |
| `AZURE_OPENAI_DEPLOYMENT` | Deployment name (e.g. `gpt-4`)【144255623908017†L155-L156】. |
| `OPENAI_API_KEY` | Key for the public OpenAI API (used when `OPENAI_PROVIDER=openai`). |
| `OUTPUT_DIR` | Folder where generated PPTX files are stored【144255623908017†L160-L167】. |
| `IMAGE_ROOT` | Directory where WP images are loaded from【144255623908017†L160-L164】. |

After editing `.env`, save the file and restart the application.  The service will read the updated values at startup and apply them automatically【144255623908017†L170-L173】.

## Network drives and UNC paths

When specifying `OUTPUT_DIR` or `IMAGE_ROOT` on a network share, use a fully qualified UNC path (e.g. `\\pem-fs.company.net\offers\generated`).  Ensure that the user or service account running the application has read/write permissions on the share【144255623908017†L165-L167】.

## Running as a service

For a production environment, consider running the Flask application behind a production WSGI server such as Gunicorn or uWSGI and reverse proxying through Nginx or Apache.  On Windows you can run the generator as a background service or scheduled task.  Make sure to monitor logs and configure automatic restarts.
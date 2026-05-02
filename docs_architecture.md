# Architecture

This document explains the high‑level architecture of the AI‑based Offer Generator and shows how its components interact.  The design follows a separation‑of‑concerns pattern to keep the system modular, maintainable and secure.

## Overview

At a high level the Offer Generator consists of a **Flask web application** that collects user input, a **TextGenerator** that uses a GPT model to expand free‑text fields into structured bullets, a **TemplateLoader** that loads the appropriate PowerPoint template and mapping file, and a **PptxFiller** that clones and fills slides.  These components are orchestrated by the main controller in `app.py`.  Configuration settings such as API keys and output directories are centralised in `config.py` and persisted in a `.env` file under `ProgramData`【144255623908017†L140-L181】.  An optional anonymizer can hash names before they are written to the presentation.

![Architecture Overview](assets/architecture-overview.png)

### Web Layer

The user interacts with the system via a browser.  The entry point (`GET /`) renders an HTML form with fields for cover data and work packages.  Upon submission (`POST /`), the controller parses the form and orchestrates the generation pipeline【144255623908017†L263-L274】.  After the presentation has been generated, the user is redirected to `/success/<filename>` and can download the PPTX from `/output/<filename>`【144255623908017†L263-L274】.

### Text Generation

`text_generator.py` encapsulates all interactions with the GPT API.  It validates inputs, constructs prompts and parses the model’s output into a `PresentationSpec` object.  The prompts enforce strict limits on the number of bullets and characters to ensure concise, consistent output【144255623908017†L39-L63】【144255623908017†L295-L309】.  Both Azure OpenAI and public OpenAI APIs are supported; credentials and endpoints are selected via environment variables in `.env`【144255623908017†L150-L159】.

### Template Management

`template_loader.py` loads the appropriate PowerPoint template (English or German) and a JSON mapping file.  The mapping associates semantic keys (e.g. `initial_situation`, `wp_title`) with slide numbers and placeholder identifiers【144255623908017†L310-L321】.  When using the German template, shape names are mapped to canonical keys to ensure that the same filling logic works for both languages【144255623908017†L310-L321】.

### Slide Rendering

`pptx_filler.py` performs low‑level operations on the PPTX.  It clones slides, fills placeholders with generated content and images, localises headings, and prunes unused slides【144255623908017†L330-L343】.  Slide filling uses a priority order to resolve shapes: slide ID → placeholder index → shape name【144255623908017†L340-L345】.  The resulting presentation is saved into the configured `OUTPUT_DIR` and served to the user.

### Configuration and Security

All configuration values—API provider, API keys, output directory, image root, fonts and localisation settings—are centralised in `config.py`.  The application reads from `.env` in `C:\ProgramData\PEMOfferGenerator` with fallbacks defined in the code【144255623908017†L140-L181】.  Access to this directory should be restricted to administrators.  API keys must never be committed to version control; they should only exist in the `.env` file or secure secret stores.

### Optional Anonymizer

`anonymizer.py` can pseudonymise names before they are inserted into the presentation.  This is useful when personal identifiers need to be protected in the generated offer.  The anonymizer may use a deterministic hash (e.g. SHA‑256) to replace names with aliases.

## Processing Flow

The processing flow shows the sequence of actions from user submission to file delivery:

![Processing Flow](assets/processing-flow.png)

1. **User input**.  The user fills in the form (project data, Initial Situation, Project Goals, WP titles and descriptions) and submits it via `POST /`【144255623908017†L93-L107】.
2. **Input parsing and validation**.  The controller validates the counts of bullet fields, ensures at least one WP is present and attaches images from `IMAGE_ROOT`.  It selects the appropriate language template【144255623908017†L263-L274】.
3. **Presentation specification**.  `TextGenerator` expands free‑text fields into structured bullets according to the prompt contract (max bullets and characters) and returns a `PresentationSpec`【144255623908017†L295-L309】.
4. **Template loading**.  `TemplateLoader` loads the PowerPoint template and mapping, determines static slides, and identifies the WP detail slide【144255623908017†L310-L321】.
5. **Slide filling**.  `PptxFiller` clones slides, populates placeholders with content and images, localises headings, and prunes unused slides【144255623908017†L330-L343】.
6. **File saving**.  The presentation is saved to `OUTPUT_DIR` and the user is redirected to `/success/<filename>` with a download link【144255623908017†L263-L274】.

This flow is designed for reliability and extensibility.  Each step is encapsulated in a dedicated module, enabling unit testing and future improvements without affecting other parts of the system.
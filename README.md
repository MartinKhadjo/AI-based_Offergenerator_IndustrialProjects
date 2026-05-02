# AI‑Based Offer Generator for Industrial Projects

This repository provides a **high‑level overview** of the AI‑based Offer Generator used at the Production Engineering of E-Mobility Components (PEM) chair.  The software enables non‑technical users to produce consistent, branded PowerPoint offers for industrial projects in minutes.  It does **not** include the proprietary source code; instead it documents the architecture, workflows and professional achievements so that employers and recruiters can quickly grasp the scope of the project.

## Motivation and main goal

The Offer Generator streamlines the creation of professional offers.  Users enter project metadata and work‑package (WP) details via a simple web form.  The system then uses a GPT model—via Azure OpenAI or public OpenAI—to **expand and polish** the content and finally fills a PowerPoint template using `python‑pptx`.  The architecture deliberately separates three concerns: (1) content generation via a prompted LLM; (2) template and mapping management; and (3) slide filling.  This separation makes the system flexible and maintainable, allowing template changes without touching the code【144255623908017†L39-L63】.

## Key features

* **User‑friendly web interface**.  A simple form captures cover data—project title, offer number, customer, location and date—along with short descriptions of the **Initial Situation** and **Project Goals** and a dynamic list of work packages【144255623908017†L93-L107】.
* **GPT‑based text expansion**.  The `TextGenerator` module uses Azure OpenAI or public OpenAI to expand free‑text inputs into well‑structured bullet points.  Prompts enforce strict character and bullet counts to ensure concise, consistent output【144255623908017†L39-L63】【144255623908017†L295-L309】.
* **Template‑driven mapping**.  A `TemplateLoader` loads branded EN/DE PowerPoint templates and a JSON mapping file that associates semantic keys with slide numbers and placeholder identifiers【144255623908017†L310-L321】.  Localisation logic maps German shape names to canonical keys.
* **Automatic slide rendering**.  The `PptxFiller` clones slides, inserts content, localises headings and adds images.  It prunes unused slides and resolves shapes by ID, placeholder index or name【144255623908017†L330-L343】.
* **Secure configuration**.  Sensitive settings such as API keys and output paths are stored in a `.env` file under `C:\ProgramData\PEMOfferGenerator`, which should be restricted to administrators【144255623908017†L140-L181】.
* **Optional anonymizer**.  Names can be pseudonymised before inclusion in the final presentation to protect personal data.
* **Modular design**.  Components such as the UI, controller, text generation, template management and slide rendering are independent, enabling future extensions and substitutions.

## Repository contents

This repository documents the design and features of the Offer Generator:

| File/Folder | Description |
|------------|-------------|
| `README.md` | This overview. |
| `CHANGELOG.md` | Version history of the Offer Generator. |
| `LICENSE.md` | Copyright and usage restrictions. |
| `NOTICE.md` | Legal notices and acknowledgements. |
| `docs/architecture.md` | Detailed description of the architecture, components and processing flow, with diagrams【144255623908017†L39-L63】. |
| `docs/user-workflows.md` | Instructions for users on how to operate the system【144255623908017†L93-L107】. |
| `docs/deployment.md` | Guidance on installation, configuration and environment settings【144255623908017†L123-L174】. |
| `docs/security-and-privacy.md` | Best practices for handling API keys, output paths and personal data【144255623908017†L140-L181】. |
| `docs/portfolio-evidence.md` | Summary of technical achievements and project highlights. |
| `docs/github-repository-description.txt` | Short professional description for the GitHub repository listing. |
| `docs/assets/` | PNG versions of architecture and flow diagrams used in the documentation. |
| `docs/diagrams/` | Source `.dot` or other intermediate files (for transparency). |

The underlying application code remains private.  For demos or source access please contact the author under an appropriate licensing agreement.

## Copyright

© 2026 Martin Khadjavian – All rights reserved.  This repository is provided as documentation only; it may not be reproduced or distributed without express written permission.
# Offer Generator

The Offer Generator streamlines the creation of professional, branded PowerPoint offers.  
Users enter project metadata and work‑package details via a simple web form: title, offer number, customer name, location, date, language (EN/DE) and a list of work packages with objectives, approaches, results and client input.  
The system then uses GPT models to **expand and polish** the initial text into concise bullet points【495314768274285†L41-L63】 and fills a PowerPoint template using **python‑pptx**.  
The architecture deliberately **separates** three concerns【495314768274285†L41-L63】:
1. **Content generation** – the LLM transforms raw text into structured bullets and paragraphs with strict length limits.
2. **Template/mapping management** – JSON‑driven mappings locate placeholders inside EN/DE templates and are cached for reuse.
3. **Slide filling (PPTX renderer)** – clones slides, resolves shapes by ID or name, localises headings and inserts work‑package images.
This decoupling means non‑technical users can produce consistent, on‑brand offers while designers can update templates without touching code.  
Configuration (API keys, endpoints, output directory) is stored securely in a `.env` file under `C:\ProgramData\PEMOfferGenerator`.  


## Features

• Web form for entering offer metadata and work packages
• GPT-based content expansion and polishing
• Separate modules for text generation, template loading, and slide filling
• Supports multiple languages with customizable templates (EN/DE)
• Uses python-pptx for rendering slides
• Secure configuration via .env stored in ProgramData

## Architecture

![Architecture Diagram](docs/architecture.png)

The above diagram summarizes the major components and their relationships.

## Results / Usage

The Offer Generator accelerates the creation of tailored PowerPoint offers. It ensures consistency with corporate branding and reduces manual editing, saving significant time.

## License

Copyright © 2026 Martin Khadjavian. All rights reserved. This repository is provided for demonstration
and personal portfolio purposes. Redistribution or commercial use without explicit permission is
prohibited.

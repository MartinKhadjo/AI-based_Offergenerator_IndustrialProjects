# Architecture

## Objective
The AI-based offer generator automates the creation of professional PowerPoint offers for industrial projects while preserving template quality, language consistency and repeatability.

## Core idea
The software separates the problem into four major concerns:

1. **Web input layer** – browser-based form handling and request orchestration
2. **Text generation layer** – structured LLM-assisted content generation
3. **Template & mapping layer** – template selection and placeholder mapping
4. **PPTX rendering layer** – slide filling, cloning and export

This separation allows presentation design to evolve independently from the application logic.

## Processing flow
![Processing flow](assets/processing-flow.png)

### Interpretation of the flow
The manual shows a high-level pipeline in which raw user input is first collected and normalized, then transformed into a structured presentation specification before template-based rendering produces the final offer file.

From a software-engineering perspective, the important design characteristics are:
- structured data contract between UI and generation logic,
- clear handover from LLM output to deterministic rendering,
- template-driven population instead of hard-coded slide authoring,
- output generation into a validated target directory.

## Form submission sequence
![Form submission sequence](assets/form-submission-sequence.png)

### Main runtime interaction
The sequence diagram indicates the following runtime steps:
- the user submits the browser form,
- the Flask application parses and enriches the input,
- a text generation component produces a structured presentation specification,
- the template loader resolves the selected PowerPoint template and mappings,
- the PPTX filling component writes and saves the output presentation,
- the user is redirected to a success page and can download the result.

This reflects a practical pattern: use the LLM for controlled text expansion and use deterministic application logic for presentation rendering and file delivery.

## Component hierarchy
![Component hierarchy](assets/component-hierarchy.png)

## Main building blocks
### Web layer
The documented system includes:
- a browser form page,
- a success / download page,
- static assets such as styling, JavaScript and branding resources.

### Presentation generation layer
The manual describes the following major modules:
- **app.py** – entrypoint and request orchestration
- **config.py** – settings and path resolution
- **text_generator.py** – LLM-backed text transformation into a presentation specification
- **template_loader.py** – template and mapping access
- **pptx_filler.py** – low-level PowerPoint manipulation and save logic
- **template_mapper.py** – mapping support for placeholders and templates
- **anonymizer.py** – optional privacy utility

### Resource layer
The system depends on:
- branded PowerPoint templates,
- mapping metadata,
- image resources for work packages,
- an output directory for generated presentations.

## Architectural strengths
- Clean separation of generated content and deterministic slide rendering
- Reusable, template-driven architecture
- Configurability for language and provider setup
- Suitable for non-technical users through a browser UI
- Practical documentation and operational maintainability

## Public-safe limitations of this document
To preserve proprietary value, this document intentionally omits:
- source code,
- full internal template mappings,
- full prompt text,
- internal logic for placeholder resolution,
- operational secrets and private infrastructure details.

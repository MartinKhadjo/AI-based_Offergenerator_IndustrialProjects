# User Workflows

This guide explains how to use the Offer Generator as an end user.  The system is designed to be intuitive: you only need to fill a form and click **Generate** to receive a polished PowerPoint offer.

## Getting started

1. **Access the application**.  Open your web browser and navigate to the IP address and port where the Offer Generator is hosted (e.g. `http://134.130.189.249:5000`)【144255623908017†L69-L74】.  If the page does not load, ensure the application is running and the network address and port are correct.
2. **Fill the cover data**.  Enter the project title, offer number, customer name, location and date.  Choose the desired language: **English** or **German**【144255623908017†L93-L107】.
3. **Provide Initial Situation and Project Goals**.  Write concise free‑text descriptions of the initial situation and your project goals.  The generator will expand these into succinct bullet points.
4. **Add work packages**.  Click to add one or more Work Packages.  For each WP provide a title and short descriptions of objectives, approach, expected results and any client input.  Keep your inputs brief—prompts enforce strict character limits【144255623908017†L111-L116】.
5. **Generate**.  Press **Generate PPTX**.  The system processes your input, calls the GPT model, loads the appropriate template, fills the slides and saves the presentation.
6. **Download**.  After generation completes you will be redirected to a success page.  Click the download link to retrieve the PPTX file【144255623908017†L105-L108】.  Files are also saved to the configured `OUTPUT_DIR` on the server.

## Tips and troubleshooting

* **Keep inputs short and factual**.  The generator imposes character limits to maintain slide readability【144255623908017†L111-L116】.
* **Match counts**.  All lists (titles, objectives, approaches, results, client inputs) must have the same number of entries; mismatches result in an error.【144255623908017†L111-L117】.
* **Language selection**.  The language you choose determines both the template and the headings.  German templates use shape aliases to map localised names to semantic keys automatically【144255623908017†L310-L321】.
* **Images**.  For each work package the generator expects an image in `IMAGE_ROOT`.  If there are fewer images than WPs, the last image will be reused【144255623908017†L189-L192】.
* **Provider errors**.  If you encounter errors related to the AI provider, verify that your API key and endpoint are correctly configured in `.env`【144255623908017†L150-L159】.
* **Logs**.  System logs and errors can be inspected on the machine where the app is installed.  Consult your administrator for access【144255623908017†L73-L89】.

By following these steps you can quickly generate professional, branded offers without editing any slides manually.
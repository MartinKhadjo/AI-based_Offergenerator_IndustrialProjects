# Security & Privacy

This section outlines best practices for securing the Offer Generator and protecting sensitive data.

## Protect API keys

The Offer Generator requires API keys for Azure OpenAI or public OpenAI.  These keys should **never** be committed to source control or exposed in logs.  They must be stored in a `.env` file located in `C:\ProgramData\PEMOfferGenerator`【144255623908017†L140-L181】.  Restrict permissions on this directory so that only administrators or the service account can read or modify the file【144255623908017†L176-L179】.

## Secure the configuration directory

All configuration files (`.env`, mapping JSON, templates) reside in `C:\ProgramData\PEMOfferGenerator` after installation【144255623908017†L141-L148】.  Because these files contain sensitive information (API keys, output paths, template metadata), ensure that:

* Only authorised users can read or write to the directory.
* Regular backups are performed and stored securely.
* The directory is excluded from automated backups to cloud storage unless encrypted.

## Restrict network access

By default the application may be configured to listen on `0.0.0.0` to allow access from client machines【144255623908017†L129-L132】.  If you deploy the generator in a private network, restrict access to trusted subnets or set up a firewall to prevent unauthorised connections.  Avoid exposing the service to the public internet unless it is behind a properly configured reverse proxy with TLS and authentication.

## Use the anonymizer when needed

The optional `anonymizer.py` module can mask personal identifiers such as names or email addresses by hashing them.  Enable the anonymizer when generating offers that reference individuals who need to remain anonymous.  The pseudonymisation function should be deterministic so that the same input yields the same alias while not revealing the original value.

## Validate user inputs

The generator validates counts and lengths of all inputs to mitigate injection and formatting issues【144255623908017†L295-L309】.  Ensure that any modifications to prompt templates continue to enforce these constraints.  If additional fields are added, implement similar validation logic.

## Avoid sensitive content in prompts

Do not include confidential information, copyrighted terms or trademarked names in the free‑text fields.  GPT models may generate content based on the context provided; avoid including secrets or legal names that should not appear in the final offer【144255623908017†L111-L116】.

## Logging and monitoring

Enable logging to capture runtime errors and track usage patterns.  Store logs in a secure location with restricted access.  Monitor for unusual activity or repeated errors and address them promptly.  If logs include user inputs or API responses, ensure they are redacted or anonymised before retention.

By adhering to these guidelines, you can operate the Offer Generator safely and protect your organisation’s data.
# Email Translation Service

An email translation service and user management system built with Flask on Google Cloud Platform, created to bridge language barriers between the administration and roughly 1,600 multilingual members of the Stamford Boys & Girls Club.

The project consists of two independently deployable App Engine services.

## `languagecomm-sendgrid/` — Translation Service

- **Send translated email** (`/`): compose a message, choose a target language, and the subject and body are translated with the Google Cloud Translation API before being delivered through SendGrid. The translated result is shown on `/sent`.
- **Read translated email** (`/login`, `/read`): log in to a Gmail account over IMAP, fetch unread messages from the inbox, and display each one translated into English.

## `db-app/` — User Management

A CRUD interface backed by Google Cloud Datastore for maintaining member records (name, email, class, and preferred language), so that messages can be addressed in each member's language.

| Route | Purpose |
| --- | --- |
| `/` | List all members |
| `/create` | Add a member |
| `/read/<name>` | View a member |
| `/update/<name>` | Edit a member |
| `/delete/<name>` | Remove a member |

## Setup

Both services target the App Engine `python37` runtime and require a GCP project with the Translation and Datastore APIs enabled.

```bash
cd languagecomm-sendgrid   # or db-app
pip install -r requirements.txt
export GOOGLE_APPLICATION_CREDENTIALS=/path/to/service-account-credentials.json
export SENDGRID_API_KEY=...   # translation service only
python main.py              # run locally
gcloud app deploy           # deploy to App Engine
```

For deployment, set `SENDGRID_API_KEY` in `languagecomm-sendgrid/app.yaml`. Reading mail from Gmail over IMAP requires an app password for the account.

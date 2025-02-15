# FastImage

A Flask and Socket.IO web application for account-based messaging with file and image sharing. It includes registration and login routes, contact management, persisted message history, real-time client connections, and a browser interface served from `static/`. Uploaded files are stored on the server and can be shared through chat messages.

## Application features

- Registers users and issues access keys after successful authentication.
- Protects contact and messaging routes with an authorization key.
- Adds and removes contacts and can notify connected users when a contact is added.
- Stores messages as JSON files and supports paginated message retrieval.
- Uses Socket.IO rooms to associate browser connections with user sessions and deliver live events.
- Serves the front-end application and uploaded files from the Flask server.

The current implementation keeps connected-user state in process memory and persists contacts, credentials, and messages in files. It is a small self-hosted prototype rather than a horizontally scaled messaging service.

## Requirements

Use Python 3.10 or newer and install the packages used by the application, including Flask, Flask-SocketIO, Flask-CORS, Werkzeug, and environs. From the project directory:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install flask flask-socketio flask-cors environs
```

Create the data directories configured by the application before starting it. By default these are under `static/data/`, including upload and message folders. Configuration can be changed with `DATA_DIR`, `FILES_DIR`, `MESSAGES_DIR`, `CONTACTS_FILE`, and `PASSWORDS_FILE` environment variables; a `.env` file can be used through environs.

## Run locally

Start the Flask-Socket.IO application from this directory:

```bash
python3 main.py
```

The front end is served at the root route. For deployment, put the application behind a suitable WSGI/Socket.IO server and configure allowed origins and upload limits for the environment.

## API outline

The server provides `/register` and `/auth` for account setup and login. Authenticated clients can list, add, or remove contacts and exchange messages through the HTTP and Socket.IO interfaces. Requests to protected routes send the issued access key in the `Authorization` header and identify the active user using the username header expected by the endpoint.

Review `main.py` for the current request payloads and Socket.IO events before writing a client. The routes and data model are intentionally lightweight and may change as the project develops.

## Data and security

User records and message history are stored as files on disk, so protect the data directory and back it up if it contains important conversations. Do not commit credentials, access keys, uploaded private files, or production `.env` values. Configure HTTPS, upload limits, origin restrictions, and persistent storage before exposing an instance to untrusted users.

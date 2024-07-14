# Messaging System with RabbitMQ/Celery and Python Application behind Nginx

## Introduction
This project aims to deploy a Python application behind Nginx that interacts with RabbitMQ/Celery for email sending and logging functionality. The application is a Flask-based web application that sends emails asynchronously using Celery and logs messages to a file. The application also uses Nginx as a reverse proxy and Ngrok for exposing the local server to the internet.

## Table of Contents
- [Introduction](#introduction)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Configuration](#configuration)
- [Contributing](#contributing)
- [License](#license)

## Getting Started
### Prerequisites
1. **Local Setup:**
    - Install RabbitMQ and Celery on your local machine.
    - Set up a Python application with the following functionalities:
        - An endpoint that can accept two parameters: `?sendmail` and `?talktome`.

2. **Endpoint Functionalities:**
    - `?sendmail`: When this parameter is passed, the system should:
        - Send an email using SMTP to the value provided (e.g., `?sendmail=destiny@destinedcodes.com`).
        - Use RabbitMQ/Celery to queue the email sending task.
        - Ensure the email-sending script retrieves and executes tasks from the queue.

    - `?talktome`: When this parameter is passed, the system should:
        - Log the current time to `/var/log/messaging_system.log`.

3. **Nginx Configuration:**
    - Configure Nginx to serve your Python application.
    - Ensure proper routing of requests to the application.

4. **Endpoint Access:**
    - Use Ngrok or a similar tool to expose your local application endpoint for external access.
    - Provide a stable endpoint for testing purposes.

5. **Documentation and Walk-through:**
    - Record a screen-captured walk-through of the entire setup and deployment process.
    - Ensure the video covers:
        - RabbitMQ/Celery setup.
        - Python application development.
        - Nginx configuration.
        - Sending email via SMTP.
        - Logging current time.
        - Exposing the endpoint using Ngrok.
    - Submit the endpoint and screen recording.

### Features
- Asynchronous email sending using Celery and RabbitMQ.
- Logging system that writes to a log file with elevated permissions.
- Nginx configuration for serving the Flask application.
- Ngrok integration for exposing the local server to the internet.
- SSL setup for secure communication.

### Prerequisites
```sh
Python 3.10
Flask
Flask-Mail
Celery
RabbitMQ
Redis
Nginx
Ngrok
OpenSSL
```

### Installation
1. Install RabbitMQ, start RabbitMQ, and check status
```sh
sudo apt-get update
sudo apt-get install rabbitmq-server
sudo systemctl enable rabbitmq-server
sudo systemctl start rabbitmq-server
sudo systemctl status rabbitmq-server
```

2. Install Nginx: <br />
`sudo apt-get install nginx`

3. Install NGROK:
```sh
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | \
sudo gpg --dearmor -o /etc/apt/keyrings/ngrok.gpg && \
echo "deb [signed-by=/etc/apt/keyrings/ngrok.gpg] https://ngrok-agent.s3.amazonaws.com buster main" | \
sudo tee /etc/apt/sources.list.d/ngrok.list && \
sudo apt update && sudo apt install ngrok
```
### Usage

1. **Clone the repository:**
    ```bash
    git clone https://github.com/yourusername/Messaging-System.git
    cd Messaging-System
    ```

2. **Create a virtual environment and activate it:**
    ```bash
    python3 -m venv .venv
    source .venv/bin/activate
    ```

3. **Install the dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4. **Create a `.env` file for environment variables:**
    ```txt
        SMTP_MAIL_SERVER=smtp.your-email-provider.com
        SMTP_MAIL_PORT=587
        SMTP_MAIL_USERNAME=your-email@example.com
        SMTP_MAIL_PASSWORD=your-email-password
        SMTP_MAIL_USE_TLS=True
        SMTP_MAIL_USE_SSL=False
        RABBITMQ_ADDRESS=pyamqp://guest@localhost//
        REDIS_ADDRESS=redis://localhost:6379/0
    ```
### Configuration
1. **Generate SSL certificates:**
    ```bash
        openssl genrsa -out localhost.key 2048
        openssl req -new -key localhost.key -out localhost.csr
        openssl x509 -req -days 365 -in localhost.csr -signkey localhost.key -out localhost.crt
    ```

2. **Configure Nginx:**
    1. **Create configuration file for flask application:**
    ```bash
    sudo nano /etc/nginx/sites-available/messaging_system
    ```
    2. **Use the following configuration:**
    ```nginx
    server {
        listen 80;
        listen 443 ssl;

        server_name localhost;

        ssl_certificate /path/to/localhost.crt;
        ssl_certificate_key /path/to/localhost.key;

        location / {
            proxy_pass http://127.0.0.1:5000;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }

        location /log {
            alias /var/log/messaging_system.log;
            default_type text/plain;
            add_header Content-Type text/plain;
        }
    }
    ```

    3. **Enable the Configuration:**
    ```bash
    sudo ln -s /etc/nginx/sites-available/messaging_system /etc/nginx/sites-enabled
    ```

    4. **Test the configuration:**
    ```bash
    sudo nginx -t
    ```

    5. **Restart Nginx:**
    ```bash
    sudo systemctl restart nginx
    ```

#### Starting the application

1. **Run the Celery worker:**
    ```bash
    celery -A app.celery worker --loglevel=info
    ```

2. **Run the Flask application:**
    ```bash
    python app.py
    ```

3. **Expose the application using Ngrok:**
    ```bash
    ngrok http 80 --host-header=localhost
    ```

- Visit `http://localhost` to access the application.
- Use `http://localhost?sendmail=recipient@example.com` to queue an email.
- Use `http://localhost?talktome` to log the current time.
- Use `http://localhost/log` to see logs.

### Logging

Logs are written to `/var/log/messaging_system.log`. Ensure your user has the necessary permissions to write to this file.

Run the following commands to set the appropriate permissions:
````bash
    sudo touch /var/log/messaging_system.log
    sudo chown yourusername:yourusername /var/log/messaging_system.log
    sudo chmod 664 /var/log/messaging_system.log
    sudo usermod -a -G yourusername $USER
```

To get your username
```sh
- whoami
- $USER
```

## Contributing
Guidelines for contributing to the project.

1. Fork the repository
2. Create a new branch (git checkout -b feature-branch)
3. Commit your changes (git commit -m 'Add some feature')
4. Push to the branch (git push origin feature-branch)
5. Open a Pull Request

## License
Include the license information for the project.

This project is licensed under the GNU GENERAL PUBLIC LICENSE - see the LICENSE file for details.

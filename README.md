# Devops Task 3
## Task: Messaging System with RabbitMQ/Celery and Python Application behind Nginx
> Objective: Deploy a Python application behind Nginx that interacts with RabbitMQ/Celery for email sending and logging functionality.
Requirements:

1. Local Setup:

    - Install RabbitMQ and Celery on your local machine.

- Set up a Python application with the following functionalities:

    - An endpoint that can accept two parameters: `?sendmail` and `?talktome`.

2. Endpoint Functionalities:

- `?sendmail`: When this parameter is passed, the system should:

    - Send an email using SMTP to the value provided (e.g., `?sendmail=destiny@destinedcodes.com`).
    - Use RabbitMQ/Celery to queue the email sending task.

    - Ensure the email-sending script retrieves and executes tasks from the queue.

- `?talktome`: When this parameter is passed, the system should:

    - Log the current time to `/var/log/messaging_system.log`.

3. Nginx Configuration:

    - Configure Nginx to serve your Python application.
    - Ensure proper routing of requests to the application.

4. Endpoint Access:

    - Use ngrok or a similar tool to expose your local application endpoint for external access.
    - Provide a stable endpoint for testing purposes.

5. Documentation and Walk-through:

    - Record a screen-captured walk-through of the entire setup and deployment process.

- Ensure the video covers:

    - RabbitMQ/Celery setup.
    - Python application development.
    - Nginx configuration.
    - Sending email via SMTP.
    - Logging current time.
    1. Exposing the endpoint using ngrok.
    2. Submit the endpoint and screen recording.

- Evaluation Criteria:

    - `Functionality`: All specified features must work correctly.
    - `Clarity`: Code and configurations must be well-documented.
    - `Presentation`: The screen recording should be clear and comprehensive.

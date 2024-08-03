# Messaging System with Flask, Celery, and RabbitMQ
This documentation explains how to set-up a messaging system with a Flask-based application designed to handle email sending asynchronously using Celery and RabbitMQ. The application also includes basic logging functionality, traffic routing with nginx and can be exposed to the internet using Ngrok.

## Technologies Used
1. Flask: Python web framework for building the application.
2. Celery: Distributed task queue for asynchronous task management.
3. RabbitMQ: Message broker for handling task queues.
4. Flask-Mail: For sending emails.
5. Nginx: reverse proxy for improved security and performance.
6. Ngrok: tool for exposing the application to the internet for testing.

## Prerequisites

To follow along with these projects, you will need:
- An Ubuntu machine (WSL on Windows is okay).
- Python 3 and Nginx installed.
- A Gmail account.

## Installation and Setup
Clone Project
```sh
git clone https://github.com/DrInTech22/devops-stage-3.git
cd devops-stagae-3/messaging_system
python3 -m venv venv
```

Install Dependencies

```sh
pip install -r requirements.txt
```


RabbitMQ

```sh
sudo apt install rabbitmq-server -y
sudo systemctl enable rabbitmq-server
sudo rabbitmq-plugins enable rabbitmq_management
sudo systemctl start rabbitmq-server
sudo systemctl status rabbitmq-server
```

Ngrok
```sh
wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
unzip ngrok-stable-linux-amd64.zip
sudo mv ngrok /usr/local/bin/   

sudo rm -f ngrok-stable-linux-amd64.zip
ngrok --version
```

Nginx
```sh
sudo mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bkp # Backup defaul nginx.conf
sudo cp messaging_system/nginx/nginx.conf /etc/nginx/nginx.conf
sudo nginx -t  #test configuration for syntax errors
sudo systemctl reload nginx
```

## Project Structure:
```
messaging_system/
├── app/
│   ├── app.py
│   └── celery_config.py
│  
├── nginx/
│   └── nginx.conf
│
├── requirements.txt
├── .env
└── venv/
```
## Application Usage
Start Services:
```sh
# Activate virtual environment
source venv/bin/activate

# Start RabbitMQ
sudo rabbitmq-server -detached

# Start Celery worker
celery -A app.celery worker --loglevel=info --detach

# Run Flask application
python app.py
```

Test email functionality

```sh
curl http://localhost:5000?sendmail=your_email@example.com
```
Test talktome functionality
```
curl http://localhost:5000?talktome
```

View Logs:

```sh
curl http://localhost:5000/logs
```

Expose to Internet for external testing:

```sh
ngrok config add-authtoken <auth-token>
ngrok http 5000
```
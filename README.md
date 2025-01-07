# Prometheus-exporter-in-Python-for-RabbitMQ

#Install Dependencies
Ensure you have Python installed (preferably Python 3.8+). You can check your Python version using:

- python3 --version

#Install the required Python libraries:

- pip install prometheus-client requests

#Set Environment Variables
Before running the script, set the required environment variables:

RABBITMQ_HOST: The hostname of the RabbitMQ instance (e.g., localhost or your-rabbitmq-host).
RABBITMQ_USER: The username to access RabbitMQ HTTP API.
RABBITMQ_PASSWORD: The password to access RabbitMQ HTTP API.
You can set these variables temporarily in your terminal:


    export RABBITMQ_HOST="localhost"
    export RABBITMQ_USER="guest"
    export RABBITMQ_PASSWORD="guest"
    Or, add them to your shell profile (~/.bashrc, ~/.zshrc, etc.) for persistence.

#Run the Script
To run the exporter script, use:

- python3 rabbitmq_exporter.py

This will:

1. Start the Prometheus exporter server on port 8000.
2. Periodically fetch metrics from RabbitMQ.
3. Expose metrics at http://<your-host>:8000/metrics.

#Test the Exporter
Open a web browser or use curl to confirm the metrics are being served:

- curl http://localhost:8000/metrics
You should see metrics like:

      rabbitmq_individual_queue_messages{host="localhost",vhost="/",name="test_queue"} 100
      rabbitmq_individual_queue_messages_ready{host="localhost",vhost="/",name="test_queue"} 50
      rabbitmq_individual_queue_messages_unacknowledged{host="localhost",vhost="/",name="test_queue"} 20

#Run as a Background Process
If you want to run the exporter in the background, use:

- nohup python3 rabbitmq_exporter.py &
To stop the process, you can find its PID using:

- ps aux | grep rabbitmq_exporter.py

Then terminate it with:

- kill <PID>

#Integrate with Prometheus
Add the exporter to your Prometheus configuration (prometheus.yml):

scrape_configs:
  - job_name: 'rabbitmq_exporter'
    static_configs:
      - targets: ['localhost:8000']  # Replace localhost with the exporter host
Restart Prometheus to apply the changes.

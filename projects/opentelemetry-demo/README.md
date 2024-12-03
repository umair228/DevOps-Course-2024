# OpenTelemetry (OTel) Observability Demo Guide

OpenTelemetry (OTel) is an open-source project that provides a unified standard for collecting telemetry data from software applications. This includes **traces**, **metrics**, and **logs** to help understand application behavior and monitor systems effectively. OpenTelemetry offers APIs, libraries, and agents for instrumenting applications and exporting telemetry data.

💡 **Fun Fact:** OpenTelemetry is considered the second-best project in the CNCF landscape after Kubernetes!

---

<figure>
<img src="https://cloudchamp.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F84ad6f6a-681d-4a55-a9be-d328db326720%2F6d7a11a8-99df-42a9-bff2-21cade1459bf%2Fd68a88b2-913d-4ff3-b602-d2ccf8016dcb.png?table=block&id=05c1c8bf-0fe4-4b06-8ad5-33cdc616eb5f&spaceId=84ad6f6a-681d-4a55-a9be-d328db326720&width=1420&userId=&cache=v2">
</figure>

## Prerequisites

- **Cloud Platform**: AWS
- **EC2 Instance Configuration**:
    - **OS**: Ubuntu 22
    - **Instance Type**: t2.xlarge (Minimum 6GB RAM required)
    - **Storage**: 15GB

---

## Setup Guide

### 1. Clone the Repository

```bash
git clone https://github.com/open-telemetry/opentelemetry-demo.git
cd opentelemetry-demo/
```
### 2. Analyze the Docker Compose File

The repository includes a Docker Compose file to set up an observability demo with microservices. Here's a breakdown of its structure:

#### Logging Configuration (`x-default-logging`)

- Defines log handling in **JSON format** with:
    - A file size limit of **5MB**.
    - A maximum of **2 log files**.
- Includes a `tag` option to label logs based on the originating service.

#### Networks

- A custom network named `opentelemetry-demo` is defined using the **bridge driver**.
- This facilitates communication between containers.

#### Services Overview

The services in this Docker Compose file fall into three categories:

1. **Core Demo Services**: Application microservices written in different programming languages.
2. **Dependent Services**: Supporting services like **Redis** and **Kafka**.
3. **Telemetry Components**: Services like **Prometheus**, **Grafana**, **Jaeger**, and **OpenSearch** for managing telemetry data.
### 3. Service Breakdown

#### Core Demo Services

1. **Accounting Service (`accountingservice`)**
    - Uses a prebuilt Docker image.
    - Configured to send telemetry data to the OpenTelemetry Collector (`OTEL_EXPORTER_OTLP_ENDPOINT`).
    - Depends on `otelcol` (OpenTelemetry Collector) and `kafka`.

2. **Ad Service (`adservice`)**
    - Sends logs and metrics to the observability system.
    - Exposes additional ports for external interactions.

3. **Cart Service (`cartservice`)**
    - Handles shopping cart operations.
    - Integrates with services like `checkoutservice`.

4. **Checkout Service (`checkoutservice`)**
    - Manages the checkout process.
    - Communicates with multiple other services to ensure functionality.

#### Frontend Services

1. **Frontend**
    - The user-facing application.

2. **Frontend Proxy**
    - Manages traffic between the frontend and backend services.

#### Utility Services

1. **Image Provider**
    - Supplies images to the frontend.

2. **Load Generator**
    - Simulates user traffic for performance testing.

#### Telemetry Services

1. **Prometheus & Grafana**
    - Used for metrics visualization.

2. **Jaeger:** For distributed tracing.

### 4. Install Docker

Run the following commands to install Docker on your EC2 instance:

```bash
# Add Docker's official GPG key
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add Docker's repository
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Install Docker and Docker Compose
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
### 5. Start the Application

Run the following command to start all containers:

```bash
sudo docker compose up --force-recreate --remove-orphans --detach
```

<figure>
<img src="https://cloudchamp.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F84ad6f6a-681d-4a55-a9be-d328db326720%2Fbc3852e1-d61a-4ce2-a652-ec3235041409%2Fimage.png?table=block&id=40cd468d-d770-4d2e-9eb7-e2afd53b0e10&spaceId=84ad6f6a-681d-4a55-a9be-d328db326720&width=1420&userId=&cache=v2">
</figure>

#### Access the Application

- **Web Store**: [http://<IP>:8080/](http://<IP>:8080/)
- **Grafana**: [http://<IP>:8080/grafana/](http://<IP>:8080/grafana/)
- **Load Generator**: [http://<IP>:8080/loadgen/](http://<IP>:8080/loadgen/)
- **Jaeger UI**: [http://<IP>:8080/jaeger/ui/](http://<IP>:8080/jaeger/ui/)

<figure>
<img src="https://cloudchamp.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F84ad6f6a-681d-4a55-a9be-d328db326720%2Fd3e122b3-a14a-4f90-84be-c1286f8a83fc%2Fimage.png?table=block&id=5691b7f5-2bf7-4af2-8fb4-a9350c6b7b09&spaceId=84ad6f6a-681d-4a55-a9be-d328db326720&width=1420&userId=&cache=v2">
</figure>

<figure>
<img src="https://cloudchamp.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F84ad6f6a-681d-4a55-a9be-d328db326720%2F5e941ce9-9318-41c6-a256-d62e7f89b736%2Fimage.png?table=block&id=967b74b6-576a-4bf8-8a68-7f3759658373&spaceId=84ad6f6a-681d-4a55-a9be-d328db326720&width=1420&userId=&cache=v2">
</figure>


#### Observability Features

- **View Traces in Jaeger**:  
  Use Jaeger for distributed tracing to identify issues across services.
<figure>
<img src="https://cloudchamp.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F84ad6f6a-681d-4a55-a9be-d328db326720%2F6c4acfdf-a838-498d-a146-0cf7fdcfb869%2Fimage.png?table=block&id=b031c690-6962-4619-ba8f-e3e574253417&spaceId=84ad6f6a-681d-4a55-a9be-d328db326720&width=1420&userId=&cache=v2">
</figure>

- **Matrics in Grafana**:

    <figure>
  <img src="https://cloudchamp.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F84ad6f6a-681d-4a55-a9be-d328db326720%2F25ea79d8-0a87-4203-9718-84d677be17a2%2Fimage.png?table=block&id=70d2429c-b83d-4a58-8ff4-0cd6aaaa3d51&spaceId=84ad6f6a-681d-4a55-a9be-d328db326720&width=1420&userId=&cache=v2">
</figure>

- **Simulate Errors with Feature Flags**:  
  Enable the `adServiceFailure` feature flag to simulate errors.  
  Modify flag settings in `src/flagd/demo.flagd.json` by setting `defaultVariant` to `"on"` for the desired flag.


### Conclusion

This OpenTelemetry demo provides hands-on experience with observability, distributed tracing, and monitoring microservices. Use this setup to explore the core components of observability and understand the complexities of managing telemetry data in distributed systems.

For more details, refer to the [OpenTelemetry documentation](https://opentelemetry.io/).

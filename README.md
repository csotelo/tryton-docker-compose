# Tryton ERP on AWS Lightsail: Production Ready Template

This project provides a streamlined `docker-compose` configuration designed to deploy **Tryton ERP** specifically on **AWS Lightsail**.

It is based on and extends the original community work found at [Tryton Community Docker](https://foss.heptapod.net/tryton/tryton-docker/-/blob/branch/default/compose.yml).

## 🚀 Why this template?

While the official template is great for local development, deploying to a cloud VPS like AWS Lightsail requires specific handling for:
1.  **Persistence:** Pre-configured volumes for Database and Tryton data.
2.  **Custom Modules:** A dedicated volume strategy to add custom modules without breaking or editing the `docker-compose.yml` file.
3.  **Networking:** Specific instructions for AWS firewall rules.

## 📂 Project Structure

```text
.
├── docker-compose.yml
├── custom_modules/      <-- Place your custom module folders here
└── README.md 
```

## 🛠 Installation & Deployment

1. Prerequisites

    * An AWS Account.
    * An AWS Lightsail instance (OS only: Ubuntu/Debian OR App: Docker).
    * Docker and Docker Compose installed on the instance.

2. Clone and Run

Clone this repository to your Lightsail instance:

```Bash

git clone <your-repo-url>
cd <your-repo-folder>
docker compose up -d
```

3. Adding Custom Modules

To keep the architecture clean, we do not mount volumes for individual modules. Instead, we mount a parent directory.

    1. Place your module folders inside the custom_modules/ directory in this project.
    2. Restart the container:

```Bash
docker compose restart server
```
    3. The configuration automatically adds this directory to the PYTHONPATH, making your modules available for installation within Tryton.

## ☁️ AWS Lightsail Configuration (Crucial Step)

1. By default, AWS Lightsail blocks most incoming traffic. To access your Tryton instance, you must configure the instance firewall.
2. Go to your AWS Lightsail Console.
3. Click on your instance name.
4. Select the Networking tab.
5. Under IPv4 Firewall, click + Add rule.
6. Configure the rule:
    * Application: Custom
    * Protocol: TCP
    * Port or Range: `8000`
6. Click Create.

You can now access your ERP at: `http://<YOUR-STATIC-IP>:8000`

## 📝 Environment Variables

You can create a .env file to override default values:

```
DB_PASSWORD=secure_password
VERSION=7.0
```

_**Disclaimer:** This is a community contribution to facilitate Tryton adoption on AWS infrastructure._


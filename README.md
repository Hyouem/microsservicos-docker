# microsservicos-docker
microsservicos-docker
Estrutura do Projeto
microsservicos-docker/
│
├── service-user/
│   ├── app/
│   │   └── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── service-order/
│   ├── app/
│   │   └── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── service-product/
│   ├── app/
│   │   └── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── docker-compose.yml
└── README.md

Código de exemplo para cada serviço (Python + Flask)

service-user/app/app.py

from flask import Flask, jsonify

app = Flask(__name__)

@app.route("/users")
def get_users():
    users = [{"id": 1, "name": "Alice"}, {"id": 2, "name": "Bob"}]
    return jsonify(users)

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)


service-user/requirements.txt

Flask==2.3.3


service-user/Dockerfile

FROM python:3.12-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "app.py"]


O mesmo padrão se aplica para service-order e service-product, alterando a rota e dados para cada serviço.

Exemplo rápido service-order/app/app.py:

from flask import Flask, jsonify

app = Flask(__name__)

@app.route("/orders")
def get_orders():
    orders = [{"id": 101, "item": "Laptop"}, {"id": 102, "item": "Phone"}]
    return jsonify(orders)

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5001)

Docker Compose

docker-compose.yml

version: '3.9'

services:
  user:
    build: ./service-user
    ports:
      - "5000:5000"
    networks:
      - app-network

  order:
    build: ./service-order
    ports:
      - "5001:5001"
    networks:
      - app-network

  product:
    build: ./service-product
    ports:
      - "5002:5002"
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

README.md Inicial
# Microsserviços Docker - Exemplo

Este projeto demonstra uma estrutura básica de microsserviços usando Docker e Docker Compose.

## Serviços
- **User**: API de usuários (`/users`).
- **Order**: API de pedidos (`/orders`).
- **Product**: API de produtos (`/products`).

## Pré-requisitos
- Docker
- Docker Compose
- Python 3.12 (para desenvolvimento local)

## Como rodar
1. Clone o repositório:
```bash
git clone <seu-repositorio>
cd microsservicos-docker


Build e start:

docker-compose up --build

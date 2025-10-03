# 📊 Crypto Sentiment Tracker - Trabalho de Desenvolvimento de Sistemas

<p align="center">
  <img src="https://img.shields.io/badge/Status-Em%20Desenvolvimento-yellow?style=for-the-badge" alt="Status do Projeto"/>
  <img src="https://img.shields.io/badge/Etapa-3-blue?style=for-the-badge" alt="Etapa do Trabalho"/>
</p>

## 1. Visão Geral do Projeto

O **Crypto Sentiment Tracker** é uma plataforma completa para monitorar o sentimento do mercado de criptomoedas em tempo real. Através de um aplicativo mobile e uma interface web, os usuários podem votar em seu sentimento atual (otimista, pessimista ou neutro ) e visualizar como a comunidade se sente em relação ao mercado.

Este repositório é o **projeto principal** que organiza todos os componentes do sistema (mobile, frontend e backend) utilizando a abordagem de **submódulos Git**. Essa estratégia foi escolhida para combinar a modularidade de serviços independentes com a praticidade de um gerenciamento centralizado, facilitando a orquestração com Docker.

## 2. Arquitetura e Tecnologias

O projeto é dividido em três serviços principais, cada um gerenciado como um submódulo Git dentro deste repositório.

| Serviço | Tecnologia | Descrição |
| :--- | :--- | :--- |
| 📱 **Mobile** | Android Nativo (Jetpack Compose) | Permite que usuários votem e visualizem o sentimento do mercado em seus dispositivos Android. |
| 🌐 **Frontend** | React.js | Oferece uma experiência web rica para votação, visualização de gráficos e histórico de sentimentos. |
| ⚙️ **Backend** | Node.js, Express & MongoDB | Uma API RESTful que centraliza a lógica de negócios: recebe votos, agrega dados e os fornece para os clientes (mobile e web). |

## 3. Submódulos (Serviços)

Cada serviço vive em seu próprio diretório e possui um `README.md` com instruções específicas.

*   ➡️ **`/mobile`**: Contém o código-fonte do aplicativo Android.
    *   **Para detalhes, acesse o [README do Mobile](.https://github.com/Cxrzin08/crypto-sentiment-mobile/blob/9fb02631973881627504e4ff12e174f6e82aebf7/README.md).**
*   ➡️ **`/frontend`**: Contém o código-fonte da aplicação web em React.
    *   **Para detalhes, acesse o [README do Frontend](.https://github.com/Cxrzin08/crypto-sentiment-frontend/blob/44ecd93a4e590957abb8a51a95a5332ef65b7ad3/README.md).**
*   ➡️ **`/backend`**: Contém o código-fonte da API e a lógica de negócios.
    *   **Para detalhes, acesse o [README do Backend](.https://github.com/Cxrzin08/crypto-sentiment-backend/blob/f698bc354b1330b0cc4d681caf3d201fc6fd4eec/README.md).**

## 4. Como Configurar e Executar o Ambiente Completo

Para facilitar o desenvolvimento e a implantação, o projeto é totalmente containerizado usando Docker. O arquivo `docker-compose.yml` na raiz orquestra todos os serviços.

### Pré-requisitos

*   [Git](https://git-scm.com/ )
*   [Docker](https://www.docker.com/get-started )
*   [Docker Compose](https://docs.docker.com/compose/install/ )

### Passos para Configuração

1.  **Clonar o Repositório Principal (com submódulos):**
    Use o comando `--recurse-submodules` para clonar o repositório e inicializar os submódulos de uma só vez.
    ```bash
    git clone --recurse-submodules https://github.com/Cxrzin08/crypto-sentiment-main.git
    cd crypto-sentiment-main
    ```
    > **Atenção:** Substitua `SEU_USUARIO` pelo seu nome de usuário no GitHub.

2.  **Atualizar os Submódulos (se necessário ):**
    Caso já tenha clonado o projeto sem os submódulos ou queira atualizá-los, use:
    ```bash
    git submodule update --init --recursive
    ```

3.  **Iniciar todos os serviços com Docker Compose:**
    Este comando irá construir as imagens de cada serviço e iniciará os contêineres.
    ```bash
    docker-compose up --build
    ```

Após a execução, os serviços estarão disponíveis nos seguintes endereços:
*   **Frontend (Web):** `http://localhost:3000`
*   **Backend (API ):** `http://localhost:5000`
*   **Banco de Dados (MongoDB ):** `mongodb://localhost:27017`

### Arquivo `docker-compose.yml`

Este arquivo define como os serviços `frontend`, `backend` e `mongo` se conectam e operam.

```yaml
# docker-compose.yml
version: '3.8'

services:
  # Serviço de Backend (API)
  backend:
    build: ./backend
    ports:
      - "5000:5000"
    environment:
      - MONGO_URI=mongodb://mongo:27017/crypto_sentiment
    depends_on:
      - mongo
    volumes:
      - ./backend:/app # Monta o diretório local para desenvolvimento em tempo real

  # Serviço de Frontend (React App)
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend
    volumes:
      - ./frontend:/app # Monta o diretório local para desenvolvimento em tempo real

  # Serviço do Banco de Dados
  mongo:
    image: mongo:latest
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:

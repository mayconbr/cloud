# Guia de Deploy com GCloud e Docker

## Requisitos

1. Baixe e instale o [Docker Desktop](https://www.docker.com/products/docker-desktop/).
2. Baixe e instale o [SDK do Google Cloud](https://cloud.google.com/sdk/docs/install).
3. Configure o Visual Studio para o modo `Release`, pois no modo `Debug` ele escuta uma porta específica e o GCloud precisa rodar na `8080`.

## Autenticação

### Login com o GCloud:

```sh
gcloud auth login
```

### Configuração do Docker para o GCloud:

```sh
gcloud auth configure-docker
```

## Publicação da Imagem no GCR

### Renomear a imagem local do Docker para ser compatível com o GCloud:

```sh
docker tag sistemaiatrix:latest gcr.io/coral-bebop-428813-g3/sistemaiatrix:1.0
```

### Fazer o push da imagem para o GCR (isso também força a criação do bucket do GCR):

```sh
docker push gcr.io/coral-bebop-428813-g3/sistemaiatrix:1.0
```

### Configuração do projeto no GCloud:

```sh
gcloud projects describe coral-bebop-428813-g3 --format="value(projectNumber)"
gcloud config set project coral-bebop-428813-g3
```

## Deploy no Cloud Run

### Publicação no Cloud Run (serverless, sem uso de VM):

```sh
gcloud run deploy sistema-iatrix-service --image gcr.io/coral-bebop-428813-g3/sistemaiatrix:1.0 --platform managed --region us-central1 --allow-unauthenticated
```

#### Para produção, basta mudar o nome do serviço:

```sh
gcloud run deploy sistema-iatrix-live --image gcr.io/coral-bebop-428813-g3/sistemaiatrix:1.0 --platform managed --region us-central1 --allow-unauthenticated
```

---

## CRM Audax

### Publicação da imagem:

```sh
docker tag crmaudax:latest gcr.io/audax-449214/crmaudax:1.0
docker push gcr.io/audax-449214/crmaudax:1.0
```

### URL do serviço:

[CRM Audax](https://crmaudax-249196489593.us-central1.run.app)

---

## Fiado Garantido

### Publicação da imagem:

```sh
docker tag fiadogarantido:latest gcr.io/audax-449214/fiadogarantido:1.0
docker push gcr.io/audax-449214/fiadogarantido:1.0
```

### URL do serviço:

[Fiado Garantido](https://fiadogarantido-249196489593.us-central1.run.app/)

---

## Criação de Novo Artifact Registry e Cloud Run

### Etapas:

1. Criar um **Artifact Registry** (bucket onde a imagem do container será armazenada). **Ativar a opção de exclusão automática para permitir upload com a mesma tag.**

2. Subir a imagem para o container, seguindo as etapas:
   - Adicionar o `Dockerfile` ao projeto.
   - Configurar o `Program` corretamente.
   - Gerar a imagem Docker.
   - Renomear a imagem local.
   - Fazer o push da imagem para o **Artifact Registry**.
3. Criar um serviço **Cloud Run** selecionando a imagem enviada.

4. Configurar o **mapeamento de domínio** para o serviço.

---

Este guia cobre todas as etapas necessárias para autenticação, criação de imagens Docker, publicação no GCR e deploy no Cloud Run. Certifique-se de ajustar os comandos conforme necessário para o seu projeto específico.


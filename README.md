# 🐾 FutureVet — Clyvo Vet Platform

> Challenge 2026 — 3º Semestre | FIAP | DevOps Tools & Cloud Computing

---

## 👥 Equipe

| Nome | RM |
|------|-----|
| João Victor Caetano Alves da Silva | RM 562074 |
| João Victor Bueno Castelini da Silva | RM 564115 |
| Ryan Vetoriano | RM 565667 |
| Felipe Furlanetto | RM 562766 |
| Raul Rezende Iemini Aguiar | RM 564002 |

---

## 📋 Descrição do Projeto

O **FutureVet** é uma plataforma de medicina veterinária digital desenvolvida em parceria com a **Clyvo Vet**, com foco na **continuidade do cuidado e engajamento na jornada de saúde do pet**.

A solução conecta responsáveis, clínicas e o ecossistema veterinário, transformando o cuidado animal de algo reativo para um modelo **contínuo e preventivo**. O sistema permite o cadastro e acompanhamento de pets, tutores, consultas e histórico de vacinação.

### 🎯 Benefícios para o Negócio

- **Redução de falhas no acompanhamento:** histórico centralizado evita perda de informações entre consultas
- **Engajamento proativo:** lembretes e acompanhamento contínuo aumentam a fidelização dos tutores
- **Escalabilidade em nuvem:** infraestrutura Azure com Docker garante disponibilidade e fácil expansão
- **Rastreabilidade completa:** registro de consultas, vacinas e evolução de saúde em um único lugar

---

## 🏗️ Arquitetura Macro

```
┌─────────────────────────────────────────────────────────┐
│                    Azure Cloud (Canada Central)          │
│                                                          │
│  ┌─────────────────────────────────────────────────┐    │
│  │              Resource Group: rg-FutureVet        │    │
│  │                                                  │    │
│  │  ┌──────────────────────────────────────────┐   │    │
│  │  │         VM Ubuntu 22.04 (Standard_D2s_v3) │   │    │
│  │  │                                          │   │    │
│  │  │  ┌─────────────┐    ┌─────────────────┐ │   │    │
│  │  │  │  Container  │    │    Container    │ │   │    │
│  │  │  │  Java App   │◄──►│   H2 Database   │ │   │    │
│  │  │  │  :8080      │    │   + Volume      │ │   │    │
│  │  │  └─────────────┘    └─────────────────┘ │   │    │
│  │  │                                          │   │    │
│  │  │  NSG: portas 22 | 80 | 8080 abertas     │   │    │
│  │  └──────────────────────────────────────────┘   │    │
│  └─────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tecnologias Utilizadas

| Camada | Tecnologia |
|--------|------------|
| Linguagem | Java + Spring Boot |
| Banco de Dados | H2 |
| Containerização | Docker + Docker Compose |
| Nuvem | Microsoft Azure |
| VM | Ubuntu 22.04 LTS (Standard_D2s_v3) |
| IaC | Azure CLI Script |

---

## 🚀 How to — Instalação da Solução

### Pré-requisitos

- Conta ativa no **Microsoft Azure**
- **Azure CLI** instalado na sua máquina
- **Git** instalado

### Passo 1 — Clonar o repositório

```bash
git clone [ INSERIR LINK DO GITHUB ]
cd futureVet
```

### Passo 2 — Provisionar a VM na Azure

Execute o script de criação da infraestrutura:

```bash
chmod +x criacao.sh
sed -i 's/\r$//' criacao.sh
./criacao.sh
```

O script irá:
- Criar o Resource Group `rg-FutureVet`
- Criar a VNet e Subnet
- Criar e configurar o NSG (portas 22, 80, 8080)
- Provisionar a VM Ubuntu 22.04 (`Standard_D2s_v3`) em `canadacentral`
- Instalar Docker e Docker Compose na VM

### Passo 3 — Conectar na VM via SSH

```bash
ssh azureuser@<IP_PUBLICO_DA_VM>
# Senha: Fiap@Cloud2026
```

### Passo 4 — Subir a aplicação com Docker Compose

```bash
git clone [ INSERIR LINK DO GITHUB ]
cd futureVet
docker compose up -d
```

### Passo 5 — Verificar se está rodando

```bash
docker ps
curl http://localhost:8080
```

### Passo 6 — Acessar a aplicação

Acesse pelo navegador:
```
http://<IP_PUBLICO_DA_VM>:8080
```

---

## 📄 Script Azure CLI

O arquivo `criacao.sh` na raiz do repositório contém todo o script de provisionamento da infraestrutura Azure.

> ⚠️ Lembre-se de executar `az login` antes de rodar o script.

---

## 🐳 Docker

### Dockerfile

O `Dockerfile` na raiz do projeto realiza o build da aplicação Java e gera a imagem da aplicação.

### Docker Compose

O `docker-compose.yml` sobe dois containers:
- **app** — aplicação Java na porta 8080
- **db** — banco H2 com volume nomeado `futurevet_data` para persistência dos dados

```bash
# Subir em background
docker compose up -d

# Ver logs
docker compose logs -f

# Parar
docker compose down
```

---

## 🎥 Vídeo de Demonstração

> 📺 **[ INSERIR LINK DO YOUTUBE ]**

O vídeo demonstra:
1. Execução do script Azure CLI
2. Conexão SSH na VM
3. Aplicação rodando em nuvem via Docker Compose
4. Demonstração das rotas da API
5. Remoção da VM ao final

---

## 📁 Estrutura do Repositório

```
futureVet/
├── src/                    # Código fonte Java/Spring Boot
├── Dockerfile              # Build da aplicação
├── docker-compose.yml      # Orquestração dos containers
├── criacao.sh              # Script Azure CLI
└── README.md               # Este arquivo
```

---

## ⚠️ Importante — Remoção da VM

Ao final da demonstração, a VM foi removida conforme exigido pelo professor:

```bash
az group delete --name rg-FutureVet --yes --no-wait
```

> Print da remoção disponível no PDF de entrega.

---

*FIAP — 2026 | Turma 2TDS | DevOps Tools & Cloud Computing*

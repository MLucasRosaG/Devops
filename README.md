# Projeto DevOps: Pipeline CI/CD com Docker e GitHub Actions

Este é um projeto prático focado em conceitos modernos de **DevOps**, originalmente concebido durante o evento de tecnologia **E-Info** e recentemente resgatado, refatorado e migrado para uma infraestrutura 100% independente, automatizada e open-source.

O objetivo principal é demonstrar a eliminação do clássico problema *"na minha máquina funciona"*, encapsulando uma aplicação web em um container e criando uma esteira automatizada que testa, valida e distribui o software de forma contínua.

---

## Arquitetura e Pilares do Projeto

O ecossistema do projeto é dividido em duas frentes integradas:

1. **A Aplicação Autossuficiente:** Uma aplicação web escrita em **Python/Flask** rodando sobre o servidor de produção de alta performance **Gunicorn**, totalmente isolada por um ambiente **Docker** leve (`python:3.9-slim`).
2. **A Esteira Automatizada (CI/CD Pipeline):** Uma pipeline construída via **GitHub Actions** que monitora o repositório. A cada `git push`, ela valida o código usando testes unitários com **PyTest** (Integração Contínua) e, se tudo passar, compila a imagem Docker e a envia com tags rastreáveis para o **Docker Hub** (Entrega Contínua).

---

## Anatomia dos Arquivos

* `main.py`: O código-fonte da nossa aplicação web Flask.
* `requirements.txt`: O manifesto que gerencia e trava as versões das dependências do ecossistema.
* `Dockerfile`: A receita de infraestrutura como código que o Docker lê para buildar o ambiente isolado.
* `templates/`: A camada visual da aplicação, renderizada dinamicamente pelo interpretador do Flask.
* `.github/workflows/main.yml`: O cérebro do pipeline de automação do GitHub Actions.

---

## Como Rodar o Projeto Localmente (Sem o Pipeline)

Se você deseja rodar a aplicação isolada na sua máquina ou em ambientes cloud (como GitHub Codespaces ou Play with Docker), execute os passos abaixo no terminal:

### 1. Compilar a imagem Docker (O Build)
Este comando lê o `Dockerfile` e gera um pacote fechado e imutável do sistema:

docker build -t site-devops .

### 2. Iniciar o Container (O Run)
Este comando bota o sistema para rodar em segundo plano, mapeando as portas de comunicação:

docker run -d -p 8080:8080 --name container-devops site-devops

Acesse o site digitando http://localhost:8080 no seu navegador

### 3. Interromper a Execução
Para desligar o container vivo de forma segura, execute:

docker stop container-devops

Configuração do Pipeline Automático (CI/CD)
Para fazer a esteira do GitHub Actions funcionar integrada ao seu perfil do Docker Hub, siga estes passos de configuração:

## 1. Criar as Chaves de Acesso no GitHub
Não escreva suas senhas direto no código. Vá até o seu repositório no GitHub em Settings > Secrets and variables > Actions > New repository secret e cadastre as duas variáveis abaixo:

DOCKERHUB_USERNAME: Insira o seu usuário do Docker Hub.

DOCKERHUB_TOKEN: Insira o Token de Acesso gerado no painel do seu Docker Hub (Account Settings > Security > New Access Token).

##  2. O Fluxo Automatizado em Ação
Agora, o processo manual foi eliminado. O ciclo de vida do desenvolvimento resume-se a:

Você altera o código (ex: muda uma mensagem no main.py).

Executa o envio para o Git:

   git add .
   git commit -m "feat: atualiza mensagem do título"
   git push

O GitHub Actions acorda na nuvem, ativa uma máquina Linux virtual isolada, instala as dependências, roda os testes unitários (pytest) e, recebendo o sinal verde, faz o build automático e o push da imagem para o Docker Hub com duas tags: latest e a Hash única do commit do Git ($COMMIT_SHA) para garantir o versionamento de elite.

# Aprendizados Consolidados
Infraestrutura como Código (IaC): Domínio sobre o ciclo de vida do Docker, reduzindo drasticamente o tamanho das imagens com distribuições slim.

Qualidade de Software Ativa: Implementação de barreiras de qualidade (testes automatizados) que impedem que códigos quebrados cheguem ao ambiente de distribuição.

Cultura DevOps: Entendimento prático de esteiras de automação, conectando repositórios de código a registros de imagens na nuvem sem qualquer intervenção humana.
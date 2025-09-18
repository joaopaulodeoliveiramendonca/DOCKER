Documentação do meu aprendizado em Docker, com foco em Containers e Swarm. Do zero ao avançado: fundamentos, criação de containers, orquestração com Docker Swarm, volumes, redes, multi-containers, Docker Compose e práticas recomendadas. Todo código comentado para aprendizado prático.

**Docker:** é uma plataforma de contêineres de código aberto que permite
criar, distribuir e executar aplicativos de forma consistente e
confiável em qualquer ambiente, seja local ou na nuvem. Ele funciona
empacotando um aplicativo e suas dependências em um contêiner, uma
unidade isolada que garante a execução do software independentemente da
infraestrutura. Isso simplifica o desenvolvimento, teste e implantação
de aplicações, pois o mesmo contêiner sempre funcionará da mesma
maneira.

**Virtualização:** é o processo de criar uma versão virtual de um recurso, como servidores, armazenamento ou redes. Isso permite rodar vários sistemas operacionais em uma única máquina física.

**Containers:** são unidades leves de execução que permitem empacotar uma aplicação junto com todas as suas dependências, garantindo que ela rode de forma consistente em qualquer ambiente. Containers utilizam o sistema operacional de base (em vez de uma virtualização completa), tornando-os mais eficientes e rápidos do que as máquinas virtuais tradicionais.

# Instalação do Docker em Sua Máquina  

**Linux**: A instalação pode ser feita com o comando:  

```bash
sudo apt install docker.io
```

- **Windows** e **macOS**: Você pode baixar o Docker Desktop diretamente do [site oficial do Docker](https://www.docker.com/get-started/).

# Comandos Básicos do Docker  

Verificar versão do Docker 

```bash
docker \--version
```

Informações sobre o Docker

```bash
docker info
```

Puxar uma imagem do Docker Hub

```bash
docker pull nome_da_imagem
```

Rodar um container

```bash
docker run nome_da_imagem
```

Listar containers em execução

```bash
docker ps
```

Parar um container

```bash
docker stop id_do_container
```

Remover um container

```bash
docker rm id_do_container
```

Listar imagens 

```bash 
docker images
```

Remover uma imagem  

```bash
docker rmi nome_da_imagem
```

# Compreendendo Imagens e Containers  

    - **Imagens**: São como um molde para containers. Elas contêm tudo o
      > que é necessário para rodar uma aplicação (código, bibliotecas,
      > dependências, etc.).

    - **Containers**: São instâncias em execução de uma imagem. Eles são
      > criados a partir de imagens e podem ser executados, parados,
      > removidos, etc.

5.  **Dockerfile - Construindo Imagens Personalizadas  
    > **

Um Dockerfile é um arquivo de texto com uma lista de instruções para
criar uma imagem Docker. Exemplo básico:  
  
Usando uma imagem base

```bash 
FROM ubuntu:20.04
```

Instalando dependências

```bash 
RUN apt-get update && apt-get install -y python3
```

Definindo o diretório de trabalho

```bash 
WORKDIR /app
```

Copiando arquivos

```bash 
COPY . /app
```

Comando para rodar a aplicação

```bash
CMD \[\"python3\", \"app.py\"\]
```
- 

# Comando Básico para Construir Imagens  

Para construir uma imagem a partir de um Dockerfile, você pode usar:

```bash
docker build -t nome_da_imagem .
```

# Prática

Crie e execute containers simples usando a linha de comando:

1.  Execute o comando docker run hello-world.

Puxe uma imagem do Docker Hub e execute o container: 

```bash
docker run -d --name meu_container nginx
```

2.  Liste os containers em execução com docker ps e pare o container com docker stop meu_container.

# Gerenciamento de Containers e Imagens

Aprofundar-se em como gerenciar containers e imagens.

## Diferença entre Containers em Execução e Containers Parados

**Containers em Execução**: São containers que estão rodando no momento. Você pode verificar quais containers estão em execução com o comando docker ps.

**Containers Parados**: São containers que foram iniciados, mas não estão em execução no momento. Para listar os containers parados (além dos em execução), use o comando docker ps -a.

## Visualizando Logs de Containers  

O Docker permite visualizar os logs de containers em execução, o que é útil para depuração e monitoramento. Para ver os logs, use:  

```bash 
docker logs \<id_do_container\>
```bash

Você também pode seguir os logs em tempo real usando:  

```bash 
docker logs -f <id_do_container>
```

## Usando Volumes para Persistência de Dados  

**Volumes** são usados para persistir dados fora do ciclo de vida de um container. Isso é importante, pois se um container for removido, seus dados podem ser perdidos.

Para criar e montar um volume em um container, use:  

```bash  
docker volume create meu_volume

docker run -v meu_volume:/dados \<nome_da_imagem\>
```

Para listar os volumes existentes:  

```bash
docker volume ls
```

## Trabalhando com Redes no Docker  

O Docker cria uma rede padrão para containers, mas é possível criar redes personalizadas.

Para criar uma nova rede:  

```bash
docker network create minha_rede
```

Para rodar um container em uma rede específica:  

```bash
docker run \--network=minha_rede \<nome_da_imagem\>
```

Para listar as redes:  

```bash
docker network ls
```

## Criando e Usando Dockerfiles para Personalizar Imagens  

O Dockerfile permite que você personalize suas imagens, como adicionar pacotes, configurar variáveis de ambiente e definir o comportamento de execução.

Exemplo de Dockerfile mais avançado:  
  
Usando imagem base do Python

```bash
FROM python:3.8-slim
```

Instalando dependências

```bash
RUN pip install \--no-cache-dir Flask
```

Copiando o código da aplicação

```bash
COPY app.py /app.py
```

Expondo a porta

```bash
EXPOSE 5000
```

Comando para rodar a aplicação

```bash
CMD \[\"python\", \"app.py\"\]
```

## Otimização de Imagens: Layers e Cache  

As imagens Docker são compostas de camadas, cada uma representando uma instrução do Dockerfile. Quando você faz mudanças em uma instrução do Dockerfile, o Docker tenta reutilizar as camadas anteriores que não foram alteradas. Isso ajuda a otimizar o tempo de construção e o armazenamento de imagens.

## Prática

Crie um **volume** e execute um container que utilize esse volume para
persistir dados:  

```bash
docker volume create meu_volume
docker run -v meu_volume:/dados busybox touch /dados/teste.txt
```

1.  Verifique se o arquivo teste.txt foi criado no volume.

2.  **Crie um Dockerfile** para uma aplicação simples e construa a
    > imagem a partir dele:

3.   Crie um arquivo Dockerfile com o conteúdo acima.

Construa a imagem:  

```bash
docker build -t minha_imagem .
```

4.  Execute um container baseado nessa imagem:

```bash
docker run -d -p 5000:5000 minha_imagem
```

# Docker Compose

Aprender a orquestrar containers com Docker Compose.

O que é Docker Compose e Por que Usá-lo? 

**Docker Compose:** é uma ferramenta que permite definir e gerenciar multi-containers Docker. Em vez de iniciar containers individualmente com o comando docker run, você pode usar o Docker Compose para configurar e orquestrar múltiplos containers que fazem parte de uma aplicação.

Usando o Docker Compose, você pode definir todos os containers em um único arquivo YAML (docker-compose.yml), facilitando o gerenciamento de dependências, redes e volumes.

## Estrutura de Arquivo docker-compose.yml  

O arquivo docker-compose.yml é o coração do Docker Compose. Ele define os containers, suas configurações, redes e volumes.

Exemplo básico de docker-compose.yml para rodar uma aplicação web e um banco de dados: 

```yaml
version: \'3\'

services:

web:

image: nginx

ports:

\- \"8080:80\"

db:

image: postgres

environment:

POSTGRES_USER: user

POSTGRES_PASSWORD: password
```

## Comandos Principais do Docker Compose

**Subir os containers definidos no docker-compose.yml**:  

 ```bash
docker-compose up
```

**Subir os containers em modo detach (em segundo plano)**:  

```bash
docker-compose up -d
```

**Parar os containers**:  

```bash
docker-compose down
```

**Verificar logs dos containers**:  

```bash
docker-compose logs
```

**Recriar os containers (caso haja alterações nos arquivos)**: 

```bash
docker-compose up \--build
```

**Variáveis de Ambiente em Compose**

Você pode usar variáveis de ambiente no seu docker-compose.yml para tornar a configuração mais flexível e segura.

Exemplo de uso de variáveis de ambiente:  

```yaml
version: \'3\'

services:

db:

image: postgres

environment:

POSTGRES_USER: \${DB_USER}

POSTGRES_PASSWORD: \${DB_PASSWORD}
```

As variáveis DB_USER e DB_PASSWORD podem ser definidas em um arquivo .env ou como variáveis de ambiente no seu sistema.

# Prática

Crie um arquivo docker-compose.yml para rodar uma aplicação web simples com banco de dados.

Exemplo para rodar uma aplicação web (usando o nginx) e um banco de dados (PostgreSQL):  

```yaml
version: \'3\'

services:

web:

image: nginx

ports:

\- \"8080:80\"

db:

image: postgres

environment:

POSTGRES_USER: user

POSTGRES_PASSWORD: password
```

Use o comando para iniciar os containers: 

```bash
docker-compose up -d
```  

Verifique se a aplicação está acessível na URL [[http://localhost:8080](http://localhost:8080).

Para parar e remover os containers, execute:  

```bash
docker-compose down
```

# Docker Swarm (Orquestração de Containers)

Compreender a orquestração de containers usando Docker Swarm.

O que é Docker Swarm? Diferença entre Docker Swarm e Kubernetes

**Docker Swarm** é uma ferramenta nativa do Docker para orquestração de containers. Ele permite que você agrupe vários hosts Docker em um cluster e execute containers de forma distribuída e escalável.

**Diferença entre Docker Swarm e Kubernetes**: Ambos são ferramentas de orquestração de containers, mas o Kubernetes é mais complexo e oferece mais funcionalidades, enquanto o Docker Swarm é mais simples de configurar e usar, ideal para ambientes menores e para quem já está familiarizado com o Docker.

**Criando um Docker Swarm Cluster** Para iniciar um cluster Docker Swarm, você precisa configurar um **nó gerente** (manager node) e, se necessário, adicionar **nós trabalhadores** (worker nodes).

**Inicializar o Swarm** (no nó gerente):  

```bash  
docker swarm init
```

Isso retornará um comando com um token de adesão que você usará para adicionar nós trabalhadores. Exemplo de comando para adicionar um nó trabalhador:  

```bash
docker swarm join \--token \<token\> \<ip_do_nó_gerente\>:2377
```

**Comandos Básicos do Docker Swarm**

**Visualizar o status do Swarm**:  

```bash
docker node ls
```

**Iniciar um serviço no Swarm**:  
  
docker service create \--name meu_serviço \--replicas 3 nginx

- 

**Escalonar o número de réplicas de um serviço**:

```bash
docker service scale meu_serviço=5
```

**Visualizar serviços em execução**:  

```bash
docker service ls
```


**Verificar os logs de um serviço**:  

```bash
docker service logs meu_serviço
``` 

**Parar o Swarm**:  

```bash
docker swarm leave \--force
```

**Escalonamento e Gerenciamento de Serviços no Swarm**

O Docker Swarm permite escalonar facilmente os serviços para aumentar ou diminuir a quantidade de réplicas (instâncias de containers) de um serviço.

Para **escalonar** um serviço para 5 réplicas:

```bash
docker service scale meu_serviço=5
```

**Implementando Redes e Volumes no Swarm** O Docker Swarm suporta redes e volumes compartilhados entre os nós do cluster.

Para criar uma rede para serviços no Swarm:  

```bash
docker network create \--driver overlay minha_rede
```

Para criar um volume no Swarm:  

```bash
docker volume create meu_volume
```

# Prática

**Configuração do Swarm Cluster**

Inicialize o Docker Swarm no nó principal com:  

```bash
docker swarm init
```

No nó secundário, execute o comando de adesão fornecido pelo nó principal para adicionar o nó trabalhador.

**Implantando um Serviço no Swarm**

Crie um serviço no Swarm para rodar 3 réplicas do Nginx:  

 ```bash 
docker service create \--name meu_serviço \--replicas 3 nginx
```

Verifique o status do serviço:

```bash
docker service ls
```

**Escalonando o Serviço**

Aumente o número de réplicas do serviço para 5:  

```bash
docker service scale meu_serviço=5
```

**Verificando as Redes no Swarm**

Crie uma rede overlay e inicie o serviço com a rede personalizada:  

```bash  
docker network create \--driver overlay minha_rede

docker service create \--name meu_serviço \--replicas 3 \--network
minha_rede nginx
```

**Removendo o Cluster Swarm**

Para sair do Swarm em um nó: 

```bash
docker swarm leave \--force
```

## Docker em Ambientes de Produção

Preparar containers para ambientes de produção e seguir boas práticas.

**Como Otimizar a Performance de Containers**

**Imagens Menores**: Use imagens leves, como as imagens base alpine sempre que possível. Imagens menores reduzem o tempo de download, o uso de armazenamento e aumentam a segurança.

Exemplo: Ao invés de usar uma imagem do Ubuntu, use

```bash
python:3.8-alpine para aplicações Python.
```

**Limitar o Uso de Recursos**: Você pode limitar o uso de CPU e memória
dos containers com as opções \--memory e \--cpus:

```bash
docker run \--memory=\"512m\" \--cpus=\"1.0\" my_image
```

**Usar Docker Compose em Produção:** Docker Compose não é apenas para ambientes de desenvolvimento. Com configurações adequadas, pode ser usado também para gerenciar containers de produção.

**Monitoramento de Containers** Para garantir que os containers estão funcionando corretamente em produção, é essencial implementar um sistema de monitoramento. Algumas ferramentas populares para monitoramento de Docker incluem:

**Prometheus**: Coleta e armazena métricas.

**Grafana**: Visualiza as métricas coletadas pelo Prometheus.

Você pode integrar o Prometheus aos seus containers para coletar métricas de desempenho.

**Segurança no Docker**

**Usar Imagens Oficiais e Confiáveis**: Sempre prefira imagens oficiais, que têm uma maior garantia de segurança.

**Evitar Rodar Containers como Root**: Não execute containers como root, a menos que seja absolutamente necessário. Use o flag USER no Dockerfile para definir um usuário não-root:  

```bash  
USER nobody
``` 

**Escaneamento de Imagens**: Use ferramentas como o **Clair** ou o **Trivy** para escanear imagens Docker em busca de vulnerabilidades de segurança.

**Limitar o Acesso de Rede**: Use redes Docker personalizadas para restringir o acesso entre containers e a internet, limitando a superfície de ataque.

# Deploy de Containers em Servidores

**Docker em Servidores**: Em um ambiente de produção, você provavelmente estará executando containers em servidores (físicos ou na nuvem). Para fazer deploy, você pode usar ferramentas como:

**Docker Swarm**: Já vimos como configurar um cluster de Swarm.

**Docker Compose**: Para orquestrar múltiplos containers em um único servidor.

**Ferramentas de Deploy Automático**: Integrar Docker com ferramentas como **Jenkins**, **GitLab CI** ou **GitHub Actions** permite o deploy automático de containers sempre que o código é atualizado.

## Prática

**Otimização de Imagens**

Crie uma imagem de aplicação com base na imagem alpine para manter o tamanho da imagem pequeno. Exemplo:  

```bash  
FROM python:3.8-alpine

COPY . /app

RUN pip install -r /app/requirements.txt

CMD \[\"python\", \"/app/app.py\"\]
```

Construa e execute o container com a imagem otimizada.

**Monitoramento de Containers**

Instale o **Prometheus** e **Grafana** em um container para monitorar os recursos dos containers Docker. Use o comando docker stats para visualizar o consumo de recursos dos containers em tempo real.

**Segurança e Escaneamento**

Utilize a ferramenta **Trivy** para escanear imagens em busca de vulnerabilidades:  

```bash  
trivy image my_image
```

**Deploy em Produção com Docker Compose**

Crie um arquivo docker-compose.yml com múltiplos serviços para rodar em
um servidor de produção. Exemplo:  

```yaml
version: \'3\'

services:

web:

image: my_web_image

ports:

\- \"80:80\"

db:

image: postgres

environment:

POSTGRES_USER: user

POSTGRES_PASSWORD: password
```

Faça o deploy com:

```bash
docker-compose up -d.
```

# Integração com CI/CD (Integração Contínua e Entrega Contínua)**

Entender como integrar Docker com pipelines de CI/CD.

**O que é CI/CD e como Docker se Encaixa Nisso**

**CI (Integração Contínua)**: Refere-se ao processo de integrar o código de todos os desenvolvedores em um repositório compartilhado várias vezes ao dia. Com isso, as alterações de código podem ser verificadas e testadas automaticamente.

**CD (Entrega Contínua)**: Refere-se à prática de entregar o código de maneira contínua para produção ou para um ambiente de testes. O Docker facilita esse processo, pois permite criar ambientes consistentes em qualquer lugar.

**Como o Docker Ajuda no CI/CD**: Com Docker, você pode garantir que os testes e o deploy sejam feitos em um ambiente idêntico ao de produção. Isso reduz a possibilidade de erros relacionados a diferenças de ambiente.

**Configurando Pipelines de CI/CD com Docker**

**GitHub Actions**: GitHub Actions é uma ferramenta de automação que permite criar pipelines diretamente dentro do GitHub.

**Exemplo de Workflow no GitHub Actions**:  

Crie um arquivo .github/workflows/ci.yml:  

```yaml   
name: CI/CD Pipeline

on:

push:

branches:

\- main

jobs:

build:

runs-on: ubuntu-latest

steps:

\- uses: actions/checkout@v2

\- name: Configurar Docker Buildx

uses: docker/setup-buildx-action@v1

\- name: Fazer o Build da Imagem Docker

run: \|

docker build -t my_image .

\- name: Rodar Testes em Docker

run: \|

docker run my_image pytest tests/

\- name: Enviar a Imagem para Docker Hub

run: \|
```

```bash 
docker login -u \$DOCKER_USERNAME -p \$DOCKER_PASSWORD

docker push my_image
``` 

**GitLab CI**: GitLab CI também tem suporte nativo para Docker. Você
pode criar um arquivo .gitlab-ci.yml para definir um pipeline:  
  
stages:

```yaml   
\- build

\- test

\- deploy

build:

stage: build

script:

\- docker build -t my_image .

test:

stage: test

script:

\- docker run my_image pytest tests/

deploy:

stage: deploy

script:

\- docker login -u \$DOCKER_USERNAME -p \$DOCKER_PASSWORD

\- docker push my_image
```

# Testando Containers em um Pipeline CI/CD

Durante a pipeline, depois de construir a imagem, é possível rodar testes dentro de um container. Isso garante que a aplicação foi criada corretamente e funciona conforme esperado.

Exemplo de teste em uma aplicação Python:  

```bash   
docker run my_image pytest tests/
```

**Deploy Automatizado de Containers**

Após os testes passarem com sucesso, você pode automatizar o deploy da aplicação para produção.

No GitHub Actions, por exemplo, você pode fazer o deploy para um servidor de produção com Docker. O workflow pode incluir um passo de deploy como:  
  
name: Deploy to Production Server

```bash 
run: \|

ssh user@server_ip \"docker pull my_image && docker-compose up -d\"
```

## Prática

**Configuração de um Pipeline CI/CD com GitHub Actions**

Crie um repositório no GitHub e adicione um arquivo .github/workflows/ci.yml.

No pipeline, defina as etapas para construir, testar e enviar a imagem para um registry (ex: Docker Hub).

Exemplo de configuração de GitHub Actions para uma aplicação que utiliza Docker:  

```yaml 
name: Docker CI/CD Pipeline

on:

push:

branches:

\- main

jobs:

build:

runs-on: ubuntu-latest

steps:

\- uses: actions/checkout@v2

\- name: Build Docker Image

run: \|

docker build -t my_image .

\- name: Run Tests

run: \|

docker run my_image pytest tests/

\- name: Push Docker Image

run: \|
```

```bash 
docker login -u \$DOCKER_USERNAME -p \$DOCKER_PASSWORD

docker push my_image
```

## Testando o Pipeline  

Faça um push para o repositório e verifique se o workflow de CI/CD é executado corretamente.

**Automatizando o Deploy**

Após o build e teste, adicione um estágio de deploy no seu pipeline para enviar a nova imagem para o servidor de produção, como mostrado acima.

# Tópicos Avançados e Estratégias de Escalabilidade

Aprender práticas avançadas para escalar e otimizar containers.

**Clusterização e Balanceamento de Carga com Docker Swarm**

**Escalonamento de Serviços**: Docker Swarm permite que você escale facilmente os serviços adicionando ou removendo réplicas de containers em execução. Isso ajuda a garantir alta disponibilidade e balanceamento de carga.

Exemplo para escalar um serviço:  

```bash 
docker service scale meu_serviço=5
```

**Balanceamento de Carga**: O Docker Swarm automaticamente balanceia a carga entre os containers de um serviço, distribuindo o tráfego entre eles. Não é necessário configurar um balanceador de carga externo, pois o próprio Swarm gerencia isso.

**Tuning de Performance de Containers e Redes Docker**

**Memória e CPU**: Você pode alocar mais recursos de CPU e memória para containers para melhorar a performance.

Exemplo de comando para limitar recursos:  

```bash 
docker run \--memory=\"2g\" \--cpus=\"2.0\" my_image
```

**Redes Docker**: Para comunicação eficiente entre containers, use redes personalizadas. Isso pode melhorar o desempenho, além de aumentar a segurança.

Criando uma rede personalizada:  

```bash 
docker network create \--driver overlay minha_rede
```

**Como Utilizar Docker em Grandes Escalas**

Para ambientes de produção em larga escala, você pode usar o Docker em conjunto com ferramentas como Kubernetes ou Docker Swarm para gerenciar containers distribuídos.

**Kubernetes**: Embora o Docker Swarm seja suficiente para orquestrar containers em ambientes menores, o Kubernetes é mais adequado para escalabilidade em larga escala. Ele oferece recursos como auto-escalonamento, gerenciamento de clusters e deploy de containers com alta disponibilidade.

**Estratégias de Backup e Recuperação**

**Backup de Volumes**: Como os volumes Docker são onde você armazena dados persistentes, é importante fazer backups regulares.

Para fazer backup de um volume, você pode usar o comando:  

```bash 
docker run \--rm \--volumes-from meu_container -v \$(pwd):/backup ubuntu
tar cvf /backup/backup.tar /dados
```

**Backup de Imagens**: É uma boa prática fazer backup das imagens Docker, especialmente em ambientes de produção.

Exemplo de comando para salvar uma imagem:  

```bash 
docker save -o minha_imagem.tar my_image
```

**Monitoramento e Logging Avançado**

**Monitoramento com Prometheus**: Para grandes ambientes de produção, é importante monitorar os containers de forma eficaz.

**Prometheus** coleta métricas sobre o desempenho dos containers e do sistema. Você pode configurar containers para expor métricas que o Prometheus coleta e envia para um dashboard Grafana.

**Centralização de Logs**: Em vez de acessar logs de containers individualmente, use uma solução de centralização de logs, como **ELK Stack (Elasticsearch, Logstash, Kibana)** ou **Fluentd**.

Isso permite visualizar e analisar logs de todos os containers em um único lugar.

## Prática

**Escalando um Serviço no Docker Swarm**

Crie um cluster Docker Swarm com pelo menos dois nós (gerente e trabalhador).

Crie um serviço no Swarm e escalone o número de réplicas para 5:

```bash 
docker service create \--name meu_serviço \--replicas 3 nginx

docker service scale meu_serviço=5
```

**Configurando Recursos para Containers**

Limite o uso de memória e CPU para um container:  

```bash 
docker run \--memory=\"2g\" \--cpus=\"1.5\" my_image
```

**Criando uma Rede Overlay no Docker**

Crie uma rede overlay para conectar containers em um cluster Docker Swarm:  

```bash 
docker network create \--driver overlay minha_rede
```

**Backup de Volumes**

Faça backup de um volume Docker:  

```bash
docker run \--rm \--volumes-from meu_container -v \$(pwd):/backup ubuntu
tar cvf /backup/backup.tar /dados
```

**Monitoramento com Prometheus** Configure o **Prometheus** para coletar métricas dos containers. Configure o **Grafana** para visualizar essas métricas.

# Revisão e Projetos Práticos

Consolidar o aprendizado com projetos práticos.

**Revisão dos Conceitos**

Ao longo das semanas anteriores, você aprendeu sobre:

**Docker e Containers**: Entendeu o conceito de containers, como criar e gerenciar imagens e containers, além de explorar o Docker Compose e o Docker Swarm.

**Docker Compose**: Viu como orquestrar múltiplos containers e configurar redes e volumes para facilitar o gerenciamento de aplicações complexas.

**Docker Swarm**: Aprendeu a criar clusters, escalonar serviços, configurar redes e volumes no Swarm, e compreender a alta disponibilidade e balanceamento de carga.

**Ambientes de Produção e CI/CD**: Entendeu como otimizar imagens, fazer deploy automatizado com Docker e configurar pipelines CI/CD para integração e entrega contínua.

**Escalabilidade e Monitoramento**: Viu como escalar containers, configurar redes personalizadas, fazer backups, e utilizar ferramentas de monitoramento como Prometheus e Grafana.

**Desenvolvendo um Projeto Completo** A melhor maneira de solidificar os conceitos é criar um projeto do início ao fim, aplicando o que foi aprendido em situações do mundo real.

**Exemplo de Projeto**: Vamos criar uma aplicação web com frontend e backend, utilizando Docker para empacotar os serviços e Docker Compose para orquestrar o ambiente.

# Projeto Prático: Aplicação Web com Frontend e Backend

Criar uma aplicação de microserviços com Docker. O backend será em **Node.js** e o frontend será em **React**. Ambos serão containerizados e orquestrados com Docker Compose. O backend se conectará a um banco de dados **PostgreSQL**.

## Passos do Projeto

**Criar o Backend em Node.js**

Crie um diretório backend e inicialize um projeto Node.js. 

```bash 
mkdir backend

cd backend

npm init -y

npm install express pg
```

Crie um arquivo server.js para o servidor Express que conecta ao banco
de dados PostgreSQL:

```javascript
const express = require(\'express\');

const { Client } = require(\'pg\');

const app = express();

const port = 5000;

const client = new Client({

host: \'db\',

port: 5432,

user: \'user\',

password: \'password\',

database: \'mydb\',

});

client.connect();

app.get(\'/\', (req, res) =\> {

client.query(\'SELECT NOW()\', (err, result) =\> {

if (err) {

res.send(err);

} else {

res.send(\`Current Time: \${result.rows\[0\].now}\`);

}

});

});

app.listen(port, () =\> {

console.log(\`Backend running on port \${port}\`);

});
```

**Criar o Frontend em React**

Crie o diretório frontend e inicialize um projeto React.  

```bash 
npx create-react-app frontend
```

Modifique o componente App.js para fazer uma requisição ao backend:  

```javascript
import React, { useState, useEffect } from \'react\';

function App() {

const \[time, setTime\] = useState(\'\');

useEffect(() =\> {

fetch(\'http://localhost:5000/\')

.then((response) =\> response.text())

.then((data) =\> setTime(data));

}, \[\]);

return (

\<div\>

\<h1\>Current Time: {time}\</h1\>

\</div\>

);

}

export default App;
```

**Criar o Dockerfile para o Backend**

Crie um Dockerfile no diretório backend:  

```bash
FROM node:14

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 5000

CMD \[\"node\", \"server.js\"\]
```

**Criar o Dockerfile para o Frontend**

Crie um Dockerfile no diretório frontend:  

```bash
FROM node:14

WORKDIR /app

COPY . .

RUN npm install

RUN npm run build

EXPOSE 3000

CMD \[\"serve\", \"-s\", \"build\"\]
```

**Configuração do Docker Compose**

Crie um arquivo docker-compose.yml para orquestrar o frontend, backend e
o banco de dados:

```yaml  
version: \'3\'

services:

backend:

build: ./backend

ports:

\- \"5000:5000\"

depends_on:

\- db

frontend:

build: ./frontend

ports:

\- \"3000:3000\"

db:

image: postgres

environment:

POSTGRES_USER: user

POSTGRES_PASSWORD: password

POSTGRES_DB: mydb

volumes:

\- postgres_data:/var/lib/postgresql/data

volumes:

postgres_data:
```

**Rodando o Projeto com Docker Compose **

Na raiz do projeto (onde o docker-compose.yml está localizado),
execute:  

```bash 
docker-compose up \--build
```

O Docker Compose irá construir as imagens e rodar os containers para o frontend, backend e banco de dados. O backend estará acessível em [http://localhost:5000](http://localhost:5000), e o frontend estará disponível em [http://localhost:3000](http://localhost:3000).

## Conclusão do Projeto

Após a execução do projeto, você terá uma aplicação funcional com Docker, usando Docker Compose para orquestrar o ambiente de desenvolvimento. Esse tipo de projeto é uma boa prática para solidificar o que você aprendeu e testá-lo em um ambiente mais complexo.

## Prática Adicional

Agora que você tem a base de um projeto, tente adicionar funcionalidades extras como autenticação, comunicação com outras APIs, ou até mesmo configurar um pipeline de CI/CD para o seu projeto com GitHub Actions ou GitLab CI.

Com isso, você tem um plano completo de estudos sobre Docker, abrangendo desde os conceitos básicos até práticas avançadas e integração com CI/CD, além de um projeto prático para consolidar seu aprendizado.


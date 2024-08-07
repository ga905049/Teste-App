### Pré-requisitos<a name="prerequisites"></a>

- Docker
- Linux (de preferência, mas não obrigatório)

### Build local<a name="build"></a>

Para você se familiarizar com a aplicação, utilizamos Docker para simplificar o build e deploy do frontend e backend na sua máquina. A partir do seu terminal, execute os seguintes comandos:

```bash
cd cloud-engineer-test-app
```

```bash
docker build -t teste-backend -f Dockerfile.backend ./backend
```

```bash
docker build -t teste-frontend -f Dockerfile.frontend ./frontend
``` 

### Deploy local<a name="deploy"></a>

```bash
docker run -d -p5500:5500 teste-backend
```

```bash
docker run -d -p3000:3000 teste-frontend
``` 

Para verificar se deu certo, rode o comando `docker ps`. Resultado deve ser parecido com esse:

```bash
❯ docker ps
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS                                       NAMES
6ac59bbf1ffb   teste-backend    "docker-entrypoint.s…"   20 minutes ago   Up 20 minutes   0.0.0.0:5500->5500/tcp, :::5500->5500/tcp   hardcore_mccarthy
730c6eabc5cd   teste-frontend   "docker-entrypoint.s…"   23 minutes ago   Up 23 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   wizardly_pasteur
```

No seu navegador, acesse `http://localhost:3000`, você deverá visualizar a seguinte tela:

<p align="center">
  <a href="http://deepesg.com/" target="blank"><img src="https://snipboard.io/tnBXDd.jpg" /></a>
</p>

### CI/CD

O arquivo principal que configura a pipeline é o **app.yml** que está em **.github/workflows**. Para executar a pipeline basta fazer um push para a branch main.

É necessário configurar as seguintes secrets no github:

- SSH_KEY_BASTION: Chave privada para acesso a instância do Bastion.
- SSH_HOST_BASTION: IP da instância do Bastion.
- SSH_USER_BASTION: Usuário de acesso a instância do Bastion.
- SSH_KEY_DOCKER_SERVER: Chave privada para acesso a instância do Docker.
- SSH_HOST_DOCKER_SERVER: IP da instância do Docker.

A pipeline possui os seguintes steps:

- Configure SSH: Configura o SSH para acesso ao bastion.
- Deploy: Realiza o deploy fazendo acesso ssh ao bastion e depois à instância do Docker e realizar o passos de build e start dos containers.
- Test App: Realiza acesso ssh ao bastion e depois à instância do Docker e executa um teste de Curl para verificar se a aplicação esta respondendo 200.


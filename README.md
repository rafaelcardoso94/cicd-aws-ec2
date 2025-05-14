# 🚀 Continuous Delivery Workflow
Este repositório utiliza GitHub Actions para realizar um pipeline de entrega contínua (CD) que constrói uma imagem Docker, publica no Amazon ECR e executa o deploy via SSH em uma instância EC2.

## 🔁 Fluxo do Workflow
O workflow é acionado automaticamente a cada push na branch main.

## _🛠️ 1. Build_
Executado em ubuntu-latest:

Checkout do código:

```yaml
- uses: actions/checkout@v3
```
`Geração de Tag Geral:`
Cria uma variável de ambiente GENERAL_TAG no formato YYYYMMDD_RUN_NUMBER.

`Autenticação na AWS:`
Usa as credenciais armazenadas em secrets para configurar o acesso à AWS.

`Login no Amazon ECR:`
Faz login no repositório de container da AWS.

`Build e Push da Imagem Docker:`
A imagem é construída e enviada ao ECR com a tag GENERAL_TAG.

## _🚀 2. Deploy_
Executado após a finalização do job de build:

`Checkout do código`
`Regeração da Tag Geral (a mesma do build)`

`Deploy via SSH:`
Utiliza appleboy/ssh-action para conectar na instância EC2 e executar o script scripts/deploy.sh, passando a tag como variável de ambiente.

## _🔐 Segredos Necessários_
Certifique-se de configurar os seguintes Secrets no repositório:
```bash
$AWS_ACCESS_KEY_ID
$AWS_SECRET_ACCESS_KEY
$SSH_PRIVATE_KEY
```
## _🐳 Pré-requisitos_
Repositório Amazon ECR criado e com permissões apropriadas.
Instância EC2 acessível via SSH e com Docker configurado.
Script scripts/deploy.sh presente no repositório, responsável por puxar a imagem e atualizar a aplicação.
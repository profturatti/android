# Criando uma API do zero

mkdir API
cd API
npm init -y          <- vai criar o package.json com a estrutura básica vazia
   
npm install express  <- vai instalar a biblioteca express (que oferece recursos web)

Saiba mais em: expressjs.com

Neste momento são criados os seguintes conteúdos:
package.json         <- recebe a dependencia express em seu conteúdo
package-lock.json    <- backup do package.json
node_modules         <- diretório que armazena as bibliotecas do projeto

Vamos criar o seguinte arquivo:
server.js     <- vai ser nosso servidor

O servidor responderá a métodos HTTP como:
- GET    para listar
- POST   para criar
- PUT    para editar vários
- PATCH  para editar um
- DELETE para excluir

Como isso funciona? Requisitos

1. Tipo de rota/método HTTP
2. Endereço

no package.json adicionar depois da tag "version"
"type": "module",   <- isto permitirá o uso de import ao invés do request (descontinuada)

--server.js--
import express from 'express'

const app = express()

app.get('/', (request, response) => {
    response.send('OK! Funcionou!')
})

app.listen(3000)
---

Vamos rodar o servidor:

$ node server.js

Abra o navegador e acesse: localhost:port -> 127.0.0.1:3000

O padrão de requisições no navegador é GET. Mas como posso testar outros métodos?

Uma das formas possíveis é através da extensão: Thunder Client - Lightweight Rest API Client

Após instalada a extensão, um icone é adicionado na lateral do VSCode para criar requests e efetuar os testes desejados

A cada modificação no arquivo "server.js" é necessário reiniciar o programa para aplicar as atualizações.

Uma forma de automatizar essa atualização é chamando o programa com o parametro watch

$ node --watch server.js

Para receber parâmetros nas requisições, temos basicamente três formas:

1. Query Params (GET) - para consultas

servidor/rota/usuarios?id=1&nome=Joao

2. Route Params (GET, PUT, DELETE) - buscar, editar ou apagar algo especifico

servidor/usuarios/25/usuarios

3. Body Params (POST e PUT)

{
  "nome": "Joao",
  "id": 25
}

## Atualizando o server.js

---server.js---
import express from 'express'

const app = express()
app.use(express.json()) // permite o recebimento de conteúdo json

const users = []

app.post('/usuarios', (req, res) => {
  console.log(req.body)
  res.send('POST recebido')
})

app.get('/usuarios', (req, res) => {
  res.status(200).json(users)
})

app.get('/', (request, response) => {
    response.send('Servidor funcionando!')
})

app.listen(3000)
---

HTTP Status
- 2xx <- Sucesso
- 4xx <- Erro cliente
- 5xx <- Erro servidor

Agora vamos colocar o conteudo visualizado no post dentro do array

---server.js---
import express from 'express'

const app = express()
app.use(express.json()) // permite o recebimento de conteúdo json

const users = []

app.post('/usuarios', (req, res) => {
  users.push(req.body)
  res.send("Post recebido")
})

app.get('/usuarios', (req, res) => {
  res.status(200).json(users)
})

app.get('/', (request, response) => {
    response.send('Servidor funcionando!')
})

app.listen(3000)
---

Utilizando os estados de resposta HTTP

---server.js---
import express from 'express'

const app = express()
app.use(express.json()) // permite o recebimento de conteúdo json

const users = []

app.post('/usuarios', (req, res) => {
  users.push(req.body)
  res.status(201).json(req.body)

})

app.get('/usuarios', (req, res) => {
  res.status(200).json(users)
})

app.get('/', (request, response) => {
    response.send('Servidor funcionando!')
})

app.listen(3000)
---

Agora falta a parte de edição e deleção de dados
Vamos utilizar o mongoDB: https://www.mongodb.com/pt-br

Comece agora mesmo
Crie uma conta
Verifique seu email para ativar a conta na plataforma
No dashboard, menu a esquerda, Deployment > Database > Build a cluster
Escolha "M0 Free"
Atribua um nome: Users (por exemplo)
Provider (pode deixar AWS que é o padrão)
Region (pode deixar Sao Paulo) <- Quanto mais próximo de sua localidade real, mais rápido
Não é necessário efetuar outras configurações

Clique no botão "Create Deployment"

Se mostrar algum custo, clique para editar e refaça a configuração escolhendo "M0"

A seguir, uma janela flutuante mostrará outras configurações.
Em 1. Add a connection IP address, escolha "Allow Access from Anywhere"
Em 2. Create a database user, é informado que o usuário inicial é "atlasAdmin" e que é necessário definir um usuário e senha para acesso ao banco de dados. Preencha esses campos e faça uma cópia pra você!

$ npm install mongodb

Utilizaremos o Prisma para acessar o banco de dados

$ npm install prisma --save-dev

$ npx prisma init

Edite o arquivo ".env" e substitua a string de conexao para:

mongodb+srv://<seu_usuario:<senha>@users.svuaf.mongodb.net/<nome_do_cluster>?retryWrites=true&w=majority&appName=Users

Em prisma>schema.prisma

<screenshots>

No console

$ npx prisma db push

Ocorrendo sucesso, vamos usar o cliente do prisma

$ npm install @prisma/client

$ npx prisma studio <- abre no navegador uma visualização do banco de dados

Atualizaremos o server para usar o prisma

---server.js---
import express from 'express'
import { PrismaClient } form '@prisma/client'

const prisma = new PrismaClient()

const app = express()
app.use(express.json()) // permite o recebimento de conteúdo json

app.post('/usuarios', async (req, res) => {
  await prisma.user.create({
    data: {
      email: req.body.email.
      name: req.body.name,
      age: req.body.age
    }
  })
  res.status(201).json(req.body)
})

app.put('/usuarios/:id', async (req, res) => {
  await prisma.user.update({
    where: {
      id: req.params.id
    },
    data: {
      email: req.body.email.
      name: req.body.name,
      age: req.body.age
    }
  })
  res.status(201).json(req.body)
})

app.delete('/usuarios/:id', async (req, res) => {
  await prisma.user.delete({
    where: {
      id: req.params.id
    }
  })
  res.status(200).json({ message: 'Usuario deletado com sucesso!'})
})

app.get('/usuarios', async (req, res) => {
  const users = await prisma.user.findMany()
  res.status(200).json(users)
})

app.get('/', (request, response) => {
    response.send('Servidor funcionando!')
})

app.listen(3000)
---

Agora inserindo uma busca na listagem de usuários através de query params

---server.js---
import express from 'express'
import { PrismaClient } form '@prisma/client'

const prisma = new PrismaClient()

const app = express()
app.use(express.json()) // permite o recebimento de conteúdo json

app.post('/usuarios', async (req, res) => {
  await prisma.user.create({
    data: {
      email: req.body.email.
      name: req.body.name,
      age: req.body.age
    }
  })
  res.status(201).json(req.body)
})

app.put('/usuarios/:id', async (req, res) => {
  await prisma.user.update({
    where: {
      id: req.params.id
    },
    data: {
      email: req.body.email.
      name: req.body.name,
      age: req.body.age
    }
  })
  res.status(201).json(req.body)
})

app.delete('/usuarios/:id', async (req, res) => {
  await prisma.user.delete({
    where: {
      id: req.params.id
    }
  })
  res.status(200).json({ message: 'Usuario deletado com sucesso!'})
})

app.get('/usuarios', async (req, res) => {
  let users = []
  if (req.query) {
    users = await prisma.user.findMany({
      where: {
        name: req.query.name,
        email: req.query.email,
        age: req.query.age
      }
    })
  } else {
    users = await prisma.user.findMany()
  }

  res.status(200).json(users)
})

app.get('/', (request, response) => {
    response.send('Servidor funcionando!')
})

app.listen(3000)
---
# Projeto Extensionista - Prog. Disp. móveis com Android - ARA0089

O projeto utilizará: 

- JavaScript (React Native; NodeJS)

- EXPO (permite a visualização com Android ou iOS através do app ExpoGO)

- Banco de dados (REST API com SQLite ou PostgreSQL)

## Dúvidas

- Posso usar outro banco de dados como firebase, mySQL ou Oracle? (ou qualquer outro) `PODE`

Entretanto não receberá suporte para outros bancos de dados diferentes daqueles sugeridos

- Posso usar Flutter? `NÃO PODE`

- Posso fazer como uma página web responsiva? `NÃO PODE`

## Componentes avaliados:

### Backend utilizando REST API com SQLite ou PostgreSQL (Supabase) - NodeJS

A utilização de um backend se faz necessária considerando que seu MVP ( ) possa ser expandido para uma aplicação comercial posteriormente. Fica a critério do grupo esse tipo de decisão.

### Frontend utilizando React Native + EXPO

- Login: TODA aplicação precisa de um controle de acesso com login utilizando e-mail e senha. Serão consideradas as verificações de consistência de entrada de dados, cadastro de novos usuários, recuperação de senha e perfil do usuário onde dados adicionais podem ser cadastrados, tais como foto, nome completo, telefone para contato/whatsapp. O sistema deve possuir um controle do perfil de tal forma que após o cadastro inicial realizado pelo usuário com informações adicionais, permita somente a edição de contato e foto.

- Aplicação principal: Cada grupo tem sua solução que deverá utilizar o banco de dados (CRUD) para realizar seu propósito.

- Lembre-se que o perfil de administração poderá acessar todos os dados de todos os usuários, inclusive realizar a troca de senha se necessário e/ou bloquear ou desbloquear um usuário conforme o critério de utilização de cada aplicação.



### Banco de dados - Sugestão de controle de login:

`TABELA USUÁRIOS`


|    Campos   | Tipo de dado no banco de dados
|-------------|---
| ID          | numero, chave primaria, autoincremento
| tipoUsuario | booleano user 0 / 1 admin
| email       | varchar(50)
| senha       | varchar(30)
| mudaSenha   | booleano   nao mudar 0 / 1 mudar, usuario fará cadastro de nova senha no próximo login
| liberacao   | booleano   inativo 0 / 1 ativo

`TABELA PERFIL`

| Campos  | Tipo de dado no banco de dados
|---------|---
| ID      | numero, chave primaria, autoincremento
| userID  | numero, chave estrangeira
| nome    | varchar(50) nome completo
| contato | varchar(11) Area 2 digitos, telefone 9 digitos
| foto    | varchar() caminho para imagem salva no dispositivo

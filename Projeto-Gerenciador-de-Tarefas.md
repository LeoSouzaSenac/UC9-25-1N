## Projeto: Aplicação ToDoList com Autenticação Segura

### Contexto

Desenvolver uma aplicação desktop em Java utilizando Java Swing que implemente um sistema de autenticação e gerenciamento de tarefas pessoais. O sistema deve seguir o padrão arquitetural MVC, utilizar persistência em banco de dados relacional e aplicar criptografia de senha com BCrypt.

A proposta é construir uma aplicação simples, funcional e organizada, com foco em boas práticas de estruturação de código, separação de responsabilidades e segurança no armazenamento de credenciais.

### Objetivo

Criar um sistema no qual usuários possam se cadastrar, realizar login e gerenciar suas próprias tarefas. Cada usuário deve ter acesso apenas às tarefas vinculadas à sua conta.

A aplicação deve demonstrar domínio de:

Arquitetura MVC
CRUD completo
Integração com banco de dados
Autenticação segura com hash de senha
Organização e clareza de código

---

### Requisitos Funcionais

O sistema deve permitir cadastro de novos usuários.

O sistema deve permitir autenticação por meio de login e senha.

A senha não pode ser armazenada em texto puro no banco de dados. Deve ser utilizada a biblioteca BCrypt para gerar e validar o hash da senha.

Após autenticação bem-sucedida, o usuário deve acessar uma área restrita onde poderá gerenciar suas tarefas.

O sistema deve permitir criar, visualizar, editar e excluir tarefas.

Cada tarefa deve estar vinculada a um único usuário.

O sistema deve permitir marcar tarefas como concluídas ou não concluídas.

As tarefas exibidas devem pertencer exclusivamente ao usuário autenticado.

---

### Regras de Negócio

O nome de usuário deve ser único no sistema.

Não deve ser permitido cadastrar usuário com campos obrigatórios vazios.

Não deve ser permitido criar tarefas com título vazio.

O sistema não deve permitir que um usuário visualize, edite ou exclua tarefas que não estejam vinculadas à sua conta.

A autenticação só deve ser considerada válida se a senha informada corresponder corretamente ao hash armazenado no banco de dados.

Em caso de falha no login, o sistema não deve informar se o erro ocorreu por usuário inexistente ou senha incorreta.

Uma tarefa só pode ser marcada como concluída se estiver vinculada a um usuário autenticado.

Alterações realizadas nas tarefas devem ser persistidas imediatamente no banco de dados.

---

### Requisitos Técnicos

A aplicação deve utilizar Java Swing para construção da interface gráfica.

O projeto deve seguir o padrão MVC, mantendo clara separação entre camadas de modelo, visualização e controle.

A persistência deve ser realizada em banco de dados relacional.

O acesso ao banco deve ser organizado por meio de classes responsáveis pela comunicação com o banco, sem misturar código de interface com código de persistência.

A validação de senha deve utilizar os métodos apropriados da biblioteca BCrypt, tanto para geração quanto para verificação do hash.

O código deve estar organizado, legível e estruturado de forma coerente.

---

### Critérios de Avaliação

Correção na implementação do padrão MVC.

Funcionamento completo do fluxo de cadastro e login.

Uso adequado de hash de senha com BCrypt.

Funcionamento completo do CRUD de tarefas.

Relacionamento correto entre usuário e tarefas.

Aplicação consistente das regras de negócio.

Separação adequada entre interface, regra de negócio e acesso a dados.

Clareza, organização e qualidade do código.

---

### Observação Final

O projeto deve ser simples, mas bem estruturado. A complexidade não será avaliada pela quantidade de funcionalidades extras, e sim pela qualidade da arquitetura, pela aplicação correta dos conceitos e pela segurança na autenticação.

O foco é demonstrar maturidade técnica na construção de um sistema completo, ainda que enxuto.

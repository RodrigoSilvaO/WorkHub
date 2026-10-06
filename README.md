# WorkHub

Sistema web de gestão de tarefas colaborativo, com quadro Kanban em cards (estilo Trello), projetos compartilhados, comentários, anexos, notificações por e-mail e histórico imutável de alterações.

> Projeto da disciplina de Engenharia de Software. As funcionalidades e regras estão detalhadas no **DEF (Documento de Especificação Funcional)**.

## Funcionalidades

- Cadastro, login e recuperação de senha por e-mail (token de uso único)
- Perfil do usuário: nome, e-mail, avatar e opção de desativar notificações
- Projetos com dois papéis: **Administrador** (criador) e **Membro**
- Convite de membros por busca de e-mail
- Dashboard com os projetos do usuário e a quantidade de tarefas pendentes
- Tarefas com título, descrição, prazo e prioridade (Baixa, Média, Alta)
- Atribuição de responsáveis e opção "Assumir tarefa"
- Quadro Kanban (**A Fazer**, **Em Andamento**, **Concluído**) com drag and drop, seletor ou botão
- Filtros: Minhas Tarefas, prioridade e status
- Comentários com data e hora
- Anexos: `.pdf`, `.jpg/.png`, `.xls/.xlsx`, `.doc/.docx`, até 5 MB por arquivo
- Alertas por e-mail para tarefas que vencem em até 24 h (com opt-out no perfil)
- Histórico de alterações por tarefa (audit trail), automático e imutável

## Tecnologias

| Camada | Tecnologia |
| --- | --- |
| Front-end | CSS e JavaScript (executados no navegador) |
| Back-end | JavaScript (Node.js + Express), servindo páginas HTML e uma API REST |
| Banco de dados | MySQL |


## Pré-requisitos

- Node.js 18 ou superior
- MySQL 8 ou superior
- Conta SMTP para o envio de e-mails (em desenvolvimento, pode ser usado um serviço de teste como o Mailtrap)

## Permissões
| Ação | Administrador | Membro |
| --- | :-: | :-: |
| Editar projeto, convidar e remover membros, excluir projeto | Sim | Não |
| Ver quadro, criar tarefas, assumir tarefas, mover cards | Sim | Sim |
| Comentar e anexar arquivos | Sim | Sim |


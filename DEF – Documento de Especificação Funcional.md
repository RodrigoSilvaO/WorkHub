# Documento de Especificação Funcional (DEF)

## **0. Visão geral do projeto**

- **Nome do Software**: WorkHub
- **Versão:** 1.0
- **Data:** \[06/10/2026\]
- **Autor(es):** Alana dos Santos da Paixão, Laricia de Jesus dos Santos, Saulo Henrique da Conceição Souza, Rodrigo da Silva Oliveira, Fabiane Batista
- **Aprovado por:** André Romano Madureira

## 1. Introdução

### 1.1 Propósito

Este documento especifica as funcionalidades, regras de negócio, perfis de acesso e requisitos não funcionais do sistema de WorkHub, servindo de referência para projeto, implementação, testes e validação.

### 1.2 Escopo

Aplicação web responsiva que permite a múltiplos usuários criar projetos (espaços de trabalho), compartilhar listas de tarefas, acompanhar o progresso em quadro Kanban e colaborar por meio de comentários e anexos. O visual segue o padrão de cards, semelhante ao Trello.

### 1.3 Definições e siglas

| Termo | Definição |
| --- | --- |
| DEF | Documento de Especificação Funcional |
| RF / RNF | Requisito Funcional / Requisito Não Funcional |
| Projeto | Espaço de trabalho que reúne membros e tarefas |
| Tarefa (card) | Unidade de trabalho com título, descrição, prazo, prioridade, status e responsáveis |
| Kanban | Quadro com colunas por status (A Fazer, Em Andamento, Concluído) |
| Administrador | Criador do projeto, com privilégios de gestão |
| Membro | Usuário convidado para participar de um projeto |
| Audit trail | Histórico automático e imutável de alterações de uma tarefa |
| API REST | Interface HTTP entre front-end e back-end, com troca de dados em JSON |
| JWT | Token assinado usado para manter a sessão do usuário |
| MySQL | Banco de dados relacional usado para persistência |

## 2. Descrição Geral

### 2.1 Perspectiva do Produto

O WorkHub é uma aplicação web independente, acessada pelo navegador, composta por três camadas:

- **Front-end:** CSS e JavaScript executados no navegador, com layout em cards e quadro Kanban;
- **Back-end:** JavaScript (Node.js) que serve as páginas HTML e expõe uma API REST protegida por autenticação;
- **Banco de dados:** MySQL, com acesso por consultas parametrizadas.

### 2.2 Funções do Produto

O sistema deve oferecer as seguintes funcionalidades:

- Cadastro, login e recuperação de senha por e-mail;
- Edição de perfil (nome, e-mail, avatar e preferência de notificações);
- Criação de projetos e convite de membros por busca de e-mail;
- Dashboard com os projetos do usuário e a quantidade de tarefas pendentes;
- Criação de tarefas com título, descrição, prazo e prioridade;
- Atribuição de responsáveis e opção de assumir a tarefa;
- Quadro Kanban com mudança de status e filtros básicos;
- Comentários com data e hora, e anexos com validação de formato e tamanho;
- Notificações por e-mail e histórico imutável de alterações por tarefa.

### 2.3 Características dos Usuários

O público-alvo são equipes, inclusive não técnicas, com familiaridade básica com navegador e e-mail. Existem dois perfis, definidos **por projeto**: o mesmo usuário pode ser Administrador em um projeto e Membro em outro.

| Ação | Administrador | Membro |
| --- | :-: | :-: |
| Criar projeto | Sim | Sim |
| Editar configurações do projeto | Sim | Não |
| Convidar e remover membros | Sim | Não |
| Excluir projeto | Sim | Não |
| Visualizar o quadro | Sim | Sim |
| Criar tarefa e assumir tarefa | Sim | Sim |
| Mudar status (mover card) | Sim | Sim |
| Comentar e anexar arquivos | Sim | Sim |

### 2.4 Restrições

- Back-end em JavaScript (Node.js) e front-end em CSS e JavaScript, sem frameworks proprietários;
- Persistência exclusivamente em MySQL;
- Compatibilidade com as versões atuais de Chrome, Firefox, Edge e Safari, em desktop e celular;
- Anexos limitados a .pdf, .jpg/.jpeg, .png, .xls/.xlsx e .doc/.docx, com no máximo 5 MB por arquivo;
- Acesso via HTTPS em produção.

### 2.5 Suposições e Dependências

- O usuário possui navegador atualizado e conexão com a internet;
- Existe um servidor SMTP ou serviço de e-mail disponível para os alertas e a recuperação de senha;
- O servidor possui disco para armazenar os anexos fora do diretório público;
- O convidado já precisa estar cadastrado na plataforma para ser adicionado a um projeto.

## 3. Requisitos Funcionais (RF)

| Código | Descrição |
| --- | --- |
| RF001 | O sistema deve permitir o cadastro de conta com nome, e-mail único e senha (mínimo de 8 caracteres), armazenando a senha apenas como hash. |
| RF002 | O sistema deve permitir login com e-mail e senha, criando uma sessão autenticada e exibindo mensagem genérica em caso de credenciais inválidas. |
| RF003 | O sistema deve oferecer o fluxo "Esqueci minha senha", enviando por e-mail um link com token de uso único e validade limitada. |
| RF004 | O sistema deve permitir ao usuário atualizar nome e e-mail e enviar uma foto de perfil (avatar), exibida nos cards e comentários. |
| RF005 | O sistema deve permitir ao usuário criar múltiplos projetos, tornando o criador Administrador do projeto. |
| RF006 | O sistema deve permitir ao Administrador convidar usuários cadastrados por busca de e-mail e remover membros do projeto. |
| RF007 | O sistema deve exibir um Dashboard com os projetos do usuário e a quantidade de tarefas pendentes de cada um. |
| RF008 | O sistema deve permitir criar tarefas com título, descrição, data de vencimento e prioridade (Baixa, Média ou Alta). |
| RF009 | O sistema deve permitir atribuir uma tarefa a um ou mais membros do projeto, inclusive pela opção "Assumir tarefa". |
| RF010 | O sistema deve exibir as tarefas em quadro Kanban com as colunas "A Fazer", "Em Andamento" e "Concluído". |
| RF011 | O sistema deve permitir mudar o status da tarefa por drag and drop, seletor ou botão. |
| RF012 | O sistema deve permitir filtrar o quadro por "Minhas Tarefas", prioridade e status. |
| RF013 | O sistema deve permitir comentários em cada tarefa, registrando autor, data e hora. |
| RF014 | O sistema deve permitir anexar arquivos .pdf, .jpg/.png, .xls/.xlsx e .doc/.docx de até 5 MB, rejeitando qualquer outro formato ou tamanho. |
| RF015 | O sistema deve enviar e-mails de alerta para tarefas com vencimento em até 24 horas e para atribuições, comentários e convites, com opção de desativar no perfil. |
| RF016 | O sistema deve registrar automaticamente, de forma imutável, cada alteração da tarefa (criação, edição, status, responsáveis, comentários e anexos), com autor, valor anterior, valor novo e data/hora. |
| RF017 | O sistema deve restringir as ações de gestão do projeto (editar, convidar, remover membros e excluir) ao Administrador. |

**Regras de negócio relacionadas**

| Código | Regra |
| --- | --- |
| RN001 | Todo projeto possui ao menos um Administrador. |
| RN002 | Somente membros do projeto acessam seu quadro, tarefas, comentários e anexos. |
| RN003 | Os status são fixos: A Fazer, Em Andamento e Concluído. |
| RN004 | Tarefa com prazo vencido e status diferente de Concluído é sinalizada como atrasada. |
| RN005 | Excluir um projeto remove tarefas, comentários, anexos e históricos, mediante confirmação explícita. |
| RN006 | Datas e horas são gravadas no servidor (UTC) e exibidas no fuso do usuário. |

## 4. Requisitos Não Funcionais (RNF)

| Código | Descrição |
| --- | --- |
| RNF001 | Usabilidade: interface simples e intuitiva, com baixa curva de aprendizado para equipes não técnicas. |
| RNF002 | Responsividade: acesso adequado em desktop e dispositivos móveis. |
| RNF003 | Segurança: proteção dos dados de autenticação e controle de acesso conforme os papéis. |
| RNF004 | Disponibilidade: sistema acessível durante o uso normal por equipes remotas ou distribuídas. |
| RNF005 | Desempenho: operações comuns respondem em menos de 1 segundo em condições normais. |
| RNF006 | Integridade do histórico: o log de alterações não pode ser editado nem apagado. |

## 5. Interfaces do Sistema

### 5.1 Interface do Usuário

As telas do sistema são:

- **Login, cadastro e recuperação de senha:** formulários simples com validação imediata;
- **Dashboard:** cards dos projetos com quantidade de tarefas pendentes;
- **Quadro Kanban:** três colunas (A Fazer, Em Andamento, Concluído), cards com prioridade, prazo e avatares, e barra de filtros;
- **Detalhe da tarefa:** descrição, responsáveis, comentários, anexos e histórico de alterações;
- **Membros do projeto:** busca por e-mail, convite e remoção (somente Administrador);
- **Perfil:** nome, e-mail, avatar e opção de desativar notificações por e-mail.

### 5.2 Interfaces de Hardware

Não se aplicam.

### 5.3 Interfaces de Software

- Back-end: JavaScript (Node.js) com HTML servido ao navegador;
- Front-end: CSS e JavaScript;
- Banco de dados: MySQL;
- Servidor SMTP ou serviço de e-mail para as notificações.

### 5.4 Interfaces de Comunicação

Comunicação entre navegador e servidor por HTTPS, com API REST em JSON.&#32;

## 6. Requisitos de Qualidade

| Atributo | Descrição |
| --- | --- |
| Usabilidade | Interface intuitiva e simples, adequada para equipes não técnicas. |
| Confiabilidade | Validação de entradas e transações no banco, garantindo que alteração e histórico sejam gravados juntos. |
| Segurança | Senhas com hash, controle de acesso por papel e validação de anexos no servidor. |
| Manutenibilidade | Código modular (rotas, serviços e acesso a dados separados) e documentado. |
| Portabilidade | Execução em qualquer navegador atual, em desktop e celular. |
| Eficiência | Consultas indexadas e paginação, com baixo consumo de recursos. |

## 7. Casos de Uso

### 7.1 Criar uma tarefa

1. O Membro abre o quadro do projeto.
2. O Membro clica em "Nova tarefa".
3. O Membro informa título, descrição, prazo e prioridade.
4. O Membro confirma.
5. O sistema grava a tarefa na coluna "A Fazer" e registra a criação no histórico.

### 7.2 Mover uma tarefa

1. O Membro arrasta o card para outra coluna (ou escolhe o status no seletor do card).
2. O sistema verifica a sessão e a participação no projeto.
3. O sistema atualiza o status e registra no histórico, por exemplo "João alterou o status de 'A Fazer' para 'Em Andamento'".
4. O quadro é atualizado.

### 7.3 Anexar um arquivo

1. O Membro abre o detalhe da tarefa e seleciona o arquivo.
2. O front-end confere extensão e tamanho.
3. O servidor revalida formato, tipo do arquivo e limite de 5 MB.
4. O sistema salva o arquivo, registra "Maria anexou o arquivo relatorio.pdf" no histórico e notifica os responsáveis que aceitam e-mails.

### 7.4 Convidar um membro

1. O Administrador abre a tela de membros e busca por e-mail.
2. O sistema localiza o usuário cadastrado.
3. O Administrador confirma o convite.
4. O usuário passa a ver o projeto no Dashboard.

### 7.5 Recuperar a senha

1. O usuário clica em "Esqueci minha senha" e informa o e-mail.
2. O sistema responde de forma neutra e envia o link com token.
3. O usuário abre o link e define a nova senha.
4. O sistema invalida o token e as sessões antigas.

## 8. Casos de Teste (CT)

| Caso de Teste | Entrada | Resultado Esperado |
| --- | --- | --- |
| CT001 (Cadastro) | Nome, e-mail novo e senha de 8+ caracteres | Conta criada; senha salva como hash |
| CT002 (E-mail duplicado) | Cadastro com e-mail já existente | Cadastro recusado com mensagem de erro |
| CT003 (Login inválido) | E-mail correto e senha errada | "E-mail ou senha inválidos"; nenhuma sessão criada |
| CT004 (Token expirado) | Link de redefinição usado após a validade | Redefinição recusada |
| CT005 (Criar tarefa) | Título, prazo e prioridade Alta | Tarefa em "A Fazer" e entrada no histórico |
| CT006 (Mover card) | Arrastar tarefa de "A Fazer" para "Em Andamento" | Status atualizado e histórico com autor, valor anterior e novo |
| CT007 (Anexo válido) | relatorio.pdf de 2 MB | Arquivo anexado |
| CT008 (Anexo inválido) | programa.exe | Upload recusado: formato não permitido |
| CT009 (Anexo grande) | arquivo .pdf de 5,1 MB | Upload recusado: acima de 5 MB |
| CT010 (Permissão) | Membro tenta excluir o projeto | Resposta 403; projeto mantido |
| CT011 (Acesso indevido) | Não membro abre uma tarefa do projeto | Acesso negado |
| CT012 (Filtro) | Filtro "Minhas Tarefas" | Somente tarefas em que o usuário é responsável |
| CT013 (Notificação) | Tarefa vence em 23 h, usuário com e-mails ativos | E-mail enviado uma única vez |
| CT014 (Opt-out) | Mesma situação, e-mails desativados no perfil | Nenhum e-mail enviado |
| CT015 (Histórico imutável) | Tentativa de UPDATE ou DELETE no histórico | Operação negada pelo banco |
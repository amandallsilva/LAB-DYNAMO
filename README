# RELATÓRIO TÉCNICO: DESENVOLVIMENTO DE SISTEMA DE MENSAGERIA ESCALÁVEL COM NOSQL

**Disciplina:** Banco de Dados 2  
**Tema:** Persistência Poliglota e Modelagem Orientada a Acesso (Access Patterns)  
**Projeto:** UFAM Connect  

---

## 1. INTRODUÇÃO

O presente trabalho descreve a implementação de uma aplicação de chat em tempo real (**UFAM Connect**), utilizando uma arquitetura baseada em microsserviços simulados e persistência de dados não-relacional (NoSQL).  
O projeto visa demonstrar as vantagens do uso de bancos de dados orientados a chave-valor e documentos para cenários de alta volumetria de escrita e leitura, onde bancos relacionais tradicionais (RDBMS) apresentam gargalos de escalabilidade horizontal.

---

## 2. OBJETIVOS

### 2.1 Objetivo Geral
Desenvolver uma aplicação Web completa (Frontend e Backend) que utilize o **Amazon DynamoDB** como motor de persistência, aplicando técnicas avançadas de modelagem NoSQL para garantir performance e baixo custo computacional.

### 2.2 Objetivos Específicos

- **Implementar CRUD de Usuários e Mensagens:** Permitir cadastro, autenticação e troca de mensagens multimídia.  
- **Otimizar Padrões de Acesso (Access Patterns):** Eliminar operações de varredura completa (*Full Table Scans*) em favor de buscas diretas (*GetItem/Query*).  
- **Garantir Escalabilidade:** Modelar tabelas utilizando chaves de partição (*Partition Keys*) e chaves de ordenação (*Sort Keys*) adequadas para chat.  
- **Aplicar Atualizações Atômicas:** Implementar edição de perfil sem a necessidade de reescrita total do objeto, economizando **Write Capacity Units (WCUs)**.

## 3. REQUISITOS DO SISTEMA

### 3.1 Requisitos Funcionais (RF)

- **RF01 – Gerenciamento de Identidade:**  
  O sistema deve permitir o cadastro de usuários com login, senha e upload de foto de perfil (avatar).

- **RF02 – Autenticação:**  
  O sistema deve validar credenciais para permitir acesso às funcionalidades privadas.

- **RF03 – Criação de Conversas:**  
  O sistema deve identificar ou criar automaticamente uma sala de chat entre dois usuários.

- **RF04 – Troca de Mensagens:**  
  O usuário deve ser capaz de enviar mensagens de texto e imagens em tempo real.

- **RF05 – Histórico de Mensagens:**  
  O sistema deve carregar as mensagens anteriores ordenadas cronologicamente.

- **RF06 – Edição de Perfil:**  
  O usuário deve conseguir alterar sua foto de perfil através de uma interface modal, com atualização imediata no sistema.

---

### 3.2 Requisitos Não-Funcionais (RNF)

- **RNF01 – Performance:**  
  O tempo de carregamento de mensagens deve ser constante (**O(1)**), independente do volume total de dados, utilizando paginação.

- **RNF02 – Persistência:**  
  Os dados devem ser persistidos em container Docker (DynamoDB Local) com volumes mapeados em disco.

- **RNF03 – Armazenamento de Mídia:**  
  Imagens devem ser armazenadas em sistema de arquivos estático, persistindo apenas a referência (URL) no banco de dados (**Reference Store Pattern**).

## 4. MODELAGEM DE DADOS (NoSQL)

A modelagem foi desenhada para atender aos padrões de acesso da aplicação, fugindo da normalização tradicional (3FN) dos bancos relacionais.

---

### 4.1 Tabela **Users**

- **Partition Key (PK):** `userId` (UUID)

**Justificativa:**  
Permite busca direta do perfil do usuário por ID.

---

### 4.2 Tabela **Chats**

- **Partition Key (PK):** `chatId`  
  **Formato:** `USERA_USERB` (String composta)

**Justificativa:**  
Implementação de **ID Determinístico**.  
Ao ordenar alfabeticamente os IDs dos participantes, cria-se uma chave única.  
Isso elimina a necessidade de realizar um *Scan* no banco para verificar se um chat já existe.

---

### 4.3 Tabela **Messages**

- **Partition Key (PK):** `chatId`  
- **Sort Key (SK):** `timestamp` (string numérica)

**Justificativa:**  
Permite uso de operações de **Query**.  
A SK mantém as mensagens ordenadas cronologicamente, enquanto a PK agrupa todas as mensagens de uma conversa na mesma partição física — garantindo leitura rápida e eficiente.

---

## 5. ARQUITETURA E TECNOLOGIAS

- **Frontend:** HTML5, CSS3 e JavaScript (Vanilla) com manipulação de DOM para criar uma SPA simples.  
- **Backend:** Node.js com Express estruturando a API RESTful.  
- **Banco de Dados:** DynamoDB Local executado via Docker Compose.  
- **Integração:** AWS SDK v3 para comunicação otimizada com o banco NoSQL.

---

## 6. CONCLUSÃO

O desenvolvimento do sistema **UFAM Connect** demonstrou que, quando modelados corretamente utilizando chaves de partição e ordenação, bancos NoSQL oferecem vantagens significativas sobre bancos relacionais em aplicações de chat.  
A implementação de **IDs determinísticos** e **paginação via Query** eliminou gargalos de performance, atendendo plenamente aos requisitos da disciplina de Banco de Dados 2.

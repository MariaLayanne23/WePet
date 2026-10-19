# 🐾 PRD - Product Requirements Document: WePet

## 1. Visão do Produto
* **Nome do Projeto:** WePet
* **Problema Identificado:** O processo tradicional de adoção de animais de estimação costuma ser burocrático e descentralizado. Protetores e abrigos sofrem para gerenciar interessados através de redes sociais ou aplicativos de mensagens pessoais, perdendo o histórico de conversas e dificultando a triagem dos adotantes.
* **Público-alvo:** 
  * *Adotantes:* Pessoas físicas que buscam adotar um animal de estimação de forma segura e transparente.
  * *Protetores/Abrigos:* ONGs e protetores independentes que divulgam animais para adoção e precisam gerenciar os interessados.
* **Objetivos e Benefícios Esperados:** 
  * Centralizar o processo de adoção em uma única plataforma.
  * Facilitar a comunicação direta e organizada entre quem doa e quem quer adotar.
  * Aumentar a taxa de adoções bem-sucedidas por meio de um canal de comunicação ágil.

---

## 2. Escopo

### Funcionalidades Previstas (Gerais)
1. Cadastro e login de usuários (com perfis diferenciados: Adotante e Protetor).
2. Cadastro e listagem de pets disponíveis para adoção (com fotos, raça, idade e descrição).
3. Sistema de filtros e busca de pets.
4. Funcionalidade de chat em tempo real entre interessado e protetor.
5. Sistema de favoritos para pets.

### Definição do MVP (Mínimo Produto Viável)
* Cadastro básico de usuários.
* Listagem e visualização de detalhes dos pets.
* **Chat funcional** permitindo iniciar conversas a partir da página de um pet e trocar mensagens armazenadas em banco NoSQL.

### O que ficará fora da entrega (Out of Scope / Versão 2)
* Notificações Push em tempo real no celular.
* Envio de mídias pesadas (vídeos e áudios no chat).
* Formulários complexos de triagem de adoção automatizados dentro da conversa.

![Figma](/imagens/figma-filtros.png) 

---

## 3. Requisitos

### 3.1 Requisitos Funcionais (RF)
* **RF01:** O sistema deve permitir o cadastro de usuários informando nome, e-mail, senha e tipo de perfil (*Adotante* ou *Protetor*).
* **RF02:** O sistema deve permitir o login de usuários autenticando e-mail e senha.
* **RF03:** O protetor deve conseguir cadastrar um pet informando nome, espécie, raça, idade, descrição e foto.
* **RF04:** O adotante deve conseguir buscar e filtrar pets por raça ou espécie na tela inicial.
* **RF05:** O usuário deve conseguir iniciar uma conversa clicando no botão "Conversar com o Protetor" na página de detalhes de um pet.
* **RF06:** O sistema deve permitir o envio e salvamento de mensagens de texto entre os participantes da conversa em um banco de dados NoSQL.
* **RF07:** O sistema deve exibir o histórico de mensagens de uma conversa ordenado cronologicamente (da mais antiga para a mais recente).

### 3.2 Requisitos Não Funcionais (RNF)
* **RNF01:** O banco de dados NoSQL utilizado para persistir as conversas e mensagens deve ser o **MongoDB**.
* **RNF02:** O histórico de mensagens do chat deve carregar em no máximo 2 segundos após o usuário abrir a conversa.
* **RNF03:** As senhas dos usuários devem ser armazenadas de forma criptografada.
* **RNF04:** A interface da aplicação deve ser responsiva, adaptando-se a dispositivos móveis e desktops.

### 3.3 Histórias de Usuário e Critérios de Aceitação
* **História de Usuário 1:** *Como adotante, quero visualizar os detalhes de um pet para avaliar se ele se encaixa no meu estilo de vida.*
  * *Critérios de Aceitação:*
    1. A tela de detalhes deve exibir obrigatoriamente a foto, nome, idade e descrição do pet.
    2. A tela deve conter um botão visível com o texto "Conversar com o Protetor".
* **História de Usuário 2:** *Como adotante ou protetor, quero enviar mensagens de texto em uma conversa para negociar a adoção do pet.*
  * *Critérios de Aceitação:*
    1. O sistema não deve permitir o envio de mensagens com o campo de texto vazio.
    2. Ao enviar, a mensagem deve ser gravada imediatamente na coleção de mensagens do banco NoSQL associada ao respectivo `chat_id`.
    3. A nova mensagem deve aparecer instantaneamente na tela do remetente.

---

## 4. Gerência do Projeto
* **Integrantes e Responsabilidades:** 
Maria Layane
Vinícius Matheus
Fabio Fernandes
Gustavo França

---

## 5. Funcionalidade de Conversa com NoSQL

![Diagrama NoSQL](/imagens/diagrama-nosql.png)

### 5.1 O Banco NoSQL Escolhido e Justificativa
* **Banco de Dados Escolhido:** **MongoDB**
* **Justificativa:** Escolhido pela flexibilidade de esquema (documentos JSON), excelente desempenho em operações de escrita em massa (essencial para chats) e escalabilidade horizontal.

### 5.2 Regras de Negócio e Iniciação das Conversas
1. **Vínculo com o Pet:** O chat nasce obrigatoriamente a partir da página de detalhes de um animal.
2. **Prevenção de Duplicidade:** O sistema verifica se já existe um chat entre os mesmos usuários para aquele pet antes de criar um novo registro.
3. **Quem pode conversar:** Apenas o adotante interessado e o protetor responsável pelo pet.

### 5.3 Organização e Modelagem dos Dados (Esquema NoSQL)

* **Coleção `chats`:**
  ```json
  {
    "_id": "chat_xyz789",
    "pet_id": "pet_456",
    "participants": ["user_adotante_01", "user_protetor_02"],
    "created_at": "2026-10-19T10:00:00Z"
  }
* **Coleção `messages`:**
```json

 {
  "_id": "msg_001",
  "chat_id": "chat_xyz789",
  "sender_id": "user_adotante_01",
  "text": "Olá! Gostaria de saber mais sobre o pet.",
  "sent_at": "2026-10-19T10:05:00Z"
 }

### 5.4 Regras de Acesso e Segurança
**Validação no Back-end:** Apenas os IDs listados no array participants podem ler e escrever mensagens naquele chat.
**Tratamento de Exceção:** Tentativas de acesso externo ou de usuários não autorizados retornam um erro de acesso negado (403 Forbidden).

### 5.5 Critérios de Aceitação da Funcionalidade
* **Envio:** O sistema impede o envio de texto vazio e grava o documento na coleção messages atrelando-o corretamente ao chat_id.

* **Consulta:** Ao abrir a conversa, o sistema filtra o histórico estritamente pelo chat_id correspondente.

* **Ordenação:** As mensagens são ordenadas em ordem cronológica crescente (sent_at: 1) para manter o fluxo natural da conversa.

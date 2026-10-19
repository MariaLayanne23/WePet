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

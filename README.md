# Chat Em Tempo Real

Este é um componente de chat desenvolvido em Vue 3 e TypeScript que utiliza SignalR para comunicação em tempo real. Ele permite que usuários enviem mensagens em um chat em grupo com atualização dinâmica e interface amigável.

## Tabela de Conteúdos

- [Descrição](#descrição)
- [Requisitos](#requisitos)
- [Instalação](#instalação)
- [Uso](#uso)
- [Estrutura do Código](#estrutura-do-código)
- [Estilos](#estilos)

## Descrição

O `ChatComponent` é um componente de chat em tempo real com as seguintes funcionalidades:

- Envio de mensagens em um canal compartilhado.
- Exibição do nome de usuário e timestamp ao lado de cada mensagem.
- Rolagem automática para a última mensagem.
- Conexão com um SignalR Hub para sincronização em tempo real de mensagens entre usuários.

## Requisitos

- Node.js
- Vue 3
- Vite
- TypeScript
- SignalR Client Library

## Instalação

1. **Clone o repositório**:

   ```bash
   git clone https://github.com/carlosrsantos/clientChat.git

   cd <app-folder>
   ```

2. **Instalar dependências**:
  
  ```bash
   npm install
   ```

3. **Inicializar o projeto**:

  ```bash
  npm run dev
   ```

## Uso

### Acesse a aplicação

Abra [http://localhost:8080](http://localhost:8080) (ou a porta configurada no seu ambiente) em seu navegador para acessar o chat.

### Interaja com o Chat

1. Digite seu nome de usuário.
2. Escreva uma mensagem e pressione `Enter` ou clique em `Send` para enviá-la.

## Estrutura do Código

### Template

O template define a estrutura da interface do chat com:

- Campo de entrada para o nome de usuário.
- Campo de entrada para mensagens.
- Botão `Send` para enviar mensagens.
- Contêiner para exibição das mensagens.

### Script

O script utiliza Vue 3 com Composition API e SignalR para:

- **Gerenciar a conexão** com o SignalR Hub.
- **Enviar mensagens** usando o método `send()`.
- **Receber mensagens** e exibi-las em tempo real.
- **Rolar automaticamente** para a última mensagem na lista.

#### Principais Funções

- `send()`: Envia a mensagem para o servidor SignalR.
- `scrollToBottom()`: Realiza a rolagem para a última mensagem no contêiner de mensagens.
- `onMounted()`: Inicia a conexão SignalR quando o componente é montado.

## Estilos

O estilo é aplicado de forma escopada, com um layout centralizado para o chat e os seguintes elementos personalizados:

- Contêiner de mensagens com rolagem automática (`overflow-y`).
- Estilos personalizados para campos de entrada (`input-name`, `message-input`) e botão (`send-button`).

## Servidor para aplicação:
- [Server](https://github.com/carlosrsantos/serverchat)



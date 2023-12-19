# Projeto Node.js - Servidor Web para Responder Requisições com Diferentes Status HTTP

## Configurações

1. **Certifique-se de ter o Node.js instalado no seu sistema.**

   Se você ainda não tem o Node.js instalado, você pode baixá-lo [aqui](https://nodejs.org/). Escolha a versão recomendada para a sua plataforma e siga as instruções de instalação.

## Iniciando a Criação do Projeto

2. **Crie um arquivo chamado `server.js` e adicione o seguinte código:**

    ```javascript
    const http = require('http');
    const url = require('url');
    
    const server = http.createServer((req, res) => {
      // Parse a URL para obter os parâmetros da rota
      const parsedUrl = url.parse(req.url, true);
      const path = parsedUrl.pathname;
    
      // Lógica para determinar o status HTTP com base nos parâmetros da rota
      let statusCode;
      let responseMessage;
    
      if (path.startsWith('/listar/')) {
        const parametro = parseInt(path.split('/')[2], 10);
    
        if (isNaN(parametro)) {
          statusCode = 400; // Bad Request se o parâmetro não for um número
          responseMessage = 'Parâmetro inválido. Deve ser um número.';
        } else if (parametro === 50) {
          statusCode = 404; // Not Found para o valor 50
          responseMessage = 'Recurso não encontrado.';
        } else {
          statusCode = 200; // OK para outros valores
          responseMessage = `Requisição bem-sucedida para o valor ${parametro}.`;
        }
      } else {
        statusCode = 404; // Not Found para rotas não reconhecidas
        responseMessage = 'Rota não encontrada.';
      }
    
      // Configuração da resposta
      res.writeHead(statusCode, { 'Content-Type': 'text/plain' });
      res.end(responseMessage);
    });
    
    const PORT = 3000;
    
    server.listen(PORT, () => {
      console.log(`Servidor rodando em http://localhost:${PORT}`);
    });
    ```

3. **No terminal, execute o seguinte comando para iniciar o servidor:**

    ```bash
    node server.js
    ```

## Personalizando Rotas e Status HTTP

O servidor responde a diferentes rotas com status HTTP personalizados, dependendo dos parâmetros enviados. Veja o exemplo abaixo:

- Rota `/listar/50`: Retorna HTTP 404 (Not Found).
- Rota `/listar/10`: Retorna HTTP 200 (OK) com uma mensagem personalizada.

# Aprendendo node

## Obs: Documento em constante edição

<details>
  <summary>Montagem de ambiente</summary>

  # Configuração do Ambiente Node.js com Yarn
  ## Dependências e bibliotecas que serão usadas
   - yarn
   - nodemon
   - esLint
   - express
   - javascript
   - mysql
   - dotenv

  ## Comandos
  ```bash
  yarn init -y         //inicializa o projeto e cria um arquivo package.json
  yarn add express     //Simplifica o desenvolvimento de aplicativos web em Node.js,baixa a pasta node_modules e o package-lock.json
  yarn add mysql2      //Instala o driver do MySQL para Node.JS
  yarn add nodemon -D  //monitora as alterações nos arquivos do projeto e reinicia automaticamente o servidor quando detecta alteração.
  yarn add dotenv      //Instala a biblioteca dotenv que facilita a leitura de variáveis de ambiente

  ```
  ## Configurando esLint
   - Ferramenta de linting que ajuda a manter um código JavaScript consistente e livre de erros.
  ```bash
  npx eslint --init
  ```
   - Pergunta 1: Terceira opção
   - Pergunta 2: Common
   - Pergunta 3: None of these
   - Pergunta 4: No
   - Pergunta 6: Node
   - Pergunta 7: Answer questions about your style
   - Pergunta 8: JSON
   - Pergunta 9: Spaces
   - Pergunta 10: Single
   - Pergunta 11: Unix
   - Pergunta 12: Yes

  #### Atualize o arquivo .eslintrc.json:
   - Vá até o arquivo .eslintrc.json e, em "indent", troque o valor de 4 para 2.

  #### Crie o arquivo .eslintignore:
   - Crie o arquivo .eslintignore e adicione os seguintes nomes de pastas:
  ```bash
  node_modules/
  dist/
  ```
  ## Configuração do Script de inicialização
  ```javascript
  "scripts": {
    "start": "node src/index.js"
  }
  ```
  ### Inicie o servidor com `yarn start` 





  
</details>

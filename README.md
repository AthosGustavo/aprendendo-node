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

<details>
  <summary>Arquitetura básica de um projeto</summary>
</details>

<details>
  <summary>Express</summary>

  # Express
  #### express()
   - Retorna uma instância de um aplicativo Express que contém funções e configurações
  
  ```javascript
  const express = require('express');
  const app = express();

  app.get('/', (req, res) => {
    res.send('Hello World!');
  });

  app.listen(3000, () => {
    console.log('Server listening on port 3000');
  });
  ```

  ## Exportação e importação de arquivos
  ### Exportação e importação com module.export
  *EXEMPLO*
   - No momento de exportação, exporta-se o dado requerido e no momento da importação, importa-se o arquivo e apenas os dados exportados serão recebidos.
   - `module.export = variavelTeste`
   - `const app = require('./app');`

  #### Exportando objetos
  ```javascript
  // No arquivo config.js
  const config = {
    chave1: 'valor1',
    chave2: 'valor2',
  };
  module.exports = config;

  // Em outro arquivo
  const config = require('./config');
  console.log(config.chave1); // valor1
  ```
  #### Exportando multiplos valores
  ```javascript
  // No arquivo utils.js
  const util1 = 'alguma coisa';
  const util2 = 'outra coisa';

  module.exports = {
    util1,
    util2,
  };

  // Em outro arquivo
  const utils = require('./utils');
  console.log(utils.util1); // alguma coisa
  ```
  
  ## Sintaxe dos métodos HTTP do aplicativo Express
  ```javascript
  app.get(path,middleware, (callback => {}))
  ```
  ```javascript
  app.get('path', (request, response) => response.send('Hello word'));
  ```
  ### response
   - Objeto que contém métodos e propriedades que permitem enviar uma resposta de volta ao cliente.

  #### Métodos do objeto response
   - `response.send(data): Envia uma resposta ao cliente. Pode ser uma string, objeto ou array`
   - `res.json(data): Envia uma resposta em formato JSON.`
   - `res.status(code): Define o código de status da resposta.`
   - `res.redirect(path): Redireciona o cliente para o caminho especificado.`

  ## request
   - Objeto que contém informações sobre a solicitação do cliente

  
</details>
<details>
  <summary>TypeORM</summary>

  # TypeORM
  ## DataSource
   - Arquivo com dados de acesso ao banco de dados.

  ```typescript
  import { DataSource } from "typeorm"

  const AppDataSource = new DataSource({
    type: "mysql",
    host: "localhost",
    port: 3306,
    username: "athos",
    password: "senha",
    database: "nomeBanco",
  })

  AppDataSource.initialize()
    .then(() => {
        console.log("Data Source has been initialized!")
    })
    .catch((err) => {
        console.error("Error during Data Source initialization", err)
  })
```

  
<details>
  <summary>Mapeando entidades</summary>

  # Mapeando entidades

  ## Anotations
  ### @Entity()
   - Anotação usada para sinalizar que a classe representa uma tabela no banco de dados
   - Caso o nome da classe seja diferente do nome da tabela do banco, dentro do parênteses o nome da tabela deve ser colocada entre aspas.
  
  ### @PrimaryColumn()
   - A anotação é usada para marcar uma propriedade como chave primária da tabela

  ### @Column()
   - Anotação usada para sinalizar que o atributo da classe representa uma coluna do banco de dados
   - Caso o nome do atributo seja diferente do nome da coluna do banco de dados, o nome da coluna deve ser colocada entre aspas no parênteses
  
  ### @CreatedDateColumn()
   - Anotação usada para marcar uma propriedade que será automaticamente preenchida com a data e a hora de criação da entidade
  
  ```javascript
  import { Entity, PrimaryColumn, Column, CreateDateColumn } from 'typeorm';

  @Entity()
  class User {
    @PrimaryColumn()
    id: number;

    @Column()
    username: string;

    @Column()
    email: string;

    @CreateDateColumn()
    createdAt: Date;

  // outras propriedades...
  }

```
  
</details>

<details>
  <summary>Relacionamento</summary>

  # Relacionamento
  ## @manyToOne

  ### Lógica e sintaxe do relacionamento
   - `() => UserProfile` é uma função que retorna a classe da entidade com a qual o relacionamento esta sendo feito.
   - `@JoinColumn` é usado para indicar que a coluna na tabela atual (a tabela User neste caso) deve ser usada como coluna de junção para o relacionamento.
  ```javascript
  @OneToOne(() => UserProfile)
  @JoinColumn()
  profile: UserProfile;
  ```

  
  ```javascript
  import { Entity, PrimaryGeneratedColumn, Column, OneToOne, JoinColumn } from 'typeorm';

  @Entity()
  class User {
    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    name: string;

    @OneToOne(() => UserProfile)
    @JoinColumn()
    profile: UserProfile;
  }
  ```
  ```javascript
  @Entity()
  class UserProfile {
    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    bio: string;
  }

  ```
  ## @manyToOne

  ### Lógica e sintaxe do relacionamento

   - A função especifica a classe entidade alvo
   - Usa uma variável para representar a classe alvo e sinaliza que a propriedade autor da classe alvo será usada para fazer a junção.

  ```javascript
  @OneToMany(() => Post, post => post.author)
  posts: Post[];
  ```
  ```javascript
  import { Entity, PrimaryGeneratedColumn, Column, OneToMany } from 'typeorm';

  @Entity()
  class User {
    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    name: string;

    @OneToMany(() => Post, post => post.author)
    posts: Post[];
  }

  ```
  ```javascript
  import { Entity, PrimaryGeneratedColumn, Column, ManyToOne } from 'typeorm';

  @Entity()
  class Post {
    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    title: string;

    @ManyToOne(() => User, user => user.posts)
    author: User;
  }

  ```
  
  
</details>

  
</details>



<details>
  <summary>Repository</summary>
  
  ## Entendendo Repository
  ### Imports
  ```javascript
  import { getRepository } from 'typeorm';
  import { EntidadeMapeada } from './entities/EntidadeMapeada';
  ```
  
  ### Retorno de getRepository()
   - Retorna um objeto que contém informações da entidade mapeada, do banco de dados e métodos para realizar operações.
  ```javascript
  const entidadeMapeadaRepository = dataSource.getRepository(EntidadeMapeada);
  ```
  ### entidadeMapeadaRepository
   - Objeto que detém os métodos de consulta ao banco de dados
</details>






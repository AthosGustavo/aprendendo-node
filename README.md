# Documentação base para um projeto em Node, Express, TypeScript e TypeORM

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
  yarn typescript ts-node-dev @types/express -D
  yarn add express     //Simplifica o desenvolvimento de aplicativos web em Node.js,baixa a pasta node_modules e o package-lock.json
  yarn add mysql2      //Instala o driver do MySQL para Node.JS
  yarn add nodemon -D  //monitora as alterações nos arquivos do projeto e reinicia automaticamente o servidor quando detecta alteração.
  yarn add dotenv      //Instala a biblioteca dotenv que facilita a leitura de variáveis de ambiente
  yarn add typeorm reflect-metada 
  ```
  ## Depedências
  
  ### Dependência ts-node-dev
   - Bilioteca utilizada para executar arquivos TypeScript diretamente, sem a necessidade de compilar para JavaScript manualmente antes.
 
`--transpile-only`:opção do ts-node-dev que instrui o TypeScript a apenas transpilar o código TypeScript para JavaScript sem realizar a checagem de tipos. 
 
  - Comando para instalar a biblioteca `yarn add ts-node-dev --dev`
 
  ### dependencia @types/express
  - A dependência @types/express é usada para fornecer definições de tipos TypeScript para o Express.
  - Essas definições de tipos sao usadas para que o compilador TypeScript possa verificar e garantir que seu código esteja em conformidade com a API do Express.
 
  - Comando para instalar a dependencia `yarn add @types/express --dev`
    - `--dev` significa que você está instalando esta dependência como uma dependência de desenvolvimento.
  
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

  ## Arquivo package.json
   - Todas as dependências do projeto, incluindo Express, versões e quaisquer outras bibliotecas necessárias, são listadas neste arquivo.

  #### objeto dependencies
   - Lista as dependências do projeto que são necessárias para que o aplicativo seja executado em produção. 
   - As dependências listadas nesta seção são instaladas por padrão quando alguém executa npm install ou yarn install no diretório do projeto.

  #### objeto devDependencie
   - Esta seção lista as dependências do projeto que são necessárias apenas durante o desenvolvimento e não são necessárias para o funcionamento do aplicativo em produção.
   - Isso inclui coisas como bibliotecas de desenvolvimento, ferramentas de compilação, testes, entre outros. 
   - Essas dependências não são incluídas no pacote que será implantado no ambiente de produção.
   
</details>

<details>
  <summary>Arquitetura básica de um projeto</summary>

  ![arquitetura-passo2 drawio](https://github.com/AthosGustavo/aprendendo-node/assets/112649935/643ac6b5-f38d-4caf-ade7-2d910a42e2ca)
  
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
   - Na chave entites, deve ser colocada as entidades mapeadas.
   - A porta do banco de dados não é colocada no método construtor de DataSource, porque o método irá usar a porta padão do mysql,`3606`
  ```typescript
  import { DataSource } from "typeorm";
  import { envVariables } from "../config/env-variables";
  import { User } from "../entities/User";

  const [DATA_BASE_HOST, DATA_BASE_TYPE, DATA_BASE_USER, DATA_BASE_PASSWORD, DATA_BASE] = envVariables;
    const AppDataSource = new DataSource({
    type: DATA_BASE_TYPE,
    host: DATA_BASE_HOST,
    username: DATA_BASE_USER,
    password: DATA_BASE_PASSWORD,
    database: DATA_BASE,
    entities:[User]

  });

  export default AppDataSource;
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
  ## @OneToOne

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
  ## @ManyToMany
   - A anotação marca que a tabela irá ter uma relacionamento muitos para muitos através da propriedade `courses`
   - A função `() => Course` devolve a classe classe `Course`
   - Em seguida, a propriedade `courses` é criada e associada ao tipo da classe retornada pela função.
  ```javascript
  import { Entity, PrimaryGeneratedColumn, Column, ManyToMany, JoinTable } from 'typeorm';

  @Entity()
  class Student {
    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    name: string;

    @ManyToMany(() => Course)
    @JoinTable()
    courses: Course[];
  }
  ```
  ```javascript
  @Entity()
  class Course {
    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    name: string;

    @ManyToMany(() => Student)
    students: Student[];
  }

  ```
  
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

<details>
  <summary>Migrations</summary>

# Migrations
### Passo a passo para gerar e rodar uma migration

- 1:Criar os scrips para gerar e rodar a migration no arquivo `package.json`.
```json
"scripts": {
    "dev": "ts-node-dev --transpile-only src/server.ts",
    "typeorm": "ts-node-dev node_modules/.bin/typeorm",
    "build": "tsc",
    "migration:generate":"typeorm-ts-node-commonjs -d ./src/database/DataSource.ts migration:generate ./src/migrations/default",
    "migration:run":"typeorm-ts-node-commonjs -d ./src/database/DataSource.ts migration:run"
  },
```
- 2:Declarar o caminho das entidades e do diretório onde ficarão as migrations geradas.
```javascript
entities:[`${__dirname}/**/entities/*.{ts,js}`],
migrations: [`${__dirname}/**/migrations/*.{ts,js}`],
migrationsTableName: "migrations",
```
  - `migrationsTableName` Nome da tabela que que será armazenada as transações
- 3:Comando para criar uma nova migration `yarn typeorm migration:create <caminho>`
- 4: Criando uma migration
```javascript
import { MigrationInterface, QueryRunner, Table } from "typeorm";

export class TableUsers1707917645585 implements MigrationInterface {

  public async up(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.createTable(new Table({
      name:"users",
      columns:[
        {
          name:"id",
          type:"int",
          isPrimary:true,
          isGenerated:true,
          generationStrategy:"increment"
        },
        {
          name:"nome",
          type:"varchar",
          length:"50",
          isNullable:false,
        },
        {
          name:"email",
          type:"varchar",
          length:"50",
          isNullable:false,
        },
        {
          name:"senha",
          type:"varchar",
          length:"50",
          isNullable:false,
        },
        {
          name:"endereco",
          type:"varchar",
          length:"100",
          isNullable:true,
        },
        {
          name:"contato",
          type:"varchar",
          length:"11",
          isNullable:true,
        },
        {
          name:"data_cadastro",
          type:"timestamp",
          default:"CURRENT_TIMESTAMP",
          isNullable:false
        }

      ]
    }));

  }

  public async down(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.dropTable("users");
  }

}
```
  
</details>  
</details>










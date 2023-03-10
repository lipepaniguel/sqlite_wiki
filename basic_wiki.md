# SQLite

## SQLite Command Prompt
Acessando o prompt e conhecendo seus comandos: uma breve introdução do CLI do SQLite.

### Início
Antes de mais nada é preciso ter acesso ao prompt do SQLite. Para isso, à partir do terminal, basta utilizar o comando `sqlite3`.
```
$ sqlite3
SQLite version 3.37.2 2022-01-06 13:25:41
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite> 
```

### Help
Como observado anteriormente, já nos é apresentado, logo de cara, o comando `.help`. Um comando muito útil para conhecermos melhor as capacidades ou possibilidades de todos os comandos do prompt disponíveis. Vejamos:
```
sqlite> .help
.archive ...             Manage SQL archives
.auth ON|OFF             Show authorizer callbacks
.backup ?DB? FILE        Backup DB (default "main") to FILE
.bail on|off             Stop after hitting an error.  Default OFF       
.binary on|off           Turn binary output on or off.  Default OFF      
.cd DIRECTORY            Change the working directory to DIRECTORY       
.changes on|off          Show number of rows changed by SQL
.check GLOB              Fail if output since .testcase does not match   
.clone NEWDB             Clone data into NEWDB from the existing database
.connection [close] [#]  Open or close an auxiliary database connection  
.databases               List names and files of attached databases
.dbconfig ?op? ?val?     List or change sqlite3_db_config() options
.dbinfo ?DB?             Show status information about the database
.dump ?OBJECTS?          Render database content as SQL
.echo on|off             Turn command echo on or off
.eqp on|off|full|...     Enable or disable automatic EXPLAIN QUERY PLAN
.excel                   Display the output of next command in spreadsheet
.exit ?CODE?             Exit this program with return-code CODE
.expert                  EXPERIMENTAL. Suggest indexes for queries
.explain ?on|off|auto?   Change the EXPLAIN formatting mode.  Default: auto
.filectrl CMD ...        Run various sqlite3_file_control() operations
.fullschema ?--indent?   Show schema and the content of sqlite_stat tables
.headers on|off          Turn display of headers on or off
.help ?-all? ?PATTERN?   Show help text for PATTERN
.import FILE TABLE       Import data from FILE into TABLE
.imposter INDEX TABLE    Create imposter table TABLE on index INDEX
.indexes ?TABLE?         Show names of indexes
.limit ?LIMIT? ?VAL?     Display or change the value of an SQLITE_LIMIT
.lint OPTIONS            Report potential schema issues.
.load FILE ?ENTRY?       Load an extension library
.log FILE|off            Turn logging on or off.  FILE can be stderr/stdout
.mode MODE ?TABLE?       Set output mode
.nonce STRING            Disable safe mode for one command if the nonce matches
.nullvalue STRING        Use STRING in place of NULL values
.once ?OPTIONS? ?FILE?   Output for the next SQL command only to FILE
.open ?OPTIONS? ?FILE?   Close existing database and reopen FILE
.output ?FILE?           Send output to FILE or stdout if FILE is omitted
.parameter CMD ...       Manage SQL parameter bindings
.print STRING...         Print literal STRING
.progress N              Invoke progress handler after every N opcodes
.prompt MAIN CONTINUE    Replace the standard prompts
.quit                    Exit this program
.read FILE               Read input from FILE
.recover                 Recover as much data as possible from corrupt db.
.restore ?DB? FILE       Restore content of DB (default "main") from FILE
.save FILE               Write in-memory database into FILE
.scanstats on|off        Turn sqlite3_stmt_scanstatus() metrics on or off
.schema ?PATTERN?        Show the CREATE statements matching PATTERN
.selftest ?OPTIONS?      Run tests defined in the SELFTEST table
.separator COL ?ROW?     Change the column and row separators
.session ?NAME? CMD ...  Create or control sessions
.sha3sum ...             Compute a SHA3 hash of database content
.shell CMD ARGS...       Run CMD ARGS... in a system shell
.show                    Show the current values for various settings
.stats ?ARG?             Show stats or turn stats on or off
.system CMD ARGS...      Run CMD ARGS... in a system shell
.tables ?TABLE?          List names of tables matching LIKE pattern TABLE
.testcase NAME           Begin redirecting output to 'testcase-out.txt'
.testctrl CMD ...        Run various sqlite3_test_control() operations
.timeout MS              Try opening locked tables for MS milliseconds
.timer on|off            Turn SQL timer on or off
.trace ?OPTIONS?         Output each SQL statement as it is run
.vfsinfo ?AUX?           Information about the top-level VFS
.vfslist                 List all available VFSes
.vfsname ?AUX?           Print the name of the VFS stack
.width NUM1 NUM2 ...     Set minimum column widths for columnar output
```

### Show
Dos possíveis comandos acima, chamo a atenção para o `.show`, responsável por apresentar as configurações padrões de algumas propriedades relacionadas principalmente com a visualização (output) das tabelas do database. Todas elas podem ser alteradas por meio dos comandos correspondentes.
```
sqlite> .show
        echo: off
         eqp: off
     explain: auto
     headers: off
        mode: list
   nullvalue: ""
      output: stdout
colseparator: "|"
rowseparator: "\n"
       stats: off
       width:
    filename: :memory:
```

### Quit
Para sair ou finalizar o prompt do SQLite, basta utilizar o comando `.quit`.
```
sqlite> .quit

```

### Problema com comandos inseridos incorretamente
Um comando que possivelmente pode te poupar certo tempo é o comando `ctrl` + `D`. Para finalizar automaticamente o *statement* atual.  
Se algum comando inválido é inserido, ao pressionar `ENTER`, ao invés de executar o comando, o prompt exibe uma nova linha em branco. E não adianta tentar executar um novo comando, pois é como se esse "novo comando" fosse na verdade apenas a continuação - em uma nova linha, gerada pelo `ENTER` - daquele que foi digitado anteriormente errado.
Observe um exemplo:
```
sqlite> quit
   ...> 
   ...> .quit
   ...> 
Error: in prepare, near "quit": syntax error (1)
```
A mensagem de erro e o retorno para o terminal só ocorrem após executar o comando `ctrl` + `D`.  
Uma outra forma de contornar essa situação é utilizar `;`. Ele indica para o prompt que um *statement* chegou ao fim.
```
sqlite> quite
   ...> 
   ...> .quite
   ...> ;
Error: in prepare, near "quite": syntax error (1)
sqlite>
```
Note que diferentemente de `ctrl` + `D`, utilizar `;` faz com que o prompt não se encerre.

## Sintaxe
Como citado anteriormente, o prompt espera receber como input um *comando* ou um *statement*, daqui em diante referido como *declaração*. Os comandos como já vistos, iniciam-se com um `.` (ponto final) seguido do comando propriamente dito, em letras minúsculas.  
As declarações se iniciam com uma palavra-chave, tais como `SELECT`, `INSERT`, `UPDATE`, `DELETE`, etc, e finalizadas com um `;` (ponto e vírgula).  
No caso das declarações, ou mais precisamente, dos identificadores que as compõe, o SQLite é insensível a maiúsculas e minúsculas. Isto é, tanto as palavras-chave quanto o nome de tabelas, restrições e tipos podem ser escritos em maiúscula ou minúscula, ou utilizando ambas ao mesmo tempo.  
Entretanto existe uma regra que não é opcional, todo identificador deve iniciar com uma letra ou `_` (sublinhado). Além disso, são válidos apenas o sublinhado `_` e caracteres alfanuméricos.  
Declarações ainda podem ser feitas utilizando múltiplas linhas e espaço em branco **entre** cada um dos identificadores.  
Todos os exemplos abaixo representam declarações válidas.
```SQL
CREATE TABLE nome_da_tabela(coluna1 text);
```
```SQL
CREATE TABLE nomeDaTabela(
  COLUNAS text);
```
```SQL
CREATE TABLE Tabela (
    NomeDaColuna TEXT
);
```

## Convenções e boas práticas para nomenclatura
Se você observar o exemplo anterior, notará que o SQLite aceita uma boa variedade de combinações para se utilizar na nomenclatura de tabelas e colunas. Qual seria, então, o jeito correto de nomear tabelas e colunas - se é que ele existe?  
De fato, não existe uma regra universal, mas ao decorrer dos anos parece que alguns padrões foram se tornando mais comuns e mais reproduzidos do que outros. O link abaixo traz uma discussão a respeito desse aspecto:  
[Stackoverflow - database table and column naming conventions](https://stackoverflow.com/questions/7662/database-table-and-column-naming-conventions)

O mais importante, deixando de lado um pouco as discussões, é ser consistente depois que uma convenção for adotada. Mas se você estiver se sentindo inseguro sobre qual delas adotar, o que eu recomendaria seria algo como o exemplo abaixo:
```SQL
CREATE TABLE NomeDaTabelaSingular (
    NomeDaColunaSingular TEXT
);
```
Ou seja, PascalCase para ambos os nomes de tabela e coluna, bem como mantendo as duas no singular e evitando tanto quanto possível o uso de sublinhado \``_`\` e numerais.

## Data Type
Todos os diversos tipos de dados aceitos pelo SQLite são condensados em apenas 5 para compor suas classes de armazenamento, são eles: 
<table>
  <tr>
    <td>NULL</td>
    <td>Para tipos nulos e valores inexistentes.</td>
  </tr>
  <tr>
    <td>INTEGER</td>
    <td>Para números inteiros assinalados ou não.</td>
  <tr>
    <td>REAL</td>
    <td>Para números decimais ou flutuantes.</td>
  </tr>
  <tr>
    <td>TEXT</td>
    <td>Para caracteres e strings.</td>
  </tr>
  <tr>
    <td>BLOB</td>
    <td>Para grandes objetos binários.</td>
  </tr>
</table>
Um ponto que talvez seja importante observar é a forma como o SQLite maneja e atribui cada uma dessas classes aos objetos armazenados. Ao invés de associar o tipo do objeto à coluna que o armazena, o SQLite atribui o tipo do objeto ao seu próprio valor.  
Nesse sentido, o SQLite trabalha com um conceito de *type affinity* para as suas colunas. No qual, cada coluna possuí um tipo recomendado para os objetos que nela serão armazenados. E quando possível, realizará conversões de acordo com esse tipo. Entretanto esses tipos são recomendados, não requeridos. O que na prática permite que uma mesma coluna seja capaz de armazenar diferentes tipos de classes de objetos. Algo que seria impossível em outros tipos de database com uma tipagem mais rígida.  

## Primeiros passos
Após essa pequena introdução, é chegada a hora de abordar finalmente a parte prática. Comecemos com a criação do banco de dados.

### Criando um banco de dados
Criar um novo banco de dados em SQLite é extremamente simples. À partir do terminal, basta inserir `sqlite3` seguido de um espaço e do nome desejado para o banco de dados, seguido por fim da extensão `.db`. Observe o exemplo abaixo:
```
$ sqlite3 ExemploDeDataBase.db
```

### Verificando
Apesar do comando anterior criar uma nova base de dados, ele não cria o arquivo, mas você pode verificar se o banco de dados foi criado corretamente, checando se o prompt do SQLite foi aberto. Como no exemplo abaixo.
```
SQLite version 3.37.2 2022-01-06 13:25:41
Enter ".help" for usage hints.
sqlite>
```
Uma outra forma de verificar a existência da base de dados é utilizar o comando `.databases`, que retornará uma lista de todas as bases de dados que estão atualmente com a conexão aberta.
```
sqlite> .databases
```
O comando também cria automaticamente um arquivo `.db` vazio com o nome da base de dados, dentro do diretório.

### Criando Tabela
Para se criar uma tabela bastaria apenas a seguinte sintaxe:
```SQL
CREATE TABLE NomeDaTabela (NomeDaColuna);
```
Entretanto talvez não seja a melhor prática, tendo em vista que o SQLite nos oferece algumas opções para customizarmos nossa tabela.  
Um outro exemplo, portanto, de se criar uma tabela, talvez pudesse ser algo como:
```SQL
CREATE TABLE IF NOT EXISTS NomeDaTabela (
  NomeDaColunaUm INTEGER PRIMARY KEY AUTOINCREMENT,
  NomeDaColunaDois TEXT NOT NULL,
  NomeDaColunaTres INTEGER DEFAULT 0
) WITHOUT ROWID;
```
Na primeira linha: iniciamos com as palavras-chave `CREATE TABLE`, para se criar uma nova tabela. `IF NOT EXISTS`, como já discutido, é opcional, mas sem ele, a tentativa de se criar uma nova tabela com um nome que já existe resultará em um erro. Por fim, acrescentamos o nome da tabela.
Em seguida, entre parênteses, detalhamos cada uma das colunas que desejamos para a nossa tabela, atribuindo um nome, seguido do tipo sugerido para a respectiva coluna.  
Além disso, o SQLite permite que apliquemos algumas restrições às colunas, reforçando desse modo a conformidade e a confiabilidade do banco de dados que estamos criando. Abaixo pode-se observar palavras-chave disponíveis no SQLite, que atuam como restrições à uma determinada tabela:
<table>
  <tr>
    <td>PRIMARY KEY</td>
    <td>Fornece uma identificação única para cada linha/registro da tabela.</td>
  </tr>
  <tr>
    <td>NOT NULL</td>
    <td>Garante que a coluna não apresente valores do tipo NULL.</td>
  </tr>
  <tr>
    <td>DEFAULT</td>
    <td>Especifica um valor padrão para a coluna a ser utilizado se nenhum outro for estipulado.</td>
  <tr>
    <td>UNIQUE</td>
    <td>Garante que todos os valores da coluna sejam diferentes.</td>
  </tr>
  <tr>
    <td>CHECK</td>
    <td>Garante que todos os valores da coluna satisfazem um determinada condição.</td>
  </tr>
</table>

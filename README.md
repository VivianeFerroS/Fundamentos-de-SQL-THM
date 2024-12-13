# Fundamentos-de-SQL-THM
![image](https://github.com/user-attachments/assets/3ff6a2cf-7451-422e-85b6-1bd6b02b97c1)

<img src="https://tryhackme-badges.s3.amazonaws.com/VivianeFerro.png" alt="Your Image Badge" />
## Tarefa 1

## Introdução


Segurança cibernética é um tópico amplo que abrange uma ampla gama de assuntos, mas poucos deles são tão onipresentes quanto bancos de dados. Quer você esteja trabalhando para proteger um aplicativo da web, trabalhando em um SOC e usando um SIEM , configurando autenticação de usuário/controle de acesso ou usando ferramentas de análise de malware/detecção de ameaças (a lista continua), você estará de alguma forma contando com bancos de dados. Por exemplo, no lado ofensivo da segurança, ele pode nos ajudar a entender melhor as vulnerabilidades de SQL , como injeções de SQL , e criar consultas que nos ajudem a adulterar ou recuperar dados dentro de um serviço comprometido. Por outro lado, no lado defensivo, ele pode nos ajudar a navegar por bancos de dados e encontrar atividades suspeitas ou informações relevantes; ele também pode nos ajudar a proteger melhor um serviço implementando restrições quando necessário.

Como os bancos de dados são onipresentes, é importante entendê-los, e esta sala será seu primeiro passo nessa direção. Passaremos pelos conceitos básicos dos bancos de dados, cobrindo termos-chave, conceitos e diferentes tipos antes de nos familiarizarmos com SQL .


## Tarefa 2
## Bases de dados 101

### Apresentando Bancos de Dados
Ok, então você foi informado sobre o quão importantes eles são. Agora, é hora de entender o que eles são em primeiro lugar. Como mencionado na introdução, bancos de dados são tão onipresentes que você provavelmente interage com sistemas que os usam. Bancos de dados são uma coleção organizada de informações ou dados estruturados que são facilmente acessíveis e podem ser manipulados ou analisados. Esses dados podem assumir muitas formas, como dados de autenticação do usuário (como nomes de usuário e senhas), que são armazenados e verificados ao autenticar em um aplicativo ou site (como TryHackMe, por exemplo), dados gerados pelo usuário em mídias sociais (como Instagram e Facebook) onde dados como postagens do usuário, comentários, curtidas etc. são coletados e armazenados, bem como informações como histórico de exibição que é armazenado por serviços de streaming como Netflix e usado para gerar recomendações. 

Tenho certeza de que você entendeu: bancos de dados são usados ​​extensivamente e podem conter muitas coisas diferentes. Não são apenas empresas de grande porte que usam bancos de dados. Empresas de menor porte, ao se estabelecerem, quase certamente terão que configurar um banco de dados para armazenar seus dados. Falando em tipos de bancos de dados, vamos dar uma olhada agora em quais são eles.


### Diferentes tipos de bancos de dados
Agora faz sentido que algo seja usado por tantos e por (relativamente) tanto tempo que haveria vários tipos de implementações. Existem alguns tipos diferentes de bancos de dados que podem ser construídos, mas para esta sala introdutória, vamos nos concentrar nos dois tipos principais: bancos de dados relacionais (também conhecidos como SQL ) vs bancos de dados não relacionais (também conhecidos como NoSQL). 

![image](https://github.com/user-attachments/assets/b43fb4b0-fd64-47cf-bd43-8451365bd361)

**Bancos de dados relacionais:**  armazenam dados estruturados, o que significa que os dados inseridos neste banco de dados seguem uma estrutura. Por exemplo, os dados coletados sobre um usuário consistem em first_name, last_name, email_address, username e password. Quando um novo usuário entra, uma entrada é feita no banco de dados seguindo esta estrutura. Esses dados estruturados são armazenados em linhas e colunas em uma tabela (tudo isso será abordado em breve); relacionamentos podem ser feitos entre duas ou mais tabelas (por exemplo, user e order_history), daí o termo bancos de dados relacionais.

**Bancos de dados não relacionais:** Em vez de armazenar dados da maneira acima, armazene os dados em um formato não tabular. Por exemplo, se os documentos estão sendo digitalizados, o que pode conter vários tipos e quantidades de dados, e são armazenados em um banco de dados que exige um formato não tabular. Aqui está um exemplo de como isso pode parecer: 
`{
    _id: ObjectId("4556712cd2b2397ce1b47661"),
    name: { first: "Thomas", last: "Anderson" },
    date_of_birth: new Date('Sep 2, 1964'),
    occupation: [ "The One"],
    steps_taken : NumberLong(4738947387743977493)
}`

Em termos de qual banco de dados deve ser escolhido, sempre se resume ao contexto em que o banco de dados será usado. Bancos de dados relacionais são frequentemente usados ​​quando os dados armazenados serão recebidos de forma confiável em um formato consistente, onde a precisão é importante, como ao processar transações de comércio eletrônico.  Bancos de dados não relacionais, por outro lado, são mais bem usados ​​quando os dados recebidos podem variar muito em seu formato, mas precisam ser coletados e organizados no mesmo lugar, como plataformas de mídia social coletando conteúdo gerado pelo usuário.

### Tabelas, Linhas e Colunas
Agora que definimos os dois tipos primários de bancos de dados, vamos nos concentrar em bancos de dados relacionais. Começaremos explicando tabelas , linhas e colunas . Todos os dados armazenados em um banco de dados relacional serão armazenados em uma tabela ; por exemplo, uma coleção de livros em estoque em uma livraria pode ser armazenada em uma tabela chamada “Livros”. 
![image](https://github.com/user-attachments/assets/c4d0d4bc-2e0a-4083-84a8-79f02a54cc0c)

Ao criar esta tabela, você precisaria definir quais informações são necessárias para definir um registro de livro, por exemplo, “id”, “Name” e “Published_date”. Essas seriam suas colunas ; quando essas colunas estiverem sendo definidas, você também definiria qual tipo de dado essa coluna deve conter; se uma tentativa for feita para inserir um registro em um banco de dados onde o tipo de dado não corresponde, ela será rejeitada. Os tipos de dados que podem ser definidos podem variar dependendo do banco de dados que você está usando, mas os principais tipos de dados usados ​​por todos incluem Strings (uma coleção de palavras e caracteres), Integers (números), floats/decimais (números com um ponto decimal) e Times/Datas. 

Uma vez que uma tabela foi criada com as colunas definidas, o primeiro registro seria inserido no banco de dados, por exemplo, um livro chamado “Android Security Internals” com um id de “1” e uma data de publicação de “2014-10-14”. Uma vez inserido, esse registro seria representado como uma linha .

### Chaves primárias e estrangeiras
Depois que uma tabela foi definida e preenchida, mais dados podem precisar ser armazenados. Por exemplo, queremos criar uma tabela chamada “Autores” que armazene os autores dos livros vendidos na loja. Aqui está um exemplo muito claro de um relacionamento. Um livro (armazenado na tabela Livros) é escrito por um autor (armazenado na tabela Autores). Se quiséssemos consultar um livro em nossa história, mas também ter o autor desse livro retornado, nossos dados precisariam ser relacionados de alguma forma; fazemos isso com chaves. Existem dois tipos de chaves :
![image](https://github.com/user-attachments/assets/6d9783c8-16a3-4b27-8770-a1da9d0e4df4)

**Chaves primárias :** Uma chave primária é usada para garantir que os dados coletados em uma determinada coluna sejam únicos. Ou seja, precisa haver uma maneira de identificar cada registro armazenado em uma tabela, um valor exclusivo para esse registro e não repetido por nenhum outro registro nessa tabela. Pense nos números de matrícula em uma universidade; esses são números atribuídos a um aluno para que eles possam ser identificados exclusivamente nos registros (já que às vezes os alunos podem ter o mesmo nome). Uma coluna deve ser escolhida em cada tabela como uma chave primária; em nosso exemplo, "id" faria mais sentido, pois um id foi criado exclusivamente para cada livro, onde, como os livros podem ter a mesma data de publicação ou (em casos mais raros) título do livro. Observe que só pode haver uma coluna de chave primária em uma tabela.

**Chaves estrangeiras :** Uma chave estrangeira é uma coluna (ou colunas) em uma tabela que também existe em outra tabela dentro do banco de dados e, portanto, fornece um link entre as duas tabelas. Em nosso exemplo, pense em adicionar um campo “author_id” à nossa tabela “Books”; isso então agiria como uma chave estrangeira porque o author_id em nossa tabela Books corresponde à coluna “id” na tabela author. Chaves estrangeiras são o que permitem os relacionamentos entre diferentes tabelas em bancos de dados relacionais. Observe que pode haver mais de uma coluna de chave estrangeira em uma tabela.

### Respostas das perguntas
![image](https://github.com/user-attachments/assets/9cd3c3c2-6e70-4c58-bd7f-f69af91d26a9)
* Quando os dados variam muito em termos de formato, como por exemplo, textos, imagens, listas, e diferentes tipos de informações que não têm uma estrutura fixa, faz mais sentido usar um banco de dados não relacional. Isso porque bancos de dados como NoSQL são mais flexíveis, permitindo armazenar e acessar informações de formas diferentes, sem depender de uma estrutura rígida como as tabelas dos bancos de dados relacionais.

--------------------------------------------------
![image](https://github.com/user-attachments/assets/d0e6c1ee-b59f-4812-937f-5f491f1cddec)
*  Bancos de dados relacionais são ótimos para garantir a consistência dos dados, através de regras como integridade referencial, que ajudam a manter a qualidade e a confiabilidade das informações.
-------------------------------------------------
![image](https://github.com/user-attachments/assets/c447ca9d-cdf4-4bdd-8de2-502e481dc397)
* Sendo em um banco de dados relacional, os dados são armazenados em tabelas, que são formadas por linhas e colunas. Cada coluna representa um atributo, como "id", "Nome" ou "Data de Publicação". Já cada linha na tabela representa um registro específico.
----------------------------------------------------
![image](https://github.com/user-attachments/assets/bb6ad6ae-ab06-4ee1-80f5-0199ebaf4a83)
* Uma Foreign key é usada para criar um vínculo entre duas tabelas, permitindo relacionar informações de uma tabela com outra. Isso é útil quando queremos manter a consistência dos dados e garantir que as informações estejam conectadas de forma correta.
---------------------------------------------------
![image](https://github.com/user-attachments/assets/6512e00e-5b71-469b-9d79-afe5e5a7a5d3)




## Tarefa 3

## SQL

### O que é SQL ?

Agora, tudo isso teoricamente parece ótimo, mas na prática, como os bancos de dados funcionam? Como você faria sua primeira tabela e a preencheria com dados? O que você usaria? Os bancos de dados geralmente são controlados usando um Sistema de Gerenciamento de Banco de Dados (DBMS). Servindo como uma interface entre o usuário final e o banco de dados, um DBMS é um programa de software que permite aos usuários recuperar, atualizar e gerenciar os dados que estão sendo armazenados. Alguns exemplos de DBMSs incluem MySQL, MongoDB, Oracle Database e Maria DB. 

![image](https://github.com/user-attachments/assets/78dcc331-b4a7-49d1-b294-1d4cd66918af)

A interação entre o usuário final e o banco de dados pode ser feita usando SQL (Structured Query Language). SQL é uma linguagem de programação que pode ser usada para consultar, definir e manipular os dados armazenados em um banco de dados relacional. 

### Os benefícios do SQL e dos bancos de dados relacionais
SQL é quase tão onipresente quanto os próprios bancos de dados, e por um bom motivo. Aqui estão alguns dos benefícios que vêm com o aprendizado e o uso do SQL :

* **É rápido :**  bancos de dados relacionais (também conhecidos como aqueles nos quais o SQL é usado) podem retornar grandes lotes de dados quase instantaneamente devido ao pouco espaço de armazenamento usado e às altas velocidades de processamento. 

* **Fácil de aprender:**  Diferentemente de muitas linguagens de programação, SQL é escrito em inglês simples, o que o torna muito mais fácil de aprender. A natureza altamente legível da linguagem significa que os usuários podem se concentrar em aprender as funções e a sintaxe.

* **Confiável:**  Como mencionado anteriormente, bancos de dados relacionais podem garantir um nível de precisão quando se trata de dados, definindo uma estrutura rigorosa na qual os conjuntos de dados devem se enquadrar para serem inseridos.

* **Flexível:**  o SQL fornece todos os tipos de recursos quando se trata de consultar um banco de dados; isso permite que os usuários executem grandes tarefas de análise de dados com muita eficiência.
![image](https://github.com/user-attachments/assets/5bf96726-89b1-4620-b549-7b7c10d89b78)

![image](https://github.com/user-attachments/assets/c078dd91-be43-41a4-8b4d-df4bd4c62f56)

* `mysql` é o comando para iniciar o cliente MySQL, permite que eu possa me conectar a um servidor MySQL e executar consultas diretamente no terminal.
`-u root` **-u** é a abreviação de user, e **root** é o nome do usuário que você estamos acessando o MySQL. O root é o usuário administrador do MySQL, que tem todos os privilégios no banco de dados.
`-p` indica que o MySQL deve solicitar uma senha ao tentar fazer a conexão. Quando esse parâmetro é usado, o terminal pedirá eu digite a senha do usuário especificado (neste caso, do usuário root), que a senha digitada foi `tryhackme`.
* Se não incluirmos o `-p`, o MySQL tentará fazer login sem solicitar uma senha. Isso pode funcionar se o usuário não tiver uma senha definida, mas, por questões de segurança, é muito comum configurar uma senha, especialmente para o usuário root.

### Respostas das perguntas
![image](https://github.com/user-attachments/assets/48e3f3ff-5945-4708-8819-68e1aff7758c)
* O DBMS é como uma ponte entre o usuário e o banco de dados. É como se fosse um assistente que entende o que eu quero fazer com os dados e ele realiza essas operações de forma organizada e segura. Ele facilita a comunicação entre eu, que precisa dos dados, e o banco de dados em si.
----------------------------------------------------------------------
![image](https://github.com/user-attachments/assets/94d30249-c7c5-43e4-8119-59474dbf208f)


## Tarefa 4

## Instruções de banco de dados e tabelas

### Hora de aprender
Agora, a parte divertida! É hora de começar a aprender SQL e como usá-lo para interagir com bancos de dados. Nesta tarefa, começaremos aprendendo a usar instruções de banco de dados e tabela. Afinal, são essas instruções que precisamos para criar inicialmente nossos bancos de dados/tabelas e começar. 
Declarações de banco de dados

### **CRIAR BANCO DE DADOS**
Se um novo banco de dados for necessário, o primeiro passo que você daria seria criá-lo. Isso pode ser feito em SQL usando a **`CREATE DATABASE`**. Isso seria feito usando a seguinte sintaxe:

![image](https://github.com/user-attachments/assets/fe13b1ec-85d5-4850-bcfe-7429b91b0664)

**Explicação Bonus** : `CREATE DATABASE` Diz ao MySQL que queremoscriar um novo banco de dados. e `database_name` É o nome que vamos dar ao novo banco de dados. Poderemos escolher qualquer nome, desde que seja único no servidor.

Execute o seguinte comando para criar um banco de dados chamado `thm_bookmarket_db`:

![image](https://github.com/user-attachments/assets/fc309d9d-f39e-4104-b2f0-e071543522eb)

Explicação bonus: `CREATE DATABASE`: Esse é o comando que diz ao MySQL que queremos criar um novo banco de dados. É como reservar um espaço onde você vai armazenar várias tabelas que contêm os dados organizados de um sistema. `thm_bookmarket_db`: Esse é o nome do banco de dados em que estamos criando. Neste caso, o nome é `thm_bookmarket_db`, que indica ser um banco de dados para um sistema chamado "**bookmarket**" ( para gerenciar livros). 


### MOSTRAR BANCOS DE DADOS

Agora que criamos um banco de dados, podemos visualizá-lo usando a instrução `SHOW DATABASES`. A instrução `SHOW DATABASES` retornará uma lista de bancos de dados presentes. Execute a instrução da seguinte forma:

![image](https://github.com/user-attachments/assets/662b9937-66e6-4440-bad1-7d7fd0bfe615)

**Explicação Bonus** : `SHOW DATABASES;` Esse comando exibe uma lista de todos os bancos de dados disponíveis no servidor MySQL ao qual você está conectado.
No resultado, você vê uma lista de bancos de dados. Aqui está uma breve explicação sobre cada um deles:
* `THM{...}`: Está relacionado a uma atividade que estamos fazendo, mais a frente usaremos ela. 
* `database_name:` É um banco de dados que acabamos de criar.
* `information_schema`: Um banco de dados do sistema, que contém informações sobre todos os outros bancos de dados. Ele é útil para visualizar a estrutura dos bancos de dados, como tabelas, colunas, etc.
* `mysql`: Outro banco de dados do sistema, onde são armazenadas informações de configuração do próprio MySQL, como usuários e também permissões.
* `performance_schema`: Banco de dados que contém informações sobre o desempenho do servidor MySQL, é útil para otimização e monitoramento.
* `sys`: Um banco de dados que contém visões úteis para simplificar o diagnóstico e monitoramento do sistema MySQL.
* `task_4_db`: Banco de dados criado em uma tarefa específica que iremos fazer mais a frente.
* `thm_bookmarket_db`: Esse é o banco de dados que criamos para armazenar informações relacionadas ao "**bookmarket**".
* `thm_books`: Outro banco de dados, relacionado a livros, criado para uma tarefa específica iremos usá-lo mais adiante.
* `thm_books2`: Iremos usá-lo em uma tarefa
* `tools_db`: Um banco de dados relacionado a ferramentas.

### USAR BANCO DE DADOS
Depois que um banco de dados é criado, você pode querer interagir com ele. Antes de podermos interagir com ele, precisamos dizer ao mysql com qual banco de dados gostaríamos de interagir (para que ele saiba em qual banco de dados executar consultas subsequentes). Para definir o banco de dados que acabamos de criar como o banco de dados ativo, executaríamos a USEinstrução da seguinte forma (certifique-se de executar isso em sua máquina):


![image](https://github.com/user-attachments/assets/5e06efa5-9578-4f53-b38b-5c2739b51746)


**Explicação Bonus** : `USE` esse comando é usado para selecionar qual banco de dados queremos usar no momento. Ele indica ao MySQL que, a partir de agora todas as operações que executarmos ( sendo como criar tabelas, inserir dados, etc.) serão feitas no banco de dados que especificarmos. `thm_bookmarket_db` é o nome do banco de dados em que  estamos selecionando. Neste caso, é o banco que criamos anteriormente para armazenar informações do "**bookmarket**".
O `Database changed` é a mensagem que confirma que o banco de dados ativo agora é o `thm_bookmarket_db`. Ou seja, qualquer comando que digitarmos a partir deste ponto vai interagir com esse banco de dados.



### DROP DATABASE
Uma vez que um banco de dados não é mais necessário (talvez tenha sido criado para fins de teste, ou não seja mais necessário), ele pode ser removido usando a DROPinstrução. Para remover um banco de dados, usaríamos a seguinte sintaxe de instrução (embora, no nosso caso, queiramos manter nosso banco de dados, então não há necessidade de executar este você mesmo!):
![image](https://github.com/user-attachments/assets/3e761981-7691-4d33-a9eb-3704e7e8d9b1)

Explicação Bonus: `DROP DATABASE` É um comando usado para deletar um banco de dados. Isso significa que todos os dados, tabelas e estruturas armazenadas no banco de dados serão removidos permanentemente. `database_name` É banco que desejamos excluir. No exemplo mostrado no print, estamos deletando o banco de dados chamado database_name.


## Declarações de tabela

Agora que você pode criar, listar, usar e remover bancos de dados, é hora de examinar como preencheríamos esses bancos de dados com tabelas e interagiríamos com elas. 

### CRIAR TABELA

Seguindo a lógica das instruções do banco de dados, criar tabelas também usa uma CREATEinstrução. Uma vez que um banco de dados esteja ativo (você tenha executado a USEinstrução nele), uma tabela pode ser criada dentro dele usando a seguinte sintaxe de instrução:

`mysql> CREATE TABLE example_table_name (
    example_column1 data_type,
    example_column2 data_type,
    example_column3 data_type
);
`

Como você pode ver, há um pouco mais envolvido aqui. Na tarefa Databases 101, abordamos como e quando uma tabela é criada; deve ser decidido quais colunas formarão um registro nessa tabela, bem como qual tipo de dado deve estar contido nessa coluna. É isso que é representado por essa sintaxe aqui. No exemplo, há 3 colunas de exemplo, mas o SQL suporta muitas (mais de 1000). Vamos tentar preencher nossa thm_bookmarket_dbtabela usando a seguinte instrução:

![image](https://github.com/user-attachments/assets/9a5d5882-ab37-4e88-8f8f-5129d91578ad)

Esta declaração criará uma tabela book_inventorycom três colunas:  book_id, book_namee publication_date. book_idé um INT(Integer) como ele deve ser sempre apenas um número, AUTO_INCREMENTestá presente, significando que o primeiro livro inserido receberia book_id 1, o segundo livro inserido receberia book_id 2, e assim por diante. Finalmente,  book_idé definido como o PRIMARY KEYcomo será a maneira como identificaremos exclusivamente um registro de livro em nossa tabela (e um primário deve estar presente em uma tabela). 
Book_name tem o tipo de dado VARCHAR(255), o que significa que pode ter caracteres variáveis ​​(texto/números/pontuação) e um limite de 255 caracteres é definido e NOT NULL, o que significa que não pode estar vazio (então se alguém tentasse inserir um registro nesta tabela, mas o book_name estivesse vazio, ele seria rejeitado). Publication_date é definido como o tipo de dado DATE.

### MOSTRAR TABELAS

Assim como podemos listar bancos de dados usando uma instrução SHOW, também podemos listar as tabelas em nosso banco de dados ativo no momento (o banco de dados no qual usamos a instrução USE pela última vez). Execute o comando a seguir e você deverá ver a tabela que acabou de criar:

![image](https://github.com/user-attachments/assets/4a1639f5-3adc-41db-81c2-ee60b03d19a8)

### DESCREVER 
Se quisermos saber quais colunas estão contidas em uma tabela (e seus tipos de dados), podemos descrevê-las usando o DESCRIBEcomando (que também pode ser abreviado para DESC). Descreva a tabela que você acabou de criar usando o seguinte comando:

![image](https://github.com/user-attachments/assets/43d5ec73-e1ae-4797-926c-1f3a149e2796)

### ALTERAR 
Depois de criar uma tabela, pode chegar um momento em que sua necessidade pelo conjunto de dados muda, e você precisa alterar a tabela. Isso pode ser feito usando a ALTERinstrução. Vamos agora imaginar que decidimos que realmente queremos ter uma coluna em nosso inventário de livros que tenha a contagem de páginas de cada livro. Adicione isso à nossa tabela usando a seguinte instrução:

![image](https://github.com/user-attachments/assets/88d7e568-22e2-46ce-a9e1-593b084c20e9)

A instrução ALTER pode ser usada para fazer alterações em uma tabela, como renomear colunas, alterar o tipo de dados em uma coluna ou remover uma coluna. 


### DERRUBAR 
Semelhante à remoção de um banco de dados, você também pode remover tabelas usando a instrução DROP. Não precisamos fazer isso, mas a sintaxe que você usaria para isso é:

`DROP TABLE table_name;`

### Respostas das perguntas
![image](https://github.com/user-attachments/assets/e7248d2a-25cb-4ce2-8164-5f0841844d72)
![image](https://github.com/user-attachments/assets/950e031b-0610-4a25-9e9b-bf3717bb1ffa)

-------------------------------------------------
![image](https://github.com/user-attachments/assets/5a1b7303-dd92-46c1-a5ba-44c447b82be5)
* Comecei listando todos os bancos de dados disponíveis no servidor MySQL com o comando:

![image](https://github.com/user-attachments/assets/da2485dd-28aa-4ce2-a7b0-1ab85573188d)

* Em seguida, selecionei o banco de dados `task_4_db` para trabalhar com ele, usando o comando:
  
![image](https://github.com/user-attachments/assets/59153521-928d-4ff6-ba42-818bdec45f83)

* Para encontrar a resposta da nossa pergunta, precisei ver quais tabelas estavam disponíveis no banco de dados ativo, então usou o comando:

![image](https://github.com/user-attachments/assets/921c8206-88c3-4876-96c2-bf9df2caf668)

## Tarefa 5

## Operações CRUD

### CRUD
CRUD significa ( Create, Read, Update, Delete)  Criar , Ler , Atualizar e Excluir , que são consideradas as operações básicas em qualquer sistema que gerencia dados.

Vamos explorar todas essas operações diferentes ao trabalhar com MySQL .  Nas próximas duas tarefas, usaremos a tabela books que faz parte do banco de dados `thm_books` . Podemos acessá-la com a instrução `use thm_books`;.

### Criar Operação (INSERT)
A operação Create criará novos registros em uma tabela. No MySQL, isso pode ser feito usando a instrução `INSERT INTO`, conforme mostrado abaixo.

![image](https://github.com/user-attachments/assets/9ca99a14-fe8e-4499-90bc-74c05e516ed7)

Como podemos observar, a declaração `INSERT INTO`  especifica uma tabela, neste caso,  books , onde você pode adicionar um novo registro; as colunas id , name , published_date e description são os registros na tabela. Neste exemplo, um novo registro com um id de  1 , um nome de "Android Security Internals ", um published_date de " 2014-10-14 " e uma descrição afirmando " Android Security Internals provides a complete understanding of the security internals of Android devices " foi adicionado.


Observação: esta operação já existe no banco de dados, portanto não há necessidade de executar a consulta.

### Operação de leitura (SELECT)
A operação **Read** , como o nome sugere, é usada para ler ou recuperar informações de uma tabela. Podemos buscar uma coluna ou todas as colunas de uma tabela com a instrução `SELECT`, como mostrado no próximo exemplo.

![image](https://github.com/user-attachments/assets/07c57b6e-9b52-4321-963a-96e47a372a5b)

A `SELECT` de saída acima é seguida por um *símbolo indicando que todas as colunas devem ser recuperadas, seguido pela FROMcláusula e pelo nome da tabela, neste caso,  books .

Se quisermos selecionar uma coluna específica, como o nome e a descrição , devemos especificá-los em vez do *símbolo, conforme mostrado abaixo.

![image](https://github.com/user-attachments/assets/0194876c-24dd-4675-95fa-4da3090d8f85)

### Operação de atualização (UPDATE)
A operação Update modifica um registro existente dentro de uma tabela, e a mesma instrução, UPDATE, pode ser usada para isso.

![image](https://github.com/user-attachments/assets/799f7e6d-1257-4c23-8513-d7698faa4c65)

A UPDATE declaração especifica a tabela, neste caso,  books , e então podemos usar SETseguido pelo nome da coluna que iremos atualizar. A WHEREcláusula especifica qual linha atualizar quando a cláusula for atendida, neste caso, aquela com id 1 .

### Operação de exclusão (DELETE)
A operação delete remove registros de uma tabela. Podemos fazer isso com a DELETE instrução.

Nota: Não há necessidade de executar a consulta. Excluir esta entrada afetará o restante dos exemplos nas próximas tarefas.

![image](https://github.com/user-attachments/assets/c4e40d72-bfcb-4a23-ab1c-90f1691fea52)

Acima, podemos observar a DELETEdeclaração seguida da FROMcláusula , que nos permite especificar a tabela onde o registro será removido, neste caso, books , seguida da WHEREcláusula que indica que deve ser aquela onde o id é 1 .

### Resumo
Em resumo, os resultados das operações `CRUD` são fundamentais para operações de dados e ao interagir com bancos de dados. As instruções associadas a eles estão listadas abaixo.

* Criar (instrução `INSERT`) - Adiciona um novo registro à tabela.
* Ler (instrução `SELECT`) - Recupera registro da tabela.
* Atualizar (instrução `UPDATE`) - Modifica dados existentes na tabela.
* Excluir (instrução `DELETE`) - Remove o registro da tabela.
Essas operações nos permitem gerenciar e manipular dados de forma eficaz dentro de um banco de dados.

---------------------------------------------
![image](https://github.com/user-attachments/assets/31707395-4e5d-45d6-9aba-191c1063a1a0)

* Primeiro, defini `tools_db` como o banco de dados ativo, usando o comando:
`USE tools_db;`
* Usei o comando `SELECT` para visualizar todas as informações da tabela **hacking_tools** no **banco tools_db** :
`SELECT * FROM hacking_tools;`
* Precisei identificar a ferramenta correta de acordo com a pergunta, analisando a tabela retornada, eu encontrei a ferramenta com a ID 3 chamada WI-FI Pineapple que tem a descrição correspondente com o dispositivo usado para MITM. Nossa resposta correta é Wi-Fi Pineapple.
![image](https://github.com/user-attachments/assets/713aab73-5310-4b9d-acd8-a9147e367ff5)

--------------------------------------------
![image](https://github.com/user-attachments/assets/815647e4-e6fd-4acf-92b7-093c0de52817)
![image](https://github.com/user-attachments/assets/fecaf8ff-768a-464d-a832-ab8f3774ce89)


## Tarefa 6

## Cláusulas

Uma cláusula é uma parte de uma declaração que especifica os critérios dos dados que estão sendo manipulados, geralmente por uma declaração inicial. Cláusulas podem nos ajudar a definir o tipo de dados e como eles devem ser recuperados ou classificados. 

Em tarefas anteriores, já usamos algumas cláusulas, como FROM essa, que é usada para especificar a tabela que estamos acessando com nossa instrução e WHERE, que especifica quais registros devem ser usados.

Esta tarefa se concentrará em outras cláusulas: DISTINCT, GROUP BY, ORDER BY, e HAVING.

### Cláusula DISTINCT
A `DISTINCT` cláusula é usada para evitar registros duplicados ao fazer uma consulta, retornando apenas valores únicos.

Vamos usar uma consulta `SELECT * FROM books` e observar os resultados abaixo.

![image](https://github.com/user-attachments/assets/bab26a03-9bf6-469b-b0be-6be592c4e063)

A saída da consulta exibe todo o conteúdo da tabela books , e o registro Ethical Hacking é exibido duas vezes. Vamos executar a consulta novamente, mas, dessa vez, usando a DISTINCTcláusula .

![image](https://github.com/user-attachments/assets/61dccf28-ed89-4d5c-a20e-33f4591ed485)

 saída mostra que apenas cinco linhas são retornadas e apenas uma instância do registro Ethical Hacking é exibida.

### Cláusula GROUP BY
A GROUP BYcláusula agrega dados de vários registros e agrupa os resultados da consulta em colunas. Isso pode ser útil para agregar funções.

![image](https://github.com/user-attachments/assets/b3f365a1-111a-4fb4-8fe0-2be2b2783647)


No exemplo acima, os registros na tabela book são reagrupados pelo resultado da COUNTfunção. Já sabemos que Ethical hacking é listado duas vezes, então a contagem total  é 2, colocada no final, pois é  agrupada  por contagem.

### Cláusula ORDER BY
A ORDER BYcláusula pode ser usada para classificar os registros retornados por uma consulta em ordem crescente ou decrescente. Usar funções como ASCe DESC pode nos ajudar a fazer isso, como mostrado abaixo nos próximos dois exemplos.

ORDEM CRESCENTE
![image](https://github.com/user-attachments/assets/2c193901-55f8-42fe-a3ed-124646ef2c7e)

ORDEM DESCENDENTE
![image](https://github.com/user-attachments/assets/30eaea72-d368-4003-a451-946c93ec998f)

Podemos observar a diferença ao classificar em ordem crescente usando ASCe em ordem decrescente usando DESC, ambos usando a publised_date como referência.


### Cláusula HAVING
A HAVING cláusula é usada com outras cláusulas para filtrar grupos ou resultados de registros com base em uma condição. No caso de GROUP BY, ela avalia a condição para TRUE ou FALSE, diferentemente da WHEREcláusula HAVING filtra os resultados após a agregação ser realizada.

![image](https://github.com/user-attachments/assets/ce5c2125-920a-41b2-bb31-6eb40fdd725e)

No exemplo acima, podemos observar que a consulta retorna os livros com os nomes que contêm a palavra hack e a contagem adequada, como aprendemos antes.

![image](https://github.com/user-attachments/assets/0a5e0828-3fbd-43c5-be28-b0e196d9484e)
* `SELECT COUNT(DISTINCT category)` É o comando vai contar quantas categorias diferentes existem na coluna `category` da tabela
* `FROM hacking_tools` Especifica a tabela da qual estamos obtendo as informações.
![image](https://github.com/user-attachments/assets/e5aee822-68f4-41f5-8b81-3037f4eae2f8)

-------------------------------------------------
![image](https://github.com/user-attachments/assets/fbfb4807-a9bb-40eb-95d8-37a08ebbbef5)

Vamos por partes:
* `SELECT *`: Seleciona todas as colunas da tabela.
* `FROM hacking_tools`: Especifica a tabela hacking_tools.
* `ORDER BY name ASC`: Ordena os resultados pela coluna name em ordem crescente (ASC).
* `LIMIT 1`: Retorna apenas o primeiro registro da lista ordenada.
* 
![image](https://github.com/user-attachments/assets/7aea9a86-c5a2-4c32-8952-c74f78e42916)

----------------------------------------------------
![image](https://github.com/user-attachments/assets/70c18964-616c-45f4-864e-7521dc4c22ee)
* `ORDER BY name DESC`: Ordena os resultados pela coluna name em ordem decrescente (DESC).

![image](https://github.com/user-attachments/assets/1547e1db-d7b7-4c90-a280-0c767cb4583d)


## Tarefa 7

## Operadores

Ao trabalhar com SQL e lidar com lógica e comparações, os operadores são nossa maneira de filtrar e manipular dados de forma eficaz.  Entender esses operadores nos ajudará a criar consultas mais precisas e poderosas.  Nas próximas duas tarefas, usaremos a tabela books que faz parte do banco de dados thm_books2 . Podemos acessá-la com a instrução .  use thm_books2;

### Operadores Lógicos
Esses operadores testam a verdade de uma condição e retornam um valor booleano de TRUEor FALSE. Vamos explorar alguns desses operadores a seguir.

### Operador LIKE
O LIKEoperador é comumente usado em conjunto com cláusulas como WHEREpara filtrar padrões específicos dentro de uma coluna. Vamos continuar usando nosso DataBase para consultar um exemplo de seu uso.

![image](https://github.com/user-attachments/assets/50e61886-b3f0-4242-b919-f692a57426a1)

A consulta acima retorna uma lista de registros dos livros filtrados, mas aqueles que usam a WHEREcláusula que contém a palavra guia usando o LIKEoperador.

### Operador AND
O ANDoperador usa várias condições dentro de uma consulta e retorna TRUEse todas elas forem verdadeiras.

![image](https://github.com/user-attachments/assets/d6936a4c-2f8d-4235-9ab6-b2236e5c97f1)

A consulta acima retorna o livro com o nome Bug Bounty Bootcamp , que está na categoria de  Segurança Ofensiva .

### Operador OR
O ORoperador combina várias condições dentro de consultas e retorna TRUEse pelo menos uma dessas condições for verdadeira.

![image](https://github.com/user-attachments/assets/5d273453-c726-4491-8c6c-c612bec41de3)

A consulta acima retorna livros cujos nomes incluem Android  ou IOS .

### Operador NOT
O NOToperador inverte o valor de um operador booleano, permitindo-nos excluir uma condição específica.

![image](https://github.com/user-attachments/assets/851b8b96-64c3-4491-a572-3e3dafad3963)

A consulta acima retorna resultados onde a descrição não contém a palavra guia .

### Operador BETWEEN
O BETWEENoperador nos permite testar se um valor existe dentro de um intervalo definido .

![image](https://github.com/user-attachments/assets/c1cbd983-8727-44ea-bf2a-f0a3497bdcb6)

A consulta acima retorna livros cujo id está entre 2 e 4 .

### Operadores de comparação
Os operadores de comparação são usados ​​para comparar valores e verificar se eles atendem a critérios especificados.

### Operador Igual a
O = operador (Igual) compara duas expressões e determina se elas são iguais, ou pode verificar se um valor corresponde a outro em uma coluna específica.

![image](https://github.com/user-attachments/assets/1d865aff-2a5b-4f0a-aa98-0307ed05e585)

A consulta acima retorna o livro com o nome exato Designing Secure Software .

### Não é igual ao operador
O !=operador (não igual) compara expressões e testa se elas não são iguais; ele também verifica se um valor difere daquele dentro de uma coluna.

![image](https://github.com/user-attachments/assets/0c65298c-6a73-466f-ab6a-03997f8ecb77)

A consulta acima retorna livros, exceto aqueles cuja categoria é Segurança Ofensiva .

### Operador menor que
Operador menor que

O < operador (menor que) compara se a expressão com um determinado valor é menor que o fornecido

![image](https://github.com/user-attachments/assets/62feb6ef-4257-49c9-b815-45044968e024)

A consulta acima retorna livros que foram publicados antes de 1º de janeiro de 2020 .

### Operador Maior Que
O >operador (maior que) compara se a expressão com um determinado valor é maior que o fornecido.

![image](https://github.com/user-attachments/assets/b01aa476-381a-4257-a24c-c8be2b9d7d86)

A consulta acima retorna livros publicados depois de 1º de janeiro de 2020 .

### Operadores Menor ou Igual a e Maior   ou Igual a 
O <=operador (Menor que ou igual) compara se a expressão com um valor dado é menor que ou igual ao fornecido. Por outro lado, o >= operador (Maior que ou Igual) compara se a expressão com um valor dado é maior que ou igual ao fornecido. Vamos observar alguns exemplos de ambos abaixo.

![image](https://github.com/user-attachments/assets/ffb2a926-52bb-45e0-8c97-4d480e9e4f39)

A consulta acima retorna livros publicados em ou antes de 15 de novembro de 2021 .


![image](https://github.com/user-attachments/assets/d8266ff5-5c98-40e9-a478-8bff302534c8)

A consulta acima retorna livros que foram publicados em ou após 2 de novembro de 2021 .

![image](https://github.com/user-attachments/assets/f02c7963-ca79-4c14-82fc-4a92d32db38d)

* `SELECT *`: Seleciona todas as colunas da tabela.
* `FROM hacking_tools`: Especifica a tabela hacking_tools.
* `WHERE category = 'Multi-Tool'`: Filtra os resultados para incluir apenas os registros cuja categoria seja "Multi-Tool".
* `AND description LIKE '%pentesters%geeks%'`: Filtra a descrição para encontrar palavras-chave que mencionem "pentesters" e "geeks".

`SELECT * FROM hacking_tools  WHERE category = 'Multi-Tool' AND description LIKE '%pentesters%geeks%';`
![image](https://github.com/user-attachments/assets/a53acac5-309c-42c6-9d26-8ecd3747cd31)

----------------------------------------------
![image](https://github.com/user-attachments/assets/13f90e2c-6180-48bc-9985-2a5dc898944d)
* `SELECT category`: Seleciona a coluna category que contém as categorias das ferramentas.
* `FROM hacking_tools`: Especifica a tabela hacking_tools da qual os dados serão recuperados.
* `WHERE amount >= 300`: Filtra as ferramentas cuja quantidade (amount) seja maior ou igual a 300
![image](https://github.com/user-attachments/assets/8213232e-a995-4095-8b0a-338835f577b9)
--------------------------------------------------

![image](https://github.com/user-attachments/assets/549b3969-0cc4-46ce-847f-8d514251289b)
* ` SELECT name`: Seleciona a coluna name, que contém o nome da ferramenta.
* `FROM hacking_tools`: Especifica a tabela hacking_tools da qual os dados serão recuperados.
* `WHERE category = 'Network intelligence':` Filtra para incluir apenas os registros da categoria "Inteligência de rede".
* `AND amount < 100:` Filtra para encontrar ferramentas cujo valor (amount) seja menor que 100.
![image](https://github.com/user-attachments/assets/10f7b9be-0726-48ec-8b94-df4a2337e79c)


## Tarefa 8
## Funções
Ao trabalhar com Dados, funções podem nos ajudar a simplificar consultas e operações e manipular dados. Vamos explorar algumas dessas funções a seguir.

### Funções de String

Funções de strings realizam operações em uma string, retornando um valor associado a ela.

### Função CONCAT()

Esta função é usada para adicionar duas ou mais strings juntas. É útil para combinar texto de colunas diferentes.

![image](https://github.com/user-attachments/assets/bda8af76-0ab8-43fc-96eb-345774f81b01)

Esta consulta concatena as colunas de nome e categoria da tabela books em uma única chamada book_info .

### Função GROUP_CONCAT()

Esta função pode nos ajudar a concatenar dados de várias linhas em um campo. Vamos explorar um exemplo de seu uso.

![image](https://github.com/user-attachments/assets/82bf9234-6a46-42d5-8e89-076c195a15ca)

A consulta acima agrupa os livros por categoria e concatena os títulos dos livros dentro de cada categoria em uma única string .

### Função SUBSTRING()

Esta função recuperará uma substring de uma string dentro de uma consulta, começando em uma posição determinada. O comprimento desta substring também pode ser especificado.

![image](https://github.com/user-attachments/assets/548eb28b-8e2b-45a5-a864-33385d41ebcf)

Na consulta acima, podemos observar como ela extrai os quatro primeiros caracteres da coluna published_date e os armazena na  coluna published_year .

### Função LENGTH()

Esta função retorna o número de caracteres em uma string. Isso inclui espaços e pontuação. Podemos encontrar um exemplo abaixo.

![image](https://github.com/user-attachments/assets/9282e795-51a1-48b5-aff3-a06ef8791ecb)

Como podemos observar acima, a consulta calcula o comprimento da string dentro da coluna name e o armazena em uma coluna chamada  name_length .

### Funções Agregadas

Essas funções agregam o valor de várias linhas dentro de um critério especificado na consulta; elas podem combinar vários valores em um resultado.

### Função COUNT()

Esta função retorna o número de registros dentro de uma expressão, como mostra o exemplo abaixo.

![image](https://github.com/user-attachments/assets/6e7b915a-b81d-41c6-bdf8-97a2b0978d06)

Esta consulta acima conta o número total de linhas na tabela books . O resultado é  5 , pois há cinco books na tabela books e ele é armazenado na coluna total_books .

### Função SUM()

Esta função soma todos os valores (não NULL) de uma coluna determinada.

Nota: Não há necessidade de executar esta consulta. Isto é apenas para fins de exemplo.

![image](https://github.com/user-attachments/assets/064dde9e-751c-473f-81c5-b55e663374c1)

A consulta acima calcula a soma total da coluna price . O resultado fornece o preço agregado de todos os livros na coluna total_price .

### Função MAX()

Esta função calcula o valor máximo dentro de uma coluna fornecida em uma expressão.

![image](https://github.com/user-attachments/assets/fe4383f0-9601-4246-824c-eb4f7f21b728)

A consulta acima recupera a data da publicação mais recente (valor máximo) da tabela books . O resultado 2021-12-21 é armazenado na coluna latest_book .

### Função MIN()

Esta função calcula o valor mínimo dentro de uma coluna fornecida em uma expressão.

![image](https://github.com/user-attachments/assets/040c2f45-75e7-4136-9050-629090550bae)

A consulta acima recupera a data de publicação mais antiga (valor mínimo) da tabela books . O resultado 2014-10-14 é armazenado na coluna earlier_book .


![image](https://github.com/user-attachments/assets/8f173158-d576-4458-975d-6e8b9f69a3c6)
* SELECT name: Seleciona a coluna name, que contém o nome da ferramenta.
* FROM hacking_tools: Especifica a tabela hacking_tools.
* ORDER BY LENGTH(name) DESC: Ordena os resultados com base no comprimento (LENGTH()) dos nomes, em ordem decrescente (DESC), ou seja, do maior para o menor.
* LIMIT 1: Retorna apenas o primeiro resultado da lista ordenada, que será o nome mais longo.
![image](https://github.com/user-attachments/assets/d66dd7d9-d43a-43b6-8e7f-199a1523c9dc)

---------------------------------------

![image](https://github.com/user-attachments/assets/d5c7cb3b-6e26-41ef-85b0-123792afdc5d)
* SELECT SUM(amount): Usa a função SUM() para calcular a soma de todos os valores na coluna amount.
* FROM hacking_tools: Especifica a tabela hacking_tools.
![image](https://github.com/user-attachments/assets/dd6545fa-855f-476b-812c-a963fb56972f)

-------------------------------------------
![image](https://github.com/user-attachments/assets/603eec10-3041-4b97-a52c-bd6a6fba71b5)
* SELECT GROUP_CONCAT(name SEPARATOR ' & '): Usa a função GROUP_CONCAT() para concatenar os nomes das ferramentas (name), separados por &.
* AS tools_names: Nomeia o resultado como tools_names.
* FROM hacking_tools: Especifica a tabela hacking_tools.
* WHERE amount NOT LIKE '%0': Filtra as ferramentas cujo valor (amount) não termina em "0". O símbolo % representa qualquer sequência de caracteres, e o 0 indica que estamos buscando valores que não terminam em "0".
![image](https://github.com/user-attachments/assets/b0aa6742-6d04-42dd-8da6-4ed37f573be4)


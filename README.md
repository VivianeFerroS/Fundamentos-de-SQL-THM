# Fundamentos-de-SQL-THM
![image](https://github.com/user-attachments/assets/3ff6a2cf-7451-422e-85b6-1bd6b02b97c1)


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





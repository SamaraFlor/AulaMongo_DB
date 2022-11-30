Program Files -> MongoDB -> Server -> 4.0(versão) -> bin  - este e o caminho c
mkdir /data/db criando pastas
mongodb.exe
mongod


//////////////////////////////////////////////////////////////
comandos Mongo

show database  -  para ver os bancos 
use  nome_do_banco   - para criar
db.series.drop() remoção de coleções por linha de comando,
db.series.drop(), sendo series a coleção que desejamos excluir do banco de dados.

remover banco de dados----------------------------
Para excluir um banco de dados pela linha de comando, precisamos estar conectados ao banco de dados que será excluído e executar a instrução:

db.dropDatabase()

Já para excluir uma coleção pela linha de comando, executamos a instrução:
db.Collection.drop()

Criação banco de dados --------------------------

Para criar um banco de dados no MongoDB Shell, nós utilizamos a instrução:

use  <db>

Ao executar este comando, caso o banco de dados já exista, será apenas realizada a conexão, caso contrário, o banco de dados será criado. Mas, precisamos armazenar dados neste banco de dados pela primeira vez, para ele ser definitivamente criado. Então, precisamos criar nossa primeira coleção.

O MongoDB disponibiliza alguns métodos para criarmos uma coleção. Para criar explicitamente uma coleção no MongoDB Shell, nós utilizamos o método:
db.createCollection(“Nome_da_coleção”)
https://www.mongodb.com/docs/manual/reference/method/db.createCollection/


documentos BSON

O BSON é uma representação binária de documentos JSON. Um ponto interessante, é que o BSON trabalha com mais tipos de dados que o próprio JSON.

https://www.mongodb.com/docs/manual/core/document/

Os documentos BSON, também possuem restrições de tamanho:O tamanho máximo de um documento BSON é 16 megabytes.

INSERIR DADOS---------------------------

comando: o insertOne, utilizado quando desejamos inserir um documento por ver e o insertMany, para quando queremos inserir vários documentos ao mesmo tempo.

db.series.insertOne({
"Série": "Fleabag",
        "Ano de lançamento": 2016,
        "Temporadas disponíveis": 2,
        "Linguagem": "Inglês",
        "IMDb Avaliação": 8.7})


        Mais de um documento

        db.series.insertMany([
{        "Série": "Made in Heaven",
        "Temporadas disponíveis": 1,
        "Linguagem": "Hindi",
        "Genero": ["Drama"],
        "IMDb Avaliação": 8.3,
        "Classificação": "18+"
    },{
        "Série": "Homecoming",
        "Temporadas disponíveis": 2,
        "Linguagem": "Inglês",
        "Genero": ["Drama"],
        "IMDb Avaliação": 7.5,
        "Classificação": "16+"
    }])


Fazendo Consultas---------------mongoCompass

FILTER: utilizado para especificar qual será a condição que os documentos devem atender para serem retornados na busca.

PROJECT: utilizado para especificar quais campos serão ou não retornados na consulta.

Ao Informar o nome do campo e informar 0, todos os campos, exceto os campos especificados no campo project, são retornados. Se o campo receber o valor de 1, ele será retornado na consulta. O campo _id é retornado por padrão, a menos que este seja especificado no campo project e definido como 0.
SORT: especifica a ordem de classificação dos documentos retornados.

Para especificar a ordem crescente de um campo, defina o campo como 1.
Para especificar a ordem decrescente de um campo, defina o campo como -1.
MAX TIME MS: define o limite de tempo cumulativo em milissegundos para processar as operações da barra de consulta. Se o limite de tempo for atingido antes da conclusão da operação, o Compass interrompe a operação.

COLLATION: utilizado para especificar regras específicas do idioma para comparação de textos, como regras para letras maiúsculas ou minúsculas, acentos, entre outros.

*SKIP: especifica quantos documentos devem ser ignorados antes de retornar o conjunto de resultados.

LIMIT: especifica o número máximo de documentos a serem retornados.

https://www.mongodb.com/docs/manual/reference/operator/query/

CONSULTAS---------------------

Find,
estrutura
db.users.find(                          ← collection
   { age: { $gt: 18  } },               ← query criteria
     { name: 1, address: 1 }            ← projection
).limit(5)                              ← cursor modifier

Precisamos indicar a coleção, "collection", que estamos realizando nas nossas buscas, então, informamos "db.users", que, no caso, seria a nossa coleção de "Séries", seguido de ".find" e parênteses. Dentro dos parênteses, abrimos o primeiro conjunto de chaves e especificamos o filtro, o critério para a coleção, "query criteria", isto é, o que os documentos precisam atender para serem retornados na busca.

Precisamos indicar a coleção, "collection", que estamos realizando nas nossas buscas, então, informamos "db.users", que, no caso, seria a nossa coleção de "Séries", seguido de ".find" e parênteses. Dentro dos parênteses, abrimos o primeiro conjunto de chaves e especificamos o filtro, o critério para a coleção, "query criteria", isto é, o que os documentos precisam atender para serem retornados na busca.

Em seguida, adicionamos outro conjunto de chaves com a projeção, "projection", onde especificamos quais campos serão, ou não, retornados na busca. Para finalizar, temos o "cursor modifier", onde limitamos quantos documentos serão retornados na busca.

db.series.find() para retornar tudo
db.series.find({"Ano de lançamento": 2018})buscando pelo ano
db.series.find({}, }  Quando desejamos especificar só a projeção, precisamos referenciá-la com um primeiro conjunto de chaves vazio, que é onde indicaríamos o filtro,

db.series.find({},{"Série":1, "Ano de lançamento": 1, "_id":0})
db.series.find({"Ano de lançamento": {$in: [2019,2020]}}) intervalos

db.series.find().limit(5)ate 5 documentos

db.series.find().sort({"Série":1})
passamos o .sort() que é um modificador.

Dentro dos parênteses, abriremos chaves e informaremos por qual campo desejamos ordenar o resultado. Neste caso, é pelo campo Série. Por fim, passaremos o valor 1, determinando que os documentos devem ser ordenados de forma crescente. Se quiséssemos uma ordenação decrescente, passaríamos o valor -1.


Para series com =+ que 4 temporadas
db.series.find({"Temporadas disponíveis": {$gte: 4}})

db.series.find({"Ano de lançamento": {$ne: 2020}})
Também podemos fazer o inverso com o operador "$ne", que vai retornar apenas documentos com valores diferentes do que o que passaremos para ele.


db.series.find({"Genero": {$all:["Ação", "Comédia"]}})
É possível também fazer buscas utilizando, como critério, os dados que estão armazenados no array.
Com esse comando, estamos especificando que todos os documentos de campo Genero que sejam array e tenham os valores "Ação" e "Comédia" ou "Comédia" e "Ação" devem ser retornados na busca.

Estrutura metodo Find

db.collection.find(query, projection)
QUERY: especificamos a condição que os nossos documentos devem atender para serem retornados na nossa consulta.

PROJECTION: especificamos quais campos devem ou não ser retornados na nossa consulta.


Atualizando Dados------------------------------------------

db.users.updateMany(                 ← collection
  { age:  { $lt: 18 } },             ← update filter
    { $set:  { status: "reject" } }  ← update action
)

Precisamos especificar a coleção, collection, então vamos informar db., seguido da coleção e do método que, neste caso, é o updateMany() ou updateOne().

O comando é dividido em duas partes: primeiro, especificamos qual será o filtro, isto é, a condição que o documento precisa atender para que o parâmetro seja atualizado; depois, passamos o operador de atualização que, neste caso, é o operador $set e a ação, o que será atualizado no documento.

ex 
db.series_02.find({"Série": "Grimm"})
db.series.updateOne({"Série": "Grimm"},{$set: {"Temporadas disponíveis": 6}})`atualizando

Além dos métodos UpdateOne e o UpdateMany, o MongoDB também disponibiliza o método ReplaceOne, que é utilizado para substituir um único documento na coleção com base no filtro.

db.collection.replaceOne(
   <filter>,
   <replacement> )

   Filter: especificamos a condição para a atualização.
   Replacement: especificamos o documento que será substituído.


   REMOVENDO DOCUMENTOS-----------------------------------------
   db.users.deleteMany(     ← collection
   { status: "reject" }  ← delete filter
)


db.series.deleteOne({"Série": "The Boys"})

db.series.deleteMany(). Dentro dos parênteses, abriremos chaves e passaremos o filtro "Temporadas disponíveis": 1
db.series.deleteMany({"Temporadas disponíveis": 1})-todas as séries com apenas uma temporada foram removidas.

Se passarmos o deleteMany() vazio, removeremos todos os documentos da coleção.

O método updateOne, é utilizado para atualizar um único documento em uma coleção e possui a seguinte estrutura básica:
db.collection.updateOne(<filter>, <update>)

Já o método updateMany é utilizado para atualizar vários documentos em uma coleção, e possui a seguinte estrutura básica:
db.collection.updateMany(<filter>, <update>)

Para remover os dados, o MongoDB disponibiliza dois métodos.
O método deleteOne, é utilizado para remover um único documento em uma coleção, e possui a seguinte estrutura básica:
db.collection.deleteOne(<filter>)

O método deleteMany é utilizado para remover vários documentos em uma coleção, e possui a seguinte estrutura básica:
db.collection.deleteMany(<filter>)

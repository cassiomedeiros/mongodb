# mongodb
## Atividade Prática – MongoDB

Todas as perguntas e comandos estão documentadas no arquivo [Jupyter Notebook](https://github.com/cassiomedeiros/mongodb/blob/master/src/mongodb.ipynb).

Escolhi essa ferramenta por conta dos recursos gráficos que ela permite para documentar o código e visualisar as respostas dos comandos.

Orientações para instalar o Jupyter Notebook clique [aqui](https://jupyter.readthedocs.io/en/latest/install.html).

### Dependências

- [Python >= 3.6](https://www.python.org/downloads/)
- [Pandas](https://pandas.pydata.org/pandas-docs/stable/getting_started/install.html)
- [Numpy](https://pypi.org/project/numpy/)
- [Pymongo](https://pypi.org/project/pymongo/)

### Exercício 1- Aquecendo com os pets


```python
client = MongoClient()
```


```python
petshop = client['petshop']
pets = petshop['pets']
```


```python
pets.insert_one({"name": "Mike", "species": "Hamster"})
pets.insert_one({"name": "Dolly", "species": "Peixe"})
pets.insert_one({"name": "Kilha", "species": "Gato"})
pets.insert_one({"name": "Mike", "species": "Cachorro"})
pets.insert_one({"name": "Sally", "species": "Cachorro"})
pets.insert_one({"name": "Chuck", "species": "Gato"})
```




    <pymongo.results.InsertOneResult at 0x1a2ce680f48>



#### 1. Adicione outro Peixe e um Hamster com nome Frodo


```python
pets.insert_one({"name": "Frodo", "species": "Peixe"})
```



```python
pets.insert_one({"name": "Frodo", "species": "Hamster"})
```





#### 2. Faça uma contagem dos pets na coleção


```python
pets.count()
```




    16



#### 3. Retorne apenas um elemento o método prático possível


```python
pets.find_one()
```




    {'_id': ObjectId('5e681a90a255e706bcff626f'),
     'name': 'Mike',
     'species': 'Hamster'}



#### 4. Identifique o ID para o Gato Kilha.


```python
pets.find_one({"name": "Kilha"}, {'_id'})
```




    {'_id': ObjectId('5e681a90a255e706bcff6271')}



#### 5. Faça uma busca pelo ID e traga o Hamster Mike


```python
id_hamster = pets.find_one({"name": "Mike", "species": "Hamster"}, {'_id'})
```


    {'_id': ObjectId('5e681a90a255e706bcff626f')}


```python
pets.find_one(id_hamster)
```


    {'_id': ObjectId('5e681a90a255e706bcff626f'),
     'name': 'Mike',
     'species': 'Hamster'}



#### 6. Use o find para trazer todos os Hamsters


```python
for pet in pets.find({"species": "Hamster"}):
    print(pet)
```

    {'_id': ObjectId('5e681a90a255e706bcff626f'), 'name': 'Mike', 'species': 'Hamster'}
    {'_id': ObjectId('5e681bdca255e706bcff6276'), 'name': 'Frodo', 'species': 'Hamster'}
    {'_id': ObjectId('5e699a4aa255e73d1c699ae0'), 'name': 'Mike', 'species': 'Hamster'}
    {'_id': ObjectId('5e699a4aa255e73d1c699ae7'), 'name': 'Frodo', 'species': 'Hamster'}
    

#### 7. Use o find para listar todos os pets com nome Mike


```python
for pet in pets.find({"name": "Mike"}):
    print(pet)
```

    {'_id': ObjectId('5e681a90a255e706bcff626f'), 'name': 'Mike', 'species': 'Hamster'}
    {'_id': ObjectId('5e681a90a255e706bcff6272'), 'name': 'Mike', 'species': 'Cachorro'}
    {'_id': ObjectId('5e699a4aa255e73d1c699ae0'), 'name': 'Mike', 'species': 'Hamster'}
    {'_id': ObjectId('5e699a4aa255e73d1c699ae3'), 'name': 'Mike', 'species': 'Cachorro'}
    

#### 8. Liste apenas o documento que é um Cachorro chamado Mike


```python
for pet in pets.find({"name": "Mike", "species": "Cachorro"}):
    print(pet)
```

    {'_id': ObjectId('5e681a90a255e706bcff6272'), 'name': 'Mike', 'species': 'Cachorro'}
    {'_id': ObjectId('5e699a4aa255e73d1c699ae3'), 'name': 'Mike', 'species': 'Cachorro'}
    

## Exercício 2 – Mama mia!   

Importe o arquivo dos italian-people.js do seguinte endereço: Downloads NoSQL
FURB. Em seguida, importe o mesmo com o seguinte comando:

#### Load collection


```python
italians = client['people']['italians']
```


```python
italians.count()
```




    10000




```python
italians.find_one()
```




    {'_id': ObjectId('5e68285a7e9a37b3a0792103'),
     'firstname': 'Sonia',
     'surname': 'Ferri',
     'username': 'user100',
     'age': 30.0,
     'email': 'Sonia.Ferri@gmail.com',
     'bloodType': 'A+',
     'id_num': '351752072450',
     'registerDate': datetime.datetime(2011, 1, 1, 18, 6, 38, 988000),
     'ticketNumber': 6829.0,
     'jobs': ['Musicoterapia'],
     'favFruits': ['Goiaba', 'Goiaba', 'Uva'],
     'movies': [{'title': 'A Vida é Bela (1997)', 'rating': 3.32},
      {'title': 'O Senhor dos Anéis: A Sociedade do Anel (2001)', 'rating': 3.46},
      {'title': 'Cidade de Deus (2002)', 'rating': 0.19},
      {'title': 'Gladiador (2000)', 'rating': 2.04},
      {'title': 'Gladiador (2000)', 'rating': 2.95}],
     'cat': {'name': 'Cristina', 'age': 6.0},
     'dog': {'name': 'Andrea', 'age': 7.0}}



#### 1. Liste/Conte todas as pessoas que tem exatamente 99 anos. Você pode usar um count para indicar a quantidade


```python
cursor =  italians.find({"age": 99})
cursor.count()
```




    0



#### 2. Identifique quantas pessoas são elegíveis atendimento prioritário (pessoas com mais de 65 anos)


```python
cursor =  italians.find({"age": {"$gt": 65}})
cursor.count()
```




    2045



#### 3. Identifique todos os jovens (pessoas entre 12 a 18 anos).


```python
cursor =  italians.find({"age": {"$gt": 12, "$lt": 18}})
cursor.count()
```




    623



#### 4. Identifique quantas pessoas tem gatos, quantas tem cachorro e quantas não tem nenhum dos dois

##### quantas pessoas tem gatos,


```python
cursor = italians.find({'cat': {"$exists": True}})
cursor.count()
```




    10000



##### quantas tem cachorro


```python
cursor = italians.find({'dog': {"$exists": True}})
cursor.count()
```




    4038



##### quantas não tem nenhum dos dois


```python
cursor = italians.find({"$and": [{'cat': {"$exists": False}}, {'dog': {"$exists": False}}]})
cursor.count()
```




    0



#### 5. Liste/Conte todas as pessoas acima de 60 anos que tenham gato


```python
cursor =  italians.find({"$and": [{"age": {"$gt": 60}}, {'cat': {"$exists": True}}]})
cursor.count()
```




    2638




```python
cursor.next()
```




    {'_id': ObjectId('5e68285a7e9a37b3a0792104'),
     'firstname': 'Vincenzo',
     'surname': 'Amato',
     'username': 'user101',
     'age': 61.0,
     'email': 'Vincenzo.Amato@yahoo.com',
     'bloodType': 'A-',
     'id_num': '177505155144',
     'registerDate': datetime.datetime(2018, 10, 1, 15, 1, 3, 788000),
     'ticketNumber': 4687.0,
     'jobs': ['Biocombustíveis'],
     'favFruits': ['Mamão', 'Banana'],
     'movies': [{'title': 'A Vida é Bela (1997)', 'rating': 1.21},
      {'title': 'O Poderoso Chefão (1972)', 'rating': 3.43},
      {'title': '1917 (2019)', 'rating': 2.31}],
     'father': {'firstname': 'Roberto', 'surname': 'Amato', 'age': 79.0},
     'cat': {'name': 'Claudia', 'age': 16.0},
     'dog': {'name': 'Pietro', 'age': 9.0}}



#### 6. Liste/Conte todos os jovens com cachorro


```python
cursor =  italians.find({"$and": [{"age": {"$lt": 18}}, {'dog': {"$exists": True}}]})
cursor.count()
```




    783




```python
cursor.next()
```




    {'_id': ObjectId('5e68285a7e9a37b3a079211c'),
     'firstname': 'Alessandra',
     'surname': 'Gentile',
     'username': 'user125',
     'age': 7.0,
     'email': 'Alessandra.Gentile@gmail.com',
     'bloodType': 'A+',
     'id_num': '822371164324',
     'registerDate': datetime.datetime(2017, 4, 9, 9, 22, 5, 379000),
     'ticketNumber': 3383.0,
     'jobs': ['Engenharia Naval', 'Banco de Dados'],
     'favFruits': ['Goiaba', 'Banana'],
     'movies': [{'title': 'O Resgate do Soldado Ryan (1998)', 'rating': 1.42},
      {'title': 'Gladiador (2000)', 'rating': 0.45},
      {'title': 'Guerra nas Estrelas (1977)', 'rating': 4.32}],
     'father': {'firstname': 'Rosa', 'surname': 'Gentile', 'age': 35.0},
     'cat': {'name': 'Stefano', 'age': 1.0},
     'dog': {'name': 'Alex', 'age': 13.0}}



#### 7. Utilizando o where, liste todas as pessoas que tem gato e cachorro


```python
cursor =  italians.find({"$and": [{"$where": "this.cat != undefined"}, {"$where": "this.dog != undefined"}]})
cursor.count()
```




    4038




```python
cursor.next()
```




    {'_id': ObjectId('5e68285a7e9a37b3a0792103'),
     'firstname': 'Sonia',
     'surname': 'Ferri',
     'username': 'user100',
     'age': 30.0,
     'email': 'Sonia.Ferri@gmail.com',
     'bloodType': 'A+',
     'id_num': '351752072450',
     'registerDate': datetime.datetime(2011, 1, 1, 18, 6, 38, 988000),
     'ticketNumber': 6829.0,
     'jobs': ['Musicoterapia'],
     'favFruits': ['Goiaba', 'Goiaba', 'Uva'],
     'movies': [{'title': 'A Vida é Bela (1997)', 'rating': 3.32},
      {'title': 'O Senhor dos Anéis: A Sociedade do Anel (2001)', 'rating': 3.46},
      {'title': 'Cidade de Deus (2002)', 'rating': 0.19},
      {'title': 'Gladiador (2000)', 'rating': 2.04},
      {'title': 'Gladiador (2000)', 'rating': 2.95}],
     'cat': {'name': 'Cristina', 'age': 6.0},
     'dog': {'name': 'Andrea', 'age': 7.0}}



#### 8. Liste todas as pessoas mais novas que seus respectivos gatos.


```python
cursor =  italians.find({"$and": [{"$where": "this.cat != undefined"}, {"$where": "this.age < this.cat.age"}]})
cursor.count()
```




    546




```python
cursor.next()
```




    {'_id': ObjectId('5e68285a7e9a37b3a079211e'),
     'firstname': 'Barbara',
     'surname': 'Barbieri',
     'username': 'user127',
     'age': 15.0,
     'email': 'Barbara.Barbieri@outlook.com',
     'bloodType': 'B+',
     'id_num': '788403265227',
     'registerDate': datetime.datetime(2013, 6, 17, 11, 32, 57, 859000),
     'ticketNumber': 1020.0,
     'jobs': ['Saneamento Ambiental', 'Manutenção de aeronaves'],
     'favFruits': ['Banana'],
     'movies': [{'title': 'A Origem (2010)', 'rating': 1.11},
      {'title': 'Batman: O Cavaleiro das Trevas (2008)', 'rating': 0.83},
      {'title': 'Star Wars, Episódio V: O Império Contra-Ataca (1980)',
       'rating': 1.03},
      {'title': 'O Senhor dos Anéis: O Retorno do Rei (2003)', 'rating': 3.74},
      {'title': 'Parasita (2019)', 'rating': 0.39}],
     'father': {'firstname': 'Alessio', 'surname': 'Barbieri', 'age': 47.0},
     'cat': {'name': 'Marco', 'age': 17.0}}



#### 9. Liste as pessoas que tem o mesmo nome que seu bichano (gato ou cachorro)


```python
cursor =  italians.find({"$or": [{"$and": [{"$where": "this.cat != undefined"}, {"$where": "this.firstname == this.cat.name"}]}, 
                                 {"$and": [{"$where": "this.dog != undefined"}, {"$where": "this.firstname == this.dog.name"}]}]})
cursor.count()
```




    109




```python
cursor.next()
```




    {'_id': ObjectId('5e68285a7e9a37b3a079211f'),
     'firstname': 'Mirko',
     'surname': 'Coppola',
     'username': 'user128',
     'age': 30.0,
     'email': 'Mirko.Coppola@outlook.com',
     'bloodType': 'A+',
     'id_num': '802425736348',
     'registerDate': datetime.datetime(2011, 12, 10, 4, 18, 1, 593000),
     'ticketNumber': 2729.0,
     'jobs': ['Engenharia Hídrica', 'Engenharia Florestal'],
     'favFruits': ['Kiwi'],
     'movies': [{'title': 'O Silêncio dos Inocentes (1991)', 'rating': 2.36},
      {'title': 'Vingadores: Ultimato (2019)', 'rating': 4.79},
      {'title': 'À Espera de um Milagre (1999)', 'rating': 4.85},
      {'title': 'O Silêncio dos Inocentes (1991)', 'rating': 0.26}],
     'father': {'firstname': 'Mattia', 'surname': 'Coppola', 'age': 54.0},
     'dog': {'name': 'Mirko', 'age': 10.0},
     'cat': {'age': 1}}



#### 10. Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo


```python
cursor =  italians.find({"bloodType": {'$regex': '-'}}, {"firstname": 1, 'surname': 1, "bloodType": 1})
cursor.count()
```




    4939



#### 11. Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. Não mostre o identificado do mongo (ObjectId)


```python
cursor =  italians.aggregate([{"$match": {"$or": [{'cat': {"$exists": True}}, {'dog': {"$exists": True}}]}},
                             {
                               "$group":
                                 {
                                   "_id": {},
                                   "animais": { "$push": {"$mergeObjects": [{ "name": "$cat.name", "age": "$cat.age"},
                                                                            { "name": "$dog.name", "age": "$dog.age"}]}}
                                 }
                             }])
```


```python
animais = cursor.next()['animais']
```


```python
len(animais)
```




    10000



#### 12. Quais são as 5 pessoas mais velhas com sobrenome Rossi?


```python
cursor =  italians.find({"surname": "Rossi"}, {"firtname": 1, "age": 1}).sort("age", -1).limit(5)
```


```python
for p in cursor:
    print(p)
```

    {'_id': ObjectId('5e6828677e9a37b3a079312c'), 'age': 81.0}
    {'_id': ObjectId('5e6828657e9a37b3a0792eaa'), 'age': 80.0}
    {'_id': ObjectId('5e68286f7e9a37b3a07938f1'), 'age': 80.0}
    {'_id': ObjectId('5e6828667e9a37b3a0792f76'), 'age': 79.0}
    {'_id': ObjectId('5e6828777e9a37b3a07943e1'), 'age': 77.0}
    

#### 13. Crie um italiano que tenha um leão como animal de estimação. Associe um nome e idade ao bichano


```python
italians.insert_one({
    'firstname': 'Cassio',
    'surname': 'Medeiros',
    'dog': {'name': 'Zeus', 'age': 1}}
)
```




    <pymongo.results.InsertOneResult at 0x1a2ce680b88>



#### 14. Infelizmente o Leão comeu o italiano. Remova essa pessoa usando o Id


```python
id_novo_italiano = italians.find_one({"firstname": "Cassio"}, {'_id'})
```


```python
id_novo_italiano
```




    {'_id': ObjectId('5e699b2aa255e73d1c699ae8')}




```python
italians.delete_one(id_novo_italiano)
```




    <pymongo.results.DeleteResult at 0x1a2bd87f388>



#### 16. Passou um ano. Atualize a idade de todos os italianos e dos bichanos em 1.


```python
italians.find_one()
```




    {'_id': ObjectId('5e68285a7e9a37b3a0792103'),
     'firstname': 'Sonia',
     'surname': 'Ferri',
     'username': 'user100',
     'age': 30.0,
     'email': 'Sonia.Ferri@gmail.com',
     'bloodType': 'A+',
     'id_num': '351752072450',
     'registerDate': datetime.datetime(2011, 1, 1, 18, 6, 38, 988000),
     'ticketNumber': 6829.0,
     'jobs': ['Musicoterapia'],
     'favFruits': ['Goiaba', 'Goiaba', 'Uva'],
     'movies': [{'title': 'A Vida é Bela (1997)', 'rating': 3.32},
      {'title': 'O Senhor dos Anéis: A Sociedade do Anel (2001)', 'rating': 3.46},
      {'title': 'Cidade de Deus (2002)', 'rating': 0.19},
      {'title': 'Gladiador (2000)', 'rating': 2.04},
      {'title': 'Gladiador (2000)', 'rating': 2.95}],
     'cat': {'name': 'Cristina', 'age': 6.0},
     'dog': {'name': 'Andrea', 'age': 7.0}}




```python
italians.update_many({}, {"$inc": {"age": 1}})
italians.update_many({}, {"$inc": {"cat.age": 1}})
```




    <pymongo.results.UpdateResult at 0x1a2ce7a3cc8>




```python
italians.find_one()
```




    {'_id': ObjectId('5e68285a7e9a37b3a0792103'),
     'firstname': 'Sonia',
     'surname': 'Ferri',
     'username': 'user100',
     'age': 31.0,
     'email': 'Sonia.Ferri@gmail.com',
     'bloodType': 'A+',
     'id_num': '351752072450',
     'registerDate': datetime.datetime(2011, 1, 1, 18, 6, 38, 988000),
     'ticketNumber': 6829.0,
     'jobs': ['Musicoterapia'],
     'favFruits': ['Goiaba', 'Goiaba', 'Uva'],
     'movies': [{'title': 'A Vida é Bela (1997)', 'rating': 3.32},
      {'title': 'O Senhor dos Anéis: A Sociedade do Anel (2001)', 'rating': 3.46},
      {'title': 'Cidade de Deus (2002)', 'rating': 0.19},
      {'title': 'Gladiador (2000)', 'rating': 2.04},
      {'title': 'Gladiador (2000)', 'rating': 2.95}],
     'cat': {'name': 'Cristina', 'age': 7.0},
     'dog': {'name': 'Andrea', 'age': 7.0}}



#### 17. Utilizando o framework agregate, liste apenas as pessoas com nomes iguais a sua respectiva mãe e que tenha gato ou cachorro.


```python
cursor =  italians.aggregate([{"$match": {"$and": [{"mother": {"$exists": True}}, {"$or": [{'cat': {"$exists": True}}, {'dog': {"$exists": True}}]}]}},
                             {
                               "$project":
                                 {
                                   "firstname": 1,
                                   "mother": 1,
                                   "isEqual": { "$cmp": ["$firstname", "$mother.firstname"]}
                                 }
                             }, {"$match": {"isEqual": 0}}])
```


```python
for c in cursor:
    print(c)
```

    {'_id': ObjectId('5e68285b7e9a37b3a0792403'), 'firstname': 'Raffaele', 'mother': {'firstname': 'Raffaele', 'surname': 'Gallo', 'age': 83.0}, 'isEqual': 0}
    {'_id': ObjectId('5e68285b7e9a37b3a0792405'), 'firstname': 'Massimo', 'mother': {'firstname': 'Massimo', 'surname': 'Ricci', 'age': 49.0}, 'isEqual': 0}
    {'_id': ObjectId('5e68285d7e9a37b3a07925ef'), 'firstname': 'Alessio', 'mother': {'firstname': 'Alessio', 'surname': 'Palumbo', 'age': 73.0}, 'isEqual': 0}
    {'_id': ObjectId('5e6828647e9a37b3a0792e3c'), 'firstname': 'Carlo', 'mother': {'firstname': 'Carlo', 'surname': 'Grassi', 'age': 26.0}, 'isEqual': 0}
    {'_id': ObjectId('5e68286b7e9a37b3a07934cb'), 'firstname': 'Rita', 'mother': {'firstname': 'Rita', 'surname': 'Costatini', 'age': 33.0}, 'isEqual': 0}
    {'_id': ObjectId('5e68286e7e9a37b3a07936e8'), 'firstname': 'Giuseppe', 'mother': {'firstname': 'Giuseppe', 'surname': 'Bernardi', 'age': 76.0}, 'isEqual': 0}
    {'_id': ObjectId('5e6828737e9a37b3a0793dda'), 'firstname': 'Matteo', 'mother': {'firstname': 'Matteo', 'surname': 'Donati', 'age': 59.0}, 'isEqual': 0}
    {'_id': ObjectId('5e6828747e9a37b3a0793fe5'), 'firstname': 'Angela', 'mother': {'firstname': 'Angela', 'surname': 'Giordano', 'age': 53.0}, 'isEqual': 0}
    {'_id': ObjectId('5e6828787e9a37b3a07945f2'), 'firstname': 'Rita', 'mother': {'firstname': 'Rita', 'surname': 'Cattaneo', 'age': 87.0}, 'isEqual': 0}
    

#### 18. Utilizando aggregate framework, faça uma lista de nomes única de nomes. Faça isso usando apenas o primeiro nome


```python
cursor =  italians.aggregate([
                             {
                               "$group":
                                 {
                                   "_id": {},
                                   "pessoas":  { "$addToSet": { "firstname": "$firstname"} }
                             }}])

len(cursor.next()['pessoas'])
```




    100




```python

```

#### 19. Agora faça a mesma lista do item acima, considerando nome completo.


```python
cursor =  italians.aggregate([
                             {
                               "$group":
                                 {
                                   "_id": {},
                                   "pessoas":  { "$addToSet": { "firstname": "$firstname", 'surname': '$surname'} }
                             }}])

len(cursor.next()['pessoas'])
```




    6316



#### 20. Procure pessoas que gosta de Banana ou Maçã, tenham cachorro ou gato, mais de 20 e menos de 60 anos.


```python
cursor =  italians.find({"$and": [{"favFruits": {"$in": ["Banana", "Maçã"]}},
                        {"$or": [{'cat': {"$exists": True}}, {'dog': {"$exists": True}}]}, 
                        {"age": {"$gt": 20, "$lt": 60}}]})
```


```python
cursor.count()
```




    1757



## Exercício 3 – Fraude na Enron!

Um dos casos mais emblemáticos de fraude no mundo é o caso da Enron. A
comunicade do MongoDB utiliza muito esse dataset pois o mesmo se tornou
público, então vamos importar esse material também:

#### Load collection


```python
stocks = client['stocks']['stocks']
```


```python
stocks.count()
```

#### 1. Liste as pessoas que enviaram e-mails (de forma distinta, ou seja, sem repetir). Quantas pessoas são?


```python
cursor =  stocks.aggregate([
                             {
                               "$group":
                                 {
                                   "_id": {},
                                   "remetentes":  { "$addToSet": { "email": "$sender"} }
                             }}])

remetentes = cursor.next()['remetentes']
```

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ssmith@uwtgc.org</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tally@ssprd2.net</td>
    </tr>
    <tr>
      <th>2</th>
      <td>cmprn107577@gm20.com</td>
    </tr>
    <tr>
      <th>3</th>
      <td>gael.doar@enron.com</td>
    </tr>
    <tr>
      <th>4</th>
      <td>cheryltd@tbardranch.com</td>
    </tr>
  </tbody>
</table>
</div>


```python
len(remetentes)
```




    2200



#### 2. Contabilize quantos e-mails tem a palavra “fraud”


```python
cursor =  stocks.find({"text": {"$regex": "fraud"}})
```


```python
cursor.count()
```




    23




```python
for c in cursor:
    print(c['text'])
```

    Good morning, Liz -
    
    I left a message at your home this morning that your Dad would like to speak 
    with you when you have a chance to call.  
    
    Rosie
    
    p.s. - P. L. and I did early voting the first Saturday available!!  It was 
    such a good feeling as Tuesdays are tough with trying to get to the office 
    and leave in time to vote!!
    
    
    
    
    
    Elizabeth Lay <lizard_ar@yahoo.com> on 11/06/2000 09:13:26 AM
    To: ealvittor@yahoo.com
    cc:  
    Subject: Get out the Vote
    
    
    Dear Friends and Family,
    We are down to the wire and the race couldn't be
    closer. Every vote counts, even those of you in Texas.
    Please, remember to vote and go early, polls open at
    7am across the country and close at 7pm but don't put
    it off until 7pm as there could be lines and the
    supervisors are not required to keep the polls open
    after 7pm for those in line.
    Remind your friends and family to vote and if someone
    needs transportation to the polls, give them a ride or
    call your local Victory 2000 office (Republicans) or
    your local Democratic campaign office, they will have
    people available to transport people to the polls.
    Keep an eye out while you are at the polls for
    electoral fraud and report any suspicious activity
    including inappropriate campaigning to the poll
    watchers at the polls. The Democrats and the
    Republicans will have representatives at most of the
    polls, let them know if there is suspicious activity.
    Finally, if you have time, call your local offices and
    offer your time to make phone calls to encourage
    people to get out and vote, pass out leaflets, or to
    be available to help prevent of voter fraud.
    
    Finally, enjoy the evening and watch the results come
    in. There are election watch parties everywhere!!!
    
    Just one more day (and no more mass political e-mail's
    from me and everyone else)!
    Take care and VOTE!
    Liz
    
    __________________________________________________
    Do You Yahoo!?
    Thousands of Stores.  Millions of Products.  All in one Place.
    http://shopping.yahoo.com/
    
    
    Tue, 12 Dec 2000 18:49:21 -0600
    Date: Tue, 12 Dec 2000 18:49:21 -0600
    Message-Id: <200012130049.SAA07799@server1.pjdoland.com>
    To: klay@enron.com
    From: David Theroux <DJTheroux@independent.org>
    Reply-to: DJTheroux@independent.org
    X-Mailer: Perl Powered Socket Mailer
    Subject: THE LIGHTHOUSE: December 12, 2000
    
    THE LIGHTHOUSE
    "Enlightening Ideas for Public Policy..."
    VOL. 2, ISSUE 48
    December 12, 2000
    
    Welcome to The Lighthouse, the e-mail newsletter of The Independent 
    Institute, the non-partisan, public policy research organization 
    <http://www.independent.org>. We provide you with updates of the Institute's 
    current research publications, events and media programs.
    
    -------------------------------------------------------------
    
    IN THIS WEEK'S ISSUE:
    1. Pentagon "Shocked" to Find Rivers Dammed with Pork
    2. The Environmental Propaganda Agency
    3. William Lloyd Garrison, Antislavery Crusader
    
    -------------------------------------------------------------
    
    PENTAGON "SHOCKED" TO FIND RIVERS DAMMED WITH PORK
    
    Captain Louis Renault -- Claude Raines's cheerfully duplicitous character in 
    the 1942 film classic "Casablanca" -- asserted glibly that he was "shocked, 
    shocked" to learn that gambling was taking place at Rick's Cafe. Moments 
    later he was only too happy to collect his gambling earnings for the night.
    
    All this is by way of preamble to a new Pentagon investigation of fraud in 
    military construction. The investigation concluded that three senior Army 
    Corps of Engineers officials had, just as one whistle-blowing Corps economist 
    had claimed, engaged in a deceitful campaign to justify what the Washington 
    Post called "a billion-dollar construction binge on the Mississippi and 
    Illinois rivers."
    
    "The [Pentagon] investigators concluded that the agency's aggressive efforts 
    to expand its budget and missions, as well as its eagerness to please its 
    corporate customers and congressional patrons, have helped 'create an 
    atmosphere where objectivity in its analyses was placed in jeopardy,'" the 
    Post reports.
    
    "Even the agency's retired chief economist told them that Corps studies were 
    often 'corrupt,' and that several Corps employees cited 'immense pressure' to 
    green-light questionable projects."
    
    Bureaucratic boondoggles of such magnitude are certainly newsworthy. But they 
    are hardly news. Just as the Soviets derided the failures of previous Five 
    Year Plans (only to implement new, equally flawed versions), so it seems that 
    every few years the Pentagon uncovers massive corruption and waste in its own 
    centrally planned fiefdom -- only to present a new Plan that operates under 
    the same bad incentives that encouraged prior malfeasance.
    
    With corruption and waste seemingly "taken care of," the worst pork-barrel 
    spenders in Congress and the military are then let off the hook, only to 
    enjoy -- like Casablanca's Renault and Rick -- an amicable toast to the 
    beginnings of a beautiful new friendship.
    
    For the Washington Post series on the Army Corp of Engineers boondoggle, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-1.html.
    
    For a summary of the Independent Institute book, ARMS, POLITICS AND THE 
    ECONOMY: Historical and Contemporary Perspectives, edited by Robert Higgs, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-2.html.
    
    -------------------------------------------------------------
    
    THE ENVIRONMENTAL PROPAGANDA AGENCY
    
    Will the neck-to-neck presidential race help reduce -- or intensify -- 
    pressure for the next president of the United States to score points with 
    statist environmental activists?
    
    Except on a few controversial issues, a strong case can be made that the 
    forty-third President of the United States will wish to portray himself as a 
    close friend of "the environment." President George W. Bush, for example, 
    would face strong pressure to show that he is "bipartisan" in his approach to 
    environmental protection; whereas President Al Gore would likely attempt to 
    win back those who supported Nader and the Greens.
    
    All the more reason, then, to call attention to the failures of the current 
    approach to environmental protection -- especially those emanating from the 
    U.S. Environmental Protection Agency, or, as economist Craig Marxsen terms 
    it, the Environmental Propaganda Agency.
    
    The EPA sometimes employs the language of cost-benefit analysis to illustrate 
    its seemingly tremendous success, but it is known to employ it in a highly 
    misleading manner. The EPA claimed, for example, that its Clean Air Act 
    programs produced, from 1970 to 1990, $22.2 trillion dollars in health 
    benefits at a cost of only $523 billion. But, reports Marxsen in THE 
    INDEPENDENT REVIEW, "[The EPA's] study actually represents a milestone in 
    bureaucratic propaganda. Like junk science in the courtroom, the study 
    seemingly attempts to obtain the largest possible benefit figure rather than 
    to come as close as possible to the truth."
    
    In conclusion, writes Marxsen, "Without the illusory benefit of all the lives 
    saved, the actual benefits of the Clean Air Act were very modest and probably 
    could have been achieved nearly as well with far less sacrifice. The Clean 
    Air Act and its amendments force the EPA to mandate reduction of air 
    pollution to levels that would have no adverse health effects on even the 
    most sensitive person in the population. The EPA relentlessly presses forward 
    in its absurd quest, like a madman setting fire to his house in an insane 
    determination to eliminate the last of the insects infesting it."
    
    For more information, see "The Environmental Propaganda Agency," by Craig S. 
    Marxsen (THE INDEPENDENT REVIEW, Summer 2000), at
    http://www.independent.org/tii/lighthouse/LHLink2-48-3.html.
    
    For analysis of other EPA programs, see the Independent Institute book, 
    CUTTING GREEN TAPE: Toxic Pollutants, Environmental Regulation and the Law, 
    edited by Richard Stroup and Roger Meiners, at 
    http://www.independent.org/tii/lighthouse/LHLink2-48-4.html.
    
    For Robert Formaini's insightful review of Kip Viscusi's important book, 
    RATIONAL RISK POLICY (THE INDEPENDENT REVIEW, Winter 1999), see
    http://www.independent.org/tii/lighthouse/LHLink2-48-5.html.
    
    -------------------------------------------------------------
    
    WILLIAM LLOYD GARRISON, Antislavery Crusader
    
    "I will be as harsh as truth, and as uncompromising as justice. On this 
    subject, I do not wish to think, or speak, or write with moderation.... I 
    will not equivocate -- I will not excuse -- I will not retreat a single inch 
    -- and I will be heard."
         -- William Lloyd Garrison, THE LIBERATOR, January 1, 1831
    
    December 12 marks the 195th anniversary of the birth of William Lloyd 
    Garrison, a leading figure in the American abolitionist movement. As the late 
    historian Henry Mayer explained in his National Book Award-Finalist 
    biography, ALL ON FIRE: William Lloyd Garrison and the Abolition of Slavery:
    
    "William Lloyd Garrison (1805-1879) is an authentic American hero who, with a 
    Biblical prophet's power and a propagandist's skill, forced the nation to 
    confront the most crucial moral issue in its history. For thirty-five years 
    he edited and published a weekly newspaper in Boston, THE LIBERATOR, which 
    remains today a sterling and unrivaled example of personal journalism in the 
    service of civic idealism.
    
    "Although Garrison -- a self-made man with a scanty formal education -- 
    considered himself 'a New England mechanic' and lived outside the precincts 
    of the American intelligentsia, he nonetheless did the hard intellectual work 
    of challenging orthodoxy, questioning public policy, and offering a luminous 
    vision of a society transformed. He inspired two generations of activists -- 
    female and male, black and white -- and together they built a social movement 
    which, like the civil rights movement of our own day, was a collaboration of 
    ordinary people, stirred by injustice and committed to each other, who 
    achieved a social change that conventional wisdom first condemned as wrong 
    and then ridiculed as impossible."
    
    Indeed, without Garrison's inflammatory but compelling writing, speaking and 
    organizing, there might have been no effective American anti-slavery movement 
    at all.
    
    For more on William Lloyd Garrison, read historian Henry Mayer's talk from 
    the Independent Policy Forum, "The Civil War: Liberty and American Leviathan" 
    (with Jeffrey Rogers Hummel), at
    http://www.independent.org/tii/lighthouse/LHLink2-48-6.html, or hear it in 
    RealAudio at http://www.independent.org/tii/lighthouse/LHLink2-48-7.html.
    
    Also see Jeffrey Rogers Hummel's review of Henry Mayer's brilliant biography, 
    ALL ON FIRE: William Lloyd Garrison and the Abolition of Slavery (THE 
    INDEPENDENT REVIEW, Fall 2000), at
    http://www.independent.org/tii/lighthouse/LHLink2-48-8.html.
    
    -------------------------------------------------------------
    
    If you enjoy receiving THE LIGHTHOUSE ... please help us support it.
    
    Your supporting Independent Associate Membership enables us to reach 
    thousands of other people. So, please make a contribution to The Independent 
    Institute. See http://www.independent.org/tii/lighthouse/LHLink2-48-9.html. 
    to donate, or contact Ms. Priscilla Busch by phone at 510-632-1366 x105, fax 
    to 510-568-6040, email to <PBusch@independent.org>, or snail mail to The 
    Independent Institute, 100 Swan Way, Oakland, CA 94621-1428.
    All contributions are tax-deductible.  Thank you!
    
    -------------------------------------------------------------
    
    For previous issues of THE LIGHTHOUSE, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-10.html.
    
    -------------------------------------------------------------
    
    For information on books and other publications from The Independent 
    Institute, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-11.html.
    
    -------------------------------------------------------------
    
    For information on The Independent Institute's Independent Policy Forums, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-12.html.
    
    -------------------------------------------------------------
    
    To subscribe (or unsubscribe) to The Lighthouse, please go to 
    http://www.independent.org/subscribe.html, choose "subscribe" (or 
    "unsubscribe"), enter your e-mail address and select "Go."
    
    Copyright , 2000 The Independent Institute
    100 Swan Way
    Oakland, CA 94621-1428
    (510) 632-1366 phone
    (510) 568-6040 fax
    info@independent.org
    http://www.independent.org
    
    Dear Friends and Family,
    We are down to the wire and the race couldn't be
    closer. Every vote counts, even those of you in Texas.
    Please, remember to vote and go early, polls open at
    7am across the country and close at 7pm but don't put
    it off until 7pm as there could be lines and the
    supervisors are not required to keep the polls open
    after 7pm for those in line.
    Remind your friends and family to vote and if someone
    needs transportation to the polls, give them a ride or
    call your local Victory 2000 office (Republicans) or
    your local Democratic campaign office, they will have
    people available to transport people to the polls.
    Keep an eye out while you are at the polls for
    electoral fraud and report any suspicious activity
    including inappropriate campaigning to the poll
    watchers at the polls. The Democrats and the
    Republicans will have representatives at most of the
    polls, let them know if there is suspicious activity.
    Finally, if you have time, call your local offices and
    offer your time to make phone calls to encourage
    people to get out and vote, pass out leaflets, or to
    be available to help prevent of voter fraud.
    
    Finally, enjoy the evening and watch the results come
    in. There are election watch parties everywhere!!!
    
    Just one more day (and no more mass political e-mail's
    from me and everyone else)!
    Take care and VOTE!
    Liz
    
    __________________________________________________
    Do You Yahoo!?
    Thousands of Stores.  Millions of Products.  All in one Place.
    http://shopping.yahoo.com/
    Good morning, Liz -
    
    I left a message at your home this morning that your Dad would like to speak 
    with you when you have a chance to call.  
    
    Rosie
    
    p.s. - P. L. and I did early voting the first Saturday available!!  It was 
    such a good feeling as Tuesdays are tough with trying to get to the office 
    and leave in time to vote!!
    
    
    
    
    
    Elizabeth Lay <lizard_ar@yahoo.com> on 11/06/2000 09:13:26 AM
    To: ealvittor@yahoo.com
    cc:  
    Subject: Get out the Vote
    
    
    Dear Friends and Family,
    We are down to the wire and the race couldn't be
    closer. Every vote counts, even those of you in Texas.
    Please, remember to vote and go early, polls open at
    7am across the country and close at 7pm but don't put
    it off until 7pm as there could be lines and the
    supervisors are not required to keep the polls open
    after 7pm for those in line.
    Remind your friends and family to vote and if someone
    needs transportation to the polls, give them a ride or
    call your local Victory 2000 office (Republicans) or
    your local Democratic campaign office, they will have
    people available to transport people to the polls.
    Keep an eye out while you are at the polls for
    electoral fraud and report any suspicious activity
    including inappropriate campaigning to the poll
    watchers at the polls. The Democrats and the
    Republicans will have representatives at most of the
    polls, let them know if there is suspicious activity.
    Finally, if you have time, call your local offices and
    offer your time to make phone calls to encourage
    people to get out and vote, pass out leaflets, or to
    be available to help prevent of voter fraud.
    
    Finally, enjoy the evening and watch the results come
    in. There are election watch parties everywhere!!!
    
    Just one more day (and no more mass political e-mail's
    from me and everyone else)!
    Take care and VOTE!
    Liz
    
    __________________________________________________
    Do You Yahoo!?
    Thousands of Stores.  Millions of Products.  All in one Place.
    http://shopping.yahoo.com/
    
    
    Pleased to send you the November report. Obviously the market is weakening
    in North America which is making the quarter challenging but the underlying
    momentum of the company continues to improve as the report illustrates.
    Look forward to seeing you at the Board meeting.
    
    Regards,
    Peter
    
    
    
    > [Compaq Confidential - Internal Use Only]
    >
    > To:  Global Sales & Services Team
    >
    > Before I report on the great wins and other news this month, I want to
    > express a personal note about the organizational announcement earlier this
    > month.  I'm excited about the changes for all the reasons already
    > communicated - in particular strengthening the integration of our upstream
    > and downstream operations.  I'm also excited about Bo McBee and his
    > worldwide team in Corporate Quality and Customer Satisfaction officially
    > joining our organization.  He and his team are doing a great job, and
    > together we will further our efforts to become the leader throughout the
    > world in satisfying our customers.
    >
    > Most of all, I am extremely pleased and encouraged because I believe these
    > changes confirm the great work you have accomplished this year.  We've
    > already reported a number of major wins as a result of the joint efforts
    > by our Sales and Services teams.  There is an air of excitement and
    > anticipation about Compaq's momentum - I see it in the emails from many of
    > you and as I meet with our teams and customers around the world.  You're a
    > remarkable team and, as Michael puts it, let's keep the pedal to the metal
    > and keep the momentum strong as we work to successfully close 2000!
    >
    > Speaking of my travels...
    > This month I visited Johannesburg, South Africa, Dubai within the United
    > Arab Emirates, and Saudi Arabia.  All of these countries are part of
    > EMEA's Business Development Group (BDG), which is responsible for
    > developing Compaq business in 98 countries. The group is focused on both
    > developed and emerging markets in Eastern Europe, the Middle East and
    > Africa. Over the past six years, BDG has grown its revenue more than
    > 10-fold.
    >
    > In South Africa I visited Vodacom, which with 4 million subscribers, is
    > Africa's largest mobile phone network operator.  The company has just
    > upgraded its billing systems to handle further expansion, and to date is
    > one of the world's largest Wildfire installations with some 21 AlphaServer
    > GS systems.  I also spent time with the management of Mobile Telephone
    > Networks (MTN), South Africa's #2 cellphone operator and another big
    > Wildfire customer.  In fact, we just got word that they've placed a $10M
    > order for four GS320 AlphaServer systems and storage.
    >
    > One of my more interesting activities while there was learning more about
    > Ikageng, a Compaq-led initiative to bring the benefits of the information
    > age to the rural communities of Africa.  Ikageng brings together the
    > provision of safe drinking water, affordable healthcare, distance
    > learning, improved subsistence farming techniques and Internet access.
    > All of this is co-funded by a community bank, together with Compaq,
    > Johnnic, a South African media and information group, and the active
    > participation of the World Bank.  A real example of Inspiration Technology
    > at work!
    >
    > My visit to the United Arab Emirates included a dinner with our top 30
    > customers from across the region, a VIP lunch with our top partners, as
    > well as meetings with employees in the region.  I also attended Gitex, the
    > region's largest IT exhibition, and met with press at that event to convey
    > Compaq's commitment to the UAE.  I was also privileged to have a personal
    > meeting with His Highness Sheik Mohamad bin Rashid al Maktoum, Crown
    > Prince of Dubai and the Minister of Defense.  These meetings were around
    > the official opening of Dubai Internet City, an area of Dubai dedicated to
    > making the city the "Silicon Valley"  of the Middle East.
    >
    > I spent a very interesting day at Aramco in Saudi Arabia, our largest
    > account in the UAE. We are the ProLiant standard in this very large energy
    > company and we have a great opportnuity to build a strong partnership
    > across many additional solutions including high performance technical
    > computing, ZLE applications and enterprise storage, in addition to
    > recapturing client business from the competition
    >
    > Some of our largest wins this month
    > * Tokyo Stock Exchange - We are replacing Hitachi at the world's third
    > largest stock exchange, with a $60-80M order for Himalaya systems. This
    > contract should bring in an additional $20-30M in Professional Services.
    > *      Eli Lilly - Signed the first leg of a three-year global agreement
    > valued at $100M, securing Compaq as the sole supplier for Intel-based
    > products, forcing Dell off the customer's standards list and opening the
    > door for StorageWorks products, Global Services and high-performance
    > servers.
    > *      Winstar - Four-year, $100M contract as the exclusive provider of
    > Windows NT and storage products, including $10M in AlphaServer systems
    > running Tru64 UNIX.
    > *      Mead Corp. - Beat IBM, HP and Dell for a five-year, $50M contract
    > for ProLiant servers, storage, desktops, portables and services.
    > * France Telecom - $30M contract for a global agreement (includes all
    > subsidiaries) for a complete line of AlphaServer systems, including DS, ES
    > and GS series as well as ProLiant servers.
    > * General Motors - Selected as the global Intel-based server standard
    > for new application deployment at GM manufacturing facilities. The
    > anticipated global revenue is $30M over three years.
    > * Electronic Classroom of Tomorrow - $25M for ProLiant 8500 servers,
    > StorageWorks products and legacy-free iPAQ desktops.
    > * FleetBoston Financial - Beat IBM, Dell and HP for a $40M desktops
    > contract
    > * Airgroup (Switzerland) - Beat IBM and NetVista for a  $21M contract
    > for 20,000 iPAQ desktops.
    > * DLI (Korea) - $22M for Professional Services.
    > * AltaVista - Shut out IBM and HP by putting into place a $25M fair
    > market value lease for ProLiant- and Alpha-based servers, increasing the
    > AltaVista lease line to $75M.
    > * ASP Host Centric - One of the eight North America-certified Oracle
    > Authorized Application Providers (OAAP), the firm will standardize its
    > UNIX environment on AlphaServers, replacing Sun systems. This project
    > could generate more than $20M for us over the next 36 months.
    > * Interfusion - Three-year, $20M contract for a Tru64 UNIX-based
    > solution.
    > * Westcoast Energy- Topped Dell for a desktop and portables contract
    > valued between $15-20M.
    > * General Electric - Five-year, $15.4M contract for worldwide Lotus
    > Domino rollout and expansion of Exchange rollout, including NT Server
    > management outsourcing.
    > http://newscpq1.inline.cpqcorp.net/article.cfm?storyid=1146
    > * Moebel Pfister - $15.7M outsourcing contract.
    > * TriRiga - Beat Dell, EMC and Sun for a two-year, $15M contract for
    > storage, Professional Workstations, desktops, portables and services.
    >
    > EMEA to open Wireless Competence Centre in Stockholm
    > Press, customers and partners have been invited to help officially open
    > the Compaq Wireless Competence Centre in Stockholm, Sweden, on November
    > 27.  The centre is the company's first facility to fully display our
    > unique end-to-end capabilities of  solutions, services and products in the
    > mobile Internet and wireless space.  The hands-on centre showcases today's
    > wireless solutions within four environments - car, home, office and public
    > access areas.  Technologies featured include GSM, GPRS, future 3G
    > standards, WLAN and Bluetooth.  Compaq's mobile partners such as Nokia,
    > Oracle, Cisco, Microsoft, Siebel and Ericsson also plan to participate in
    > the opening.  The centre is already hosting customer visits and will
    > engage with thousands of customer and partners over the coming year
    > through a mix of seminars, tours and customized workshops.  For more info,
    > see  http://inline-se.soo.cpqcorp.net/wireless/
    >
    > Planning for Innovate Forum 2001 under way
    > Compaq's premier event for its global and large account customers -
    > Innovate Forum 2001 - is set for May 23-24 at the George R. Brown
    > Convention Center in Houston.  The hand-picked guest list will include
    > some 4,000-5,000 senior-level technical and business executives, including
    > our key channel partners, press, industry and financial analysts, and
    > Compaq's key alliance partners.  The program will feature keynote
    > speeches, plenary sessions, special interest seminars, a solutions
    > pavilion, and social events.  For more information, see the Innovate site
    > on Inline:  http://inline.compaq.com/na/innovate/
    >
    > Cross Border Office files first lawsuit
    > The Cross Border Office has been created to prevent unauthorized movement
    > of Compaq products by dealers and gray market brokers in order to protect
    > profit margins and ultimately, customer satisfaction.  The Cross Border
    > team provides gray market awareness training to all sales personnel, mail
    > and phone hotline access to report gray market activity, works jointly
    > with regional sales, services, business unit and channel teams to create
    > policy and procedures to reduce gray market activity and, working with the
    > Law department, to bring legal action against gray market brokers if
    > warranted.
    >
    > As a result of these efforts, Compaq has filed its first lawsuit against
    > two Canadian technology consulting firms for breach of contract and fraud.
    > The suit, which seeks compensatory damages of more than $17 million,
    > claims the consulting firms fraudulently represented to Compaq that they
    > had a contract with the U.S. Department of Transportation's Federal
    > Aviation Administration to supply a large number of computers and related
    > equipment to U.S. airports. This lawsuit hit many national publications
    > and sends a message to the worldwide gray market community that Compaq
    > will take actions to protect its authorized resellers, product quality and
    > our customers.
    >
    > For further information on the Cross Border Office, gray market red flags
    > and to view the web-based training video, see
    > http://inline.compaq.com/wwsm/crossborder/index.asp
    >
    > Key Channel Partner programs rolling out
    > Early this year the Tigerbite project was established to redefine and
    > simplify Compaq's model with our channel partners.  A key element of the
    > model is worldwide programs that provide profitable growth opportunities
    > for Compaq and its partners.  Two such programs - Internet List Pricing
    > (ILP) and the Compaq Agent Program - are currently being implemented by
    > the regions.
    >
    > Worldwide implementation of ILP is a top priority for the company.
    > Creating and publishing (where needed) competitive List Prices is
    > absolutely essential to establishing a more consistent, worldwide pricing
    > model for both our customers and partners. By the end of this month ILP
    > will have been implemented in North America, Latin America, Japan and
    > Greater China, with pilot programs in Singapore and Malaysia.  EMEA and
    > the remaining Asia Pacific countries are expected to complete the rollout
    > by January 1, 2001.
    >
    > The Compaq Agent Program, which allows partners to earn commissions when
    > they refer customers to purchase products/services directly from us,
    > currently has been implemented in the U.S., Latin America (14 countries)
    > and CKK.  This month, APD is implementing pilot programs in Singapore and
    > Malaysia, and plans to roll out the program in seven additional countries
    > in the first quarter.  EMEA held an Agent Program Summit this month with
    > 10 countries to assess and develop their 2001 rollout plans which include
    > adding Enterprise-class products to their program next year.
    >
    > News from the Compaq Alliances team
    > *      Compaq regained the #1 platform partner position with SAP with 33%
    > market share over all platforms (NT, UNIX with R/3, and mySAP.com). IBM is
    > 2nd in line with 23% share. In North America alone, our overall SAP share
    > increased from 25% to 32% in the third quarter.  As an aside, SAP's entire
    > executive board and senior executive staff use our iPAQ Pocket PCs.
    > Rollout of the product to SAP Sales and Marketing is also in progress -- a
    > very visible endorsement of Compaq's leadership in Internet access as it
    > applies to enterprise applications.
    > * Cable & Wireless CEO and executive visit to Marlboro in October
    > included CEO Graham Wallace and 56 top C&W executives. C&W new ASP
    > 'a-Services' UK launch on October 31 followed the successful U.S. launch
    > in late September.
    > * Compaq secured the notebook business with CGE&Y UK for their
    > internal use.  Toshiba had been the incumbent for 5 years.  CGE&Y is
    > upgrading to Oracle 11i on Alpha Tru64 UNIX.  As one of the first
    > customers globally to upgrade to 11i on Alpha, they have agreed to be a
    > reference site.
    > * Our successful Platinum Sponsorship of Commerce One's Global Trading
    > Web Technical Forum included a Compaq keynote and non-disclosure breakout
    > session on new ProLiant 8-ways.
    > * A  9-city roadshow in EMEA was kicked-off with Intel, starting in
    > Munich.  This is an extension of the successful 11 city roadshow in the
    > U.S. that drove traffic to the speedStart website and should do the same
    > for EMEA..
    > * Strong Compaq presence with Premier sponsorship at Oracle Open World
    > in October included Michael Capellas luncheon speech to 200+ C-level
    > customers, on-stage server presence at Larry Ellison keynote, and
    > excellent Compaq coverage in Oracle publications.
    > * Announced major Mid-Market Initiative Contract with Siebel.  We had
    > very high visibility at Siebel User Week, and also won Siebel's Platform
    > Partner of the Year awards for Excellence in EMEA and NA.  We recently
    > announced a Benchmark Figure of 10,200 Siebel users running Microsoft NT
    > and SQL 7 on ProLiant systems.
    > * Compaq had a strong presence at COMDEX with strategic partner,
    > Microsoft.  In addition to  supporting Bill Gates' keynote address, the
    > Microsoft booth featured iPAQ Pocket PCs demonstrating the award-winning
    > OmniSky wireless Internet and e-mail service running on Metricom's
    > Ricochet network - the world's fastest mobile broadband network.
    > Microsoft also announced the immediate availability of its Windows Media
    > Player Technology Preview Edition on Compaq Pocket PCX devices, which for
    > the first time delivers streamed wireless Windows Media-formatted audio
    > and video to a portable device.
    >
    > Global Accounts news
    > * Do you know about the Discovery, Design and Implementation (DDI)
    > application? Global Accounts has moved the DDI application into
    > production, resulting in a Web-enabled tool that streamlines and automates
    > the DDI phases for signing up new customers.
    > http://vinproapp03.cce.cpqcorp.net/ddi/
    > * More than 130 people from Compaq EMEA Global Accounts attended a
    > conference center at EuroDisney, Paris, for a training program that
    > included a focus on personal development skills and a broader look at how
    > Global Accounts can build sales.
    
    > * A CD and brochure designed to give Global Accounts salespeople and
    > customers a greater insight into the business can be ordered online
    > through the GA catalog.
    > http://inline.compaq.com/corpmktg/globalaccounts/know/resourcekit.asp
    > * For the first time, Compaq has a single, documented global special
    > pricing process, enabling us to be smarter than the competition on global
    > bids.  Implementation of this process is expected to begin January 1. For
    > more information, see
    > http://inline.compaq.com/corpmktg/globalaccounts/div/stratplan/index.asp
    > or e-mail Philip Kyle.
    > * Global Account managers and others whose customers and prospects
    > require multi-platform hardware, operating systems and applications will
    > want to know about the IQ Center. With more than 150 systems engineering
    > personnel, 30,000 square-feet of lab space, 500 CPUs and 100 TB of
    > storage, the Center is a well-equipped, one-stop shop for designing and
    > testing complex solutions.
    > http://inline.compaq.com/corpmktg/globalaccounts/div/gamclose.asp
    >
    > CPCG headlines
    > * Compaq regained total PC and PC server market share leadership in
    > the UK during 3Q.
    > * Among our many announcements at Comdex, we introduced the
    > three-pound, MP2800 - the world's smallest projector -- as well as
    > iPAQnet, a collection of products and solutions designed to redefine the
    > Internet experience for customers demanding wireless access to e-mail and
    > other corporate information. Last, Compaq and Oracle announced an all-new
    > Internet appliance based on ProLiant servers and the latest Oracle
    > software to deliver the fastest cache on the Internet. Oracle is backing
    > up the performance pledge with a $1 million guarantee.
    >
    > Ratings and reviews
    > * Computer Shopper named the iPAQ Pocket PC one of the "Top 100
    > Products of 2000"
    > * "Looking for the perfect present for the technophile who has
    > everything? Then check out the Compaq iPAQ Pocket PC ... the iPAQ is a lot
    > slimmer than most of the competition ... Plus, its brilliant 12-bit,
    > 4,096-color reflective display will be sure to make the holiday season
    > especially bright." - ZDNet
    > * Popular Science recognized the iPAQ Pocket PC at an awards ceremony
    > for being one of the year's 100 "hottest products and eye-opening
    > discoveries." The iPAQ Pocket PC is pictured on the cover of the
    > magazine's December edition, now on newsstands.
    > * "Sure, the Compaq iPAQ Pocket PC PDA has everything a desktop PC has
    > - word processor, Internet browser, e-mail engine, etc., etc. But that's
    > not even half the story: It can crank out color video and blast MP3 music
    > through a stereo headphone jack..." - Stuff Magazine
    >
    > Portables garner praise
    > * The Armada E500S received the "Four-Star Award" from Computer
    > Shopper. "Overall, the Armada E500S is a compelling, well-designed package
    > for small businesses ... you get a solid mix of components for the money."
    > - Computer Shopper
    > * The Notebook 100 was named one of the Top 100 Products of the Year
    > by Computer Shopper. "We were duly impressed with Compaq's price-defying
    > Notebook 100."
    >
    >
    > Consumer Group highlights
    > * Last month, we shipped our 500,000th Configure-to-Order unit. U.S.
    > CTO sales grew 256% in the third quarter.
    > * Worldwide beyond-the-box revenue in Q3 increased 90% year-over-year.
    > * Of the top 25 countries with the highest Consumer sales worldwide,
    > six are in the Latin America region: Mexico (2), Brazil (4), Argentina
    > (8), Chile (16), Peru/Bolivia (21) and Colombia (22).
    > * Consumer's EMEA region hit the $1 billion sales mark in mid-October,
    > two months earlier than in 1999.
    > * More than 50,000 DSL-ready Presario computers have been sold through
    > our deal with Southwestern Bell.
    > http://newscpq1.inline.cpqcorp.net/article.cfm?storyid=1034
    > * Popular Science magazine included the iPAQ Home Internet Appliance
    > in its "Best of What's New" in the computer and software category.
    >
    > Storage Product Group news
    > *      Compaq Belgium and Luxembourg have won four DATANEWS Awards for
    > Excellence, one of which was in the category of Enterprise Storage: Compaq
    > StorageWorks systems. Compaq also received Awards of Excellence for
    > Enterprise Server (ProLiant); High-End Workstations (Compaq Professional
    > Workstation), and Services. The Compaq Aero Professional Digital Assistant
    > (PDA) received a Quality Award. For more info, visit:
    > http://datanews.vnunet.be/dnafe0.asp
    > *      An elite group of storage networking companies has joined our
    > commitment to support VersaStor Technology - the industry's premier
    > implementation of networked storage pooling. These endorsements represent
    > an important milestone in enabling SAN customers to leverage business
    > information as a virtual resource.
    > http://www.compaq.com/newsroom/pr/2000/pr2000103002.html
    > * Construction has begun on the Storage Networking Industry
    > Association Technology Center (SNIA Technology Center) in Colorado
    > Springs, Colo. Upon completion, the 14,000-square-foot center will be the
    > largest independent storage networking lab in the world.
    > http://storage.inet.cpqcorp.net/download/doc/SNIA_Release_final.doc
    > * At last month's Storage Networking World conference, Compaq and IBM
    > demonstrated for the first time true multi-vendor online storage
    > interoperability for the Open SAN Earlier this month we announced three
    > new storage service offerings that accelerate SAN implementation, improve
    > enterprise backup performance and increase availability and reliability of
    > remote storage management.
    > http://www.compaq.com/newsroom/pr/2000/pr2000103001.html
    >
    > Business Critical Server Group highlights
    > * Our Tru64 UNIX business is gaining momentum - growing twice as fast
    > as the market in Q2 and Q3 of this year, according to International Data
    > Corp. IDC reports that Compaq was the fastest growing UNIX vendor in Q2,
    > with 25% growth versus overall UNIX market growth of 13%.
    > * On Oct. 31, we announced new Tru64 UNIX, TruCluster and AlphaServer
    > products and services enhancements to improve scalability and ease of
    > deployment for e-business solutions.
    > http://alphaserver.inet.cpqcorp.net/announcements/30oct00/index.html
    > * The International Tandem Users' Group (ITUG) Summit 2000, held Oct.
    > 15-19 in San Jose, Calif., was the largest ever, drawing 2,900 customers,
    > partners, internal developers and executives. A highlight of the general
    > session was a live demonstration of the Zero Latency Enterprise
    > architecture for customer relationship management, which brings together
    > Himalaya, AlphaServer and ProLiant platforms.
    >
    > North America eBusiness Solutions successes
    > *      Service Provider Winstar has signed Compaq as its exclusive
    > provider of NT and storage products and committed to purchase a minimum of
    > $100M of Compaq products over the next four years, $10M of which will be
    > for Alpha UNIX for their rapidly growing complex hosting business.  We're
    > also providing $50M in financing directly to Winstar and $50M in financing
    > to approved Winstar customers.  Compaq Services has been designated as a
    > Winstar Services Partner.
    > *      Exodus placed an initial order of more than 500 ProLiant servers
    > for their Intel-managed hosting platform.
    > * Compaq also inked a deal with Siebel Systems to create a dedicated
    > partner sales channel and a $30M joint marketing initiative for an
    > integrated hardware/software offering to small and medium enterprises.
    > Over 80 sales agents are being authorized to sell the packages, which are
    > delivered fully integrated by Compaq Direct.
    >
    > Compaq Financial Services making a difference
    > *      Compaq Financial Services was instrumental in helping to shut out
    > IBM and HP from long-time Compaq customer AltaVista by putting into place
    > a $25M fair market value lease for NT and AlphaServers.  Through the deal,
    > CFS increased Alta Vista's lease line to $75M.
    > * CFS scored its first local currency financing in Brazil with a $3M
    > deal for servers and services.  In awarding the contract over competitors
    > HP and IBM and their respective financing groups, Ericsson cited
    > differentiating factors including Compaq's technology and our ability to
    > provide a competitive price in local currency.  CFS invoicing
    > capabilities, including information for each separate Ericsson cost center
    > in Brazil, was also a deciding factor.
    > * CFS helped facilitate the largest delivery of Intel servers
    > (ProLiant ML 370) to the Czech Republic through a $2.8M, 3-year operating
    > lease transaction with Czech Savings Bank.  CFS was the only leasing
    > company to offer a sub-lease structure, a differentiating factor that won
    > the business over Dell and IBM.
    >
    > CEI changes name to Compaq Direct
    > Custom Edge Inc., a wholly owned Compaq subsidiary formed, is now called
    > Compaq Direct. In other "direct" news, did you know that we have more than
    > 230 major accounts now buying from us directly and more in the pipeline?
    > Combined revenue in Q3 from PartnerDirect, DirectPlus, Major Account
    > Direct and GEM Direct totaled nearly 40 percent of CPCG's total revenue in
    > North America. What's more, ISSG revenue was more than 27 percent direct
    > in Q3.
    >
    > Siebel's Platform Partner of the Year
    > I'm pleased to report that Siebel has named Compaq its Platform Partner of
    > the Year for excellence in both EMEA and North America. We recently
    > received high visibility at the Siebel User Week event while also
    > announcing a record benchmark of 10,200 Siebel users running Microsoft NT
    > and SQL 7 on ProLiant servers.
    >
    > Get Informed
    > Inform, Compaq's customer magazine, is now available in printed and
    > electronic versions. It's free and available for you to read. Sign your
    > customers up by visiting the U.S. (www.compaq.com/inform/issues/sb.html),
    > Asia Pacific (www.compaq.com.tw/) or EMEA (www.compaq.com/emea/inform)
    > sites.
    >
    > North America eCommerce and CRM marketing activities
    > * North America recently released IMPAQ express, a Web-based tool for
    > Customer Relationship Management (CRM) campaign planning and audience
    > sizing, to its marketing and sales force. For the first time, campaign
    > planning can start with a quick and easy look at the size and scope of a
    > potential installed-based audience.
    > * Compaq recently co-sponsored eLink, a B2B e-commerce event targeted
    > at procurement, IT, marketing and financial executives hosted by Commerce
    > One in Las Vegas. Attendees witnessed the on-stage construction of a live
    > e-marketplace powered by Commerce One and Compaq servers. In addition, we
    > demonstrated our Roundtrip configuration and Auction capabilities.
    >
    > Wins Around the World
    > As always, thanks to everyone for your tremendous efforts this month.
    > Please take a few minutes to look over the complete list of recent wins
    > around the world and continue to write me with your news and success
    > stories. http://inline.compaq.com/wwss/wins/worldwins.asp
    >
    > Let's finish the quarter strongly!!
    >
    > Regards,
    > Peter
    >
    > 
    Ann McGarry
    75 Hillcrest Avenue
    Rye Brook, NY 10573
    mcgarrymusic@yahoo.com
    
    To Mr. Ken Lay,
    
    I'm writing to demand that you disgorge the millions of dollars you intentionally made from fraudulently selling Enron stock before the company declared bankruptcy, to funds such as Enron Employee Transition Fund and REACH, that benefit the company's employees, who lost their retirement savings, and provide relief to low-income consumers in California, who can't afford to pay their energy bills.  Enron and you made millions out of the pocketbooks of California consumers and from the efforts of your employees.  You are a disgrace to the human race and I hope that you and your kind suffer the kind of punishment you really deserve.
    
    While you netted well over a $100 million, many of Enron's employees were financially devastated when the company declared bankruptcy and their retirement plans were wiped out.  And Enron made an astronomical profit during the California energy crisis last year.  As a result, there are thousands of consumers who are unable to pay their basic energy bills and the largest utility in the state is bankrupt.
    
    The New York Times reported that you sold $101 million worth of Enron stock while aggressively urging the company's employees to keep buying it. You are disgusting. It seems to me that anyone who is so in the grip of out-of-control GREED as you and the others of your kind in this country, e.g., Bush, Cheney, Lott, etc., are actually mentally ill.  No other explanation suffices.  It would be in your best interests to donate your ill-gotten gains to the funds set up to help repair the lives of those Americans hurt by Enron's underhanded dealings.
    
    With contempt,
    
    Ann McGarry
    Dear Friends and Family,
    We are down to the wire and the race couldn't be
    closer. Every vote counts, even those of you in Texas.
    Please, remember to vote and go early, polls open at
    7am across the country and close at 7pm but don't put
    it off until 7pm as there could be lines and the
    supervisors are not required to keep the polls open
    after 7pm for those in line.
    Remind your friends and family to vote and if someone
    needs transportation to the polls, give them a ride or
    call your local Victory 2000 office (Republicans) or
    your local Democratic campaign office, they will have
    people available to transport people to the polls.
    Keep an eye out while you are at the polls for
    electoral fraud and report any suspicious activity
    including inappropriate campaigning to the poll
    watchers at the polls. The Democrats and the
    Republicans will have representatives at most of the
    polls, let them know if there is suspicious activity.
    Finally, if you have time, call your local offices and
    offer your time to make phone calls to encourage
    people to get out and vote, pass out leaflets, or to
    be available to help prevent of voter fraud.
    
    Finally, enjoy the evening and watch the results come
    in. There are election watch parties everywhere!!!
    
    Just one more day (and no more mass political e-mail's
    from me and everyone else)!
    Take care and VOTE!
    Liz
    
    __________________________________________________
    Do You Yahoo!?
    Thousands of Stores.  Millions of Products.  All in one Place.
    http://shopping.yahoo.com/
    Good morning, Liz -
    
    I left a message at your home this morning that your Dad would like to speak 
    with you when you have a chance to call.  
    
    Rosie
    
    p.s. - P. L. and I did early voting the first Saturday available!!  It was 
    such a good feeling as Tuesdays are tough with trying to get to the office 
    and leave in time to vote!!
    
    
    
    
    
    Elizabeth Lay <lizard_ar@yahoo.com> on 11/06/2000 09:13:26 AM
    To: ealvittor@yahoo.com
    cc:  
    Subject: Get out the Vote
    
    
    Dear Friends and Family,
    We are down to the wire and the race couldn't be
    closer. Every vote counts, even those of you in Texas.
    Please, remember to vote and go early, polls open at
    7am across the country and close at 7pm but don't put
    it off until 7pm as there could be lines and the
    supervisors are not required to keep the polls open
    after 7pm for those in line.
    Remind your friends and family to vote and if someone
    needs transportation to the polls, give them a ride or
    call your local Victory 2000 office (Republicans) or
    your local Democratic campaign office, they will have
    people available to transport people to the polls.
    Keep an eye out while you are at the polls for
    electoral fraud and report any suspicious activity
    including inappropriate campaigning to the poll
    watchers at the polls. The Democrats and the
    Republicans will have representatives at most of the
    polls, let them know if there is suspicious activity.
    Finally, if you have time, call your local offices and
    offer your time to make phone calls to encourage
    people to get out and vote, pass out leaflets, or to
    be available to help prevent of voter fraud.
    
    Finally, enjoy the evening and watch the results come
    in. There are election watch parties everywhere!!!
    
    Just one more day (and no more mass political e-mail's
    from me and everyone else)!
    Take care and VOTE!
    Liz
    
    __________________________________________________
    Do You Yahoo!?
    Thousands of Stores.  Millions of Products.  All in one Place.
    http://shopping.yahoo.com/
    
    
    Pleased to send you the November report. Obviously the market is weakening
    in North America which is making the quarter challenging but the underlying
    momentum of the company continues to improve as the report illustrates.
    Look forward to seeing you at the Board meeting.
    
    Regards,
    Peter
    
    
    
    > [Compaq Confidential - Internal Use Only]
    >
    > To:  Global Sales & Services Team
    >
    > Before I report on the great wins and other news this month, I want to
    > express a personal note about the organizational announcement earlier this
    > month.  I'm excited about the changes for all the reasons already
    > communicated - in particular strengthening the integration of our upstream
    > and downstream operations.  I'm also excited about Bo McBee and his
    > worldwide team in Corporate Quality and Customer Satisfaction officially
    > joining our organization.  He and his team are doing a great job, and
    > together we will further our efforts to become the leader throughout the
    > world in satisfying our customers.
    >
    > Most of all, I am extremely pleased and encouraged because I believe these
    > changes confirm the great work you have accomplished this year.  We've
    > already reported a number of major wins as a result of the joint efforts
    > by our Sales and Services teams.  There is an air of excitement and
    > anticipation about Compaq's momentum - I see it in the emails from many of
    > you and as I meet with our teams and customers around the world.  You're a
    > remarkable team and, as Michael puts it, let's keep the pedal to the metal
    > and keep the momentum strong as we work to successfully close 2000!
    >
    > Speaking of my travels...
    > This month I visited Johannesburg, South Africa, Dubai within the United
    > Arab Emirates, and Saudi Arabia.  All of these countries are part of
    > EMEA's Business Development Group (BDG), which is responsible for
    > developing Compaq business in 98 countries. The group is focused on both
    > developed and emerging markets in Eastern Europe, the Middle East and
    > Africa. Over the past six years, BDG has grown its revenue more than
    > 10-fold.
    >
    > In South Africa I visited Vodacom, which with 4 million subscribers, is
    > Africa's largest mobile phone network operator.  The company has just
    > upgraded its billing systems to handle further expansion, and to date is
    > one of the world's largest Wildfire installations with some 21 AlphaServer
    > GS systems.  I also spent time with the management of Mobile Telephone
    > Networks (MTN), South Africa's #2 cellphone operator and another big
    > Wildfire customer.  In fact, we just got word that they've placed a $10M
    > order for four GS320 AlphaServer systems and storage.
    >
    > One of my more interesting activities while there was learning more about
    > Ikageng, a Compaq-led initiative to bring the benefits of the information
    > age to the rural communities of Africa.  Ikageng brings together the
    > provision of safe drinking water, affordable healthcare, distance
    > learning, improved subsistence farming techniques and Internet access.
    > All of this is co-funded by a community bank, together with Compaq,
    > Johnnic, a South African media and information group, and the active
    > participation of the World Bank.  A real example of Inspiration Technology
    > at work!
    >
    > My visit to the United Arab Emirates included a dinner with our top 30
    > customers from across the region, a VIP lunch with our top partners, as
    > well as meetings with employees in the region.  I also attended Gitex, the
    > region's largest IT exhibition, and met with press at that event to convey
    > Compaq's commitment to the UAE.  I was also privileged to have a personal
    > meeting with His Highness Sheik Mohamad bin Rashid al Maktoum, Crown
    > Prince of Dubai and the Minister of Defense.  These meetings were around
    > the official opening of Dubai Internet City, an area of Dubai dedicated to
    > making the city the "Silicon Valley"  of the Middle East.
    >
    > I spent a very interesting day at Aramco in Saudi Arabia, our largest
    > account in the UAE. We are the ProLiant standard in this very large energy
    > company and we have a great opportnuity to build a strong partnership
    > across many additional solutions including high performance technical
    > computing, ZLE applications and enterprise storage, in addition to
    > recapturing client business from the competition
    >
    > Some of our largest wins this month
    > * Tokyo Stock Exchange - We are replacing Hitachi at the world's third
    > largest stock exchange, with a $60-80M order for Himalaya systems. This
    > contract should bring in an additional $20-30M in Professional Services.
    > *      Eli Lilly - Signed the first leg of a three-year global agreement
    > valued at $100M, securing Compaq as the sole supplier for Intel-based
    > products, forcing Dell off the customer's standards list and opening the
    > door for StorageWorks products, Global Services and high-performance
    > servers.
    > *      Winstar - Four-year, $100M contract as the exclusive provider of
    > Windows NT and storage products, including $10M in AlphaServer systems
    > running Tru64 UNIX.
    > *      Mead Corp. - Beat IBM, HP and Dell for a five-year, $50M contract
    > for ProLiant servers, storage, desktops, portables and services.
    > * France Telecom - $30M contract for a global agreement (includes all
    > subsidiaries) for a complete line of AlphaServer systems, including DS, ES
    > and GS series as well as ProLiant servers.
    > * General Motors - Selected as the global Intel-based server standard
    > for new application deployment at GM manufacturing facilities. The
    > anticipated global revenue is $30M over three years.
    > * Electronic Classroom of Tomorrow - $25M for ProLiant 8500 servers,
    > StorageWorks products and legacy-free iPAQ desktops.
    > * FleetBoston Financial - Beat IBM, Dell and HP for a $40M desktops
    > contract
    > * Airgroup (Switzerland) - Beat IBM and NetVista for a  $21M contract
    > for 20,000 iPAQ desktops.
    > * DLI (Korea) - $22M for Professional Services.
    > * AltaVista - Shut out IBM and HP by putting into place a $25M fair
    > market value lease for ProLiant- and Alpha-based servers, increasing the
    > AltaVista lease line to $75M.
    > * ASP Host Centric - One of the eight North America-certified Oracle
    > Authorized Application Providers (OAAP), the firm will standardize its
    > UNIX environment on AlphaServers, replacing Sun systems. This project
    > could generate more than $20M for us over the next 36 months.
    > * Interfusion - Three-year, $20M contract for a Tru64 UNIX-based
    > solution.
    > * Westcoast Energy- Topped Dell for a desktop and portables contract
    > valued between $15-20M.
    > * General Electric - Five-year, $15.4M contract for worldwide Lotus
    > Domino rollout and expansion of Exchange rollout, including NT Server
    > management outsourcing.
    > http://newscpq1.inline.cpqcorp.net/article.cfm?storyid=1146
    > * Moebel Pfister - $15.7M outsourcing contract.
    > * TriRiga - Beat Dell, EMC and Sun for a two-year, $15M contract for
    > storage, Professional Workstations, desktops, portables and services.
    >
    > EMEA to open Wireless Competence Centre in Stockholm
    > Press, customers and partners have been invited to help officially open
    > the Compaq Wireless Competence Centre in Stockholm, Sweden, on November
    > 27.  The centre is the company's first facility to fully display our
    > unique end-to-end capabilities of  solutions, services and products in the
    > mobile Internet and wireless space.  The hands-on centre showcases today's
    > wireless solutions within four environments - car, home, office and public
    > access areas.  Technologies featured include GSM, GPRS, future 3G
    > standards, WLAN and Bluetooth.  Compaq's mobile partners such as Nokia,
    > Oracle, Cisco, Microsoft, Siebel and Ericsson also plan to participate in
    > the opening.  The centre is already hosting customer visits and will
    > engage with thousands of customer and partners over the coming year
    > through a mix of seminars, tours and customized workshops.  For more info,
    > see  http://inline-se.soo.cpqcorp.net/wireless/
    >
    > Planning for Innovate Forum 2001 under way
    > Compaq's premier event for its global and large account customers -
    > Innovate Forum 2001 - is set for May 23-24 at the George R. Brown
    > Convention Center in Houston.  The hand-picked guest list will include
    > some 4,000-5,000 senior-level technical and business executives, including
    > our key channel partners, press, industry and financial analysts, and
    > Compaq's key alliance partners.  The program will feature keynote
    > speeches, plenary sessions, special interest seminars, a solutions
    > pavilion, and social events.  For more information, see the Innovate site
    > on Inline:  http://inline.compaq.com/na/innovate/
    >
    > Cross Border Office files first lawsuit
    > The Cross Border Office has been created to prevent unauthorized movement
    > of Compaq products by dealers and gray market brokers in order to protect
    > profit margins and ultimately, customer satisfaction.  The Cross Border
    > team provides gray market awareness training to all sales personnel, mail
    > and phone hotline access to report gray market activity, works jointly
    > with regional sales, services, business unit and channel teams to create
    > policy and procedures to reduce gray market activity and, working with the
    > Law department, to bring legal action against gray market brokers if
    > warranted.
    >
    > As a result of these efforts, Compaq has filed its first lawsuit against
    > two Canadian technology consulting firms for breach of contract and fraud.
    > The suit, which seeks compensatory damages of more than $17 million,
    > claims the consulting firms fraudulently represented to Compaq that they
    > had a contract with the U.S. Department of Transportation's Federal
    > Aviation Administration to supply a large number of computers and related
    > equipment to U.S. airports. This lawsuit hit many national publications
    > and sends a message to the worldwide gray market community that Compaq
    > will take actions to protect its authorized resellers, product quality and
    > our customers.
    >
    > For further information on the Cross Border Office, gray market red flags
    > and to view the web-based training video, see
    > http://inline.compaq.com/wwsm/crossborder/index.asp
    >
    > Key Channel Partner programs rolling out
    > Early this year the Tigerbite project was established to redefine and
    > simplify Compaq's model with our channel partners.  A key element of the
    > model is worldwide programs that provide profitable growth opportunities
    > for Compaq and its partners.  Two such programs - Internet List Pricing
    > (ILP) and the Compaq Agent Program - are currently being implemented by
    > the regions.
    >
    > Worldwide implementation of ILP is a top priority for the company.
    > Creating and publishing (where needed) competitive List Prices is
    > absolutely essential to establishing a more consistent, worldwide pricing
    > model for both our customers and partners. By the end of this month ILP
    > will have been implemented in North America, Latin America, Japan and
    > Greater China, with pilot programs in Singapore and Malaysia.  EMEA and
    > the remaining Asia Pacific countries are expected to complete the rollout
    > by January 1, 2001.
    >
    > The Compaq Agent Program, which allows partners to earn commissions when
    > they refer customers to purchase products/services directly from us,
    > currently has been implemented in the U.S., Latin America (14 countries)
    > and CKK.  This month, APD is implementing pilot programs in Singapore and
    > Malaysia, and plans to roll out the program in seven additional countries
    > in the first quarter.  EMEA held an Agent Program Summit this month with
    > 10 countries to assess and develop their 2001 rollout plans which include
    > adding Enterprise-class products to their program next year.
    >
    > News from the Compaq Alliances team
    > *      Compaq regained the #1 platform partner position with SAP with 33%
    > market share over all platforms (NT, UNIX with R/3, and mySAP.com). IBM is
    > 2nd in line with 23% share. In North America alone, our overall SAP share
    > increased from 25% to 32% in the third quarter.  As an aside, SAP's entire
    > executive board and senior executive staff use our iPAQ Pocket PCs.
    > Rollout of the product to SAP Sales and Marketing is also in progress -- a
    > very visible endorsement of Compaq's leadership in Internet access as it
    > applies to enterprise applications.
    > * Cable & Wireless CEO and executive visit to Marlboro in October
    > included CEO Graham Wallace and 56 top C&W executives. C&W new ASP
    > 'a-Services' UK launch on October 31 followed the successful U.S. launch
    > in late September.
    > * Compaq secured the notebook business with CGE&Y UK for their
    > internal use.  Toshiba had been the incumbent for 5 years.  CGE&Y is
    > upgrading to Oracle 11i on Alpha Tru64 UNIX.  As one of the first
    > customers globally to upgrade to 11i on Alpha, they have agreed to be a
    > reference site.
    > * Our successful Platinum Sponsorship of Commerce One's Global Trading
    > Web Technical Forum included a Compaq keynote and non-disclosure breakout
    > session on new ProLiant 8-ways.
    > * A  9-city roadshow in EMEA was kicked-off with Intel, starting in
    > Munich.  This is an extension of the successful 11 city roadshow in the
    > U.S. that drove traffic to the speedStart website and should do the same
    > for EMEA..
    > * Strong Compaq presence with Premier sponsorship at Oracle Open World
    > in October included Michael Capellas luncheon speech to 200+ C-level
    > customers, on-stage server presence at Larry Ellison keynote, and
    > excellent Compaq coverage in Oracle publications.
    > * Announced major Mid-Market Initiative Contract with Siebel.  We had
    > very high visibility at Siebel User Week, and also won Siebel's Platform
    > Partner of the Year awards for Excellence in EMEA and NA.  We recently
    > announced a Benchmark Figure of 10,200 Siebel users running Microsoft NT
    > and SQL 7 on ProLiant systems.
    > * Compaq had a strong presence at COMDEX with strategic partner,
    > Microsoft.  In addition to  supporting Bill Gates' keynote address, the
    > Microsoft booth featured iPAQ Pocket PCs demonstrating the award-winning
    > OmniSky wireless Internet and e-mail service running on Metricom's
    > Ricochet network - the world's fastest mobile broadband network.
    > Microsoft also announced the immediate availability of its Windows Media
    > Player Technology Preview Edition on Compaq Pocket PCX devices, which for
    > the first time delivers streamed wireless Windows Media-formatted audio
    > and video to a portable device.
    >
    > Global Accounts news
    > * Do you know about the Discovery, Design and Implementation (DDI)
    > application? Global Accounts has moved the DDI application into
    > production, resulting in a Web-enabled tool that streamlines and automates
    > the DDI phases for signing up new customers.
    > http://vinproapp03.cce.cpqcorp.net/ddi/
    > * More than 130 people from Compaq EMEA Global Accounts attended a
    > conference center at EuroDisney, Paris, for a training program that
    > included a focus on personal development skills and a broader look at how
    > Global Accounts can build sales.
    
    > * A CD and brochure designed to give Global Accounts salespeople and
    > customers a greater insight into the business can be ordered online
    > through the GA catalog.
    > http://inline.compaq.com/corpmktg/globalaccounts/know/resourcekit.asp
    > * For the first time, Compaq has a single, documented global special
    > pricing process, enabling us to be smarter than the competition on global
    > bids.  Implementation of this process is expected to begin January 1. For
    > more information, see
    > http://inline.compaq.com/corpmktg/globalaccounts/div/stratplan/index.asp
    > or e-mail Philip Kyle.
    > * Global Account managers and others whose customers and prospects
    > require multi-platform hardware, operating systems and applications will
    > want to know about the IQ Center. With more than 150 systems engineering
    > personnel, 30,000 square-feet of lab space, 500 CPUs and 100 TB of
    > storage, the Center is a well-equipped, one-stop shop for designing and
    > testing complex solutions.
    > http://inline.compaq.com/corpmktg/globalaccounts/div/gamclose.asp
    >
    > CPCG headlines
    > * Compaq regained total PC and PC server market share leadership in
    > the UK during 3Q.
    > * Among our many announcements at Comdex, we introduced the
    > three-pound, MP2800 - the world's smallest projector -- as well as
    > iPAQnet, a collection of products and solutions designed to redefine the
    > Internet experience for customers demanding wireless access to e-mail and
    > other corporate information. Last, Compaq and Oracle announced an all-new
    > Internet appliance based on ProLiant servers and the latest Oracle
    > software to deliver the fastest cache on the Internet. Oracle is backing
    > up the performance pledge with a $1 million guarantee.
    >
    > Ratings and reviews
    > * Computer Shopper named the iPAQ Pocket PC one of the "Top 100
    > Products of 2000"
    > * "Looking for the perfect present for the technophile who has
    > everything? Then check out the Compaq iPAQ Pocket PC ... the iPAQ is a lot
    > slimmer than most of the competition ... Plus, its brilliant 12-bit,
    > 4,096-color reflective display will be sure to make the holiday season
    > especially bright." - ZDNet
    > * Popular Science recognized the iPAQ Pocket PC at an awards ceremony
    > for being one of the year's 100 "hottest products and eye-opening
    > discoveries." The iPAQ Pocket PC is pictured on the cover of the
    > magazine's December edition, now on newsstands.
    > * "Sure, the Compaq iPAQ Pocket PC PDA has everything a desktop PC has
    > - word processor, Internet browser, e-mail engine, etc., etc. But that's
    > not even half the story: It can crank out color video and blast MP3 music
    > through a stereo headphone jack..." - Stuff Magazine
    >
    > Portables garner praise
    > * The Armada E500S received the "Four-Star Award" from Computer
    > Shopper. "Overall, the Armada E500S is a compelling, well-designed package
    > for small businesses ... you get a solid mix of components for the money."
    > - Computer Shopper
    > * The Notebook 100 was named one of the Top 100 Products of the Year
    > by Computer Shopper. "We were duly impressed with Compaq's price-defying
    > Notebook 100."
    >
    >
    > Consumer Group highlights
    > * Last month, we shipped our 500,000th Configure-to-Order unit. U.S.
    > CTO sales grew 256% in the third quarter.
    > * Worldwide beyond-the-box revenue in Q3 increased 90% year-over-year.
    > * Of the top 25 countries with the highest Consumer sales worldwide,
    > six are in the Latin America region: Mexico (2), Brazil (4), Argentina
    > (8), Chile (16), Peru/Bolivia (21) and Colombia (22).
    > * Consumer's EMEA region hit the $1 billion sales mark in mid-October,
    > two months earlier than in 1999.
    > * More than 50,000 DSL-ready Presario computers have been sold through
    > our deal with Southwestern Bell.
    > http://newscpq1.inline.cpqcorp.net/article.cfm?storyid=1034
    > * Popular Science magazine included the iPAQ Home Internet Appliance
    > in its "Best of What's New" in the computer and software category.
    >
    > Storage Product Group news
    > *      Compaq Belgium and Luxembourg have won four DATANEWS Awards for
    > Excellence, one of which was in the category of Enterprise Storage: Compaq
    > StorageWorks systems. Compaq also received Awards of Excellence for
    > Enterprise Server (ProLiant); High-End Workstations (Compaq Professional
    > Workstation), and Services. The Compaq Aero Professional Digital Assistant
    > (PDA) received a Quality Award. For more info, visit:
    > http://datanews.vnunet.be/dnafe0.asp
    > *      An elite group of storage networking companies has joined our
    > commitment to support VersaStor Technology - the industry's premier
    > implementation of networked storage pooling. These endorsements represent
    > an important milestone in enabling SAN customers to leverage business
    > information as a virtual resource.
    > http://www.compaq.com/newsroom/pr/2000/pr2000103002.html
    > * Construction has begun on the Storage Networking Industry
    > Association Technology Center (SNIA Technology Center) in Colorado
    > Springs, Colo. Upon completion, the 14,000-square-foot center will be the
    > largest independent storage networking lab in the world.
    > http://storage.inet.cpqcorp.net/download/doc/SNIA_Release_final.doc
    > * At last month's Storage Networking World conference, Compaq and IBM
    > demonstrated for the first time true multi-vendor online storage
    > interoperability for the Open SAN Earlier this month we announced three
    > new storage service offerings that accelerate SAN implementation, improve
    > enterprise backup performance and increase availability and reliability of
    > remote storage management.
    > http://www.compaq.com/newsroom/pr/2000/pr2000103001.html
    >
    > Business Critical Server Group highlights
    > * Our Tru64 UNIX business is gaining momentum - growing twice as fast
    > as the market in Q2 and Q3 of this year, according to International Data
    > Corp. IDC reports that Compaq was the fastest growing UNIX vendor in Q2,
    > with 25% growth versus overall UNIX market growth of 13%.
    > * On Oct. 31, we announced new Tru64 UNIX, TruCluster and AlphaServer
    > products and services enhancements to improve scalability and ease of
    > deployment for e-business solutions.
    > http://alphaserver.inet.cpqcorp.net/announcements/30oct00/index.html
    > * The International Tandem Users' Group (ITUG) Summit 2000, held Oct.
    > 15-19 in San Jose, Calif., was the largest ever, drawing 2,900 customers,
    > partners, internal developers and executives. A highlight of the general
    > session was a live demonstration of the Zero Latency Enterprise
    > architecture for customer relationship management, which brings together
    > Himalaya, AlphaServer and ProLiant platforms.
    >
    > North America eBusiness Solutions successes
    > *      Service Provider Winstar has signed Compaq as its exclusive
    > provider of NT and storage products and committed to purchase a minimum of
    > $100M of Compaq products over the next four years, $10M of which will be
    > for Alpha UNIX for their rapidly growing complex hosting business.  We're
    > also providing $50M in financing directly to Winstar and $50M in financing
    > to approved Winstar customers.  Compaq Services has been designated as a
    > Winstar Services Partner.
    > *      Exodus placed an initial order of more than 500 ProLiant servers
    > for their Intel-managed hosting platform.
    > * Compaq also inked a deal with Siebel Systems to create a dedicated
    > partner sales channel and a $30M joint marketing initiative for an
    > integrated hardware/software offering to small and medium enterprises.
    > Over 80 sales agents are being authorized to sell the packages, which are
    > delivered fully integrated by Compaq Direct.
    >
    > Compaq Financial Services making a difference
    > *      Compaq Financial Services was instrumental in helping to shut out
    > IBM and HP from long-time Compaq customer AltaVista by putting into place
    > a $25M fair market value lease for NT and AlphaServers.  Through the deal,
    > CFS increased Alta Vista's lease line to $75M.
    > * CFS scored its first local currency financing in Brazil with a $3M
    > deal for servers and services.  In awarding the contract over competitors
    > HP and IBM and their respective financing groups, Ericsson cited
    > differentiating factors including Compaq's technology and our ability to
    > provide a competitive price in local currency.  CFS invoicing
    > capabilities, including information for each separate Ericsson cost center
    > in Brazil, was also a deciding factor.
    > * CFS helped facilitate the largest delivery of Intel servers
    > (ProLiant ML 370) to the Czech Republic through a $2.8M, 3-year operating
    > lease transaction with Czech Savings Bank.  CFS was the only leasing
    > company to offer a sub-lease structure, a differentiating factor that won
    > the business over Dell and IBM.
    >
    > CEI changes name to Compaq Direct
    > Custom Edge Inc., a wholly owned Compaq subsidiary formed, is now called
    > Compaq Direct. In other "direct" news, did you know that we have more than
    > 230 major accounts now buying from us directly and more in the pipeline?
    > Combined revenue in Q3 from PartnerDirect, DirectPlus, Major Account
    > Direct and GEM Direct totaled nearly 40 percent of CPCG's total revenue in
    > North America. What's more, ISSG revenue was more than 27 percent direct
    > in Q3.
    >
    > Siebel's Platform Partner of the Year
    > I'm pleased to report that Siebel has named Compaq its Platform Partner of
    > the Year for excellence in both EMEA and North America. We recently
    > received high visibility at the Siebel User Week event while also
    > announcing a record benchmark of 10,200 Siebel users running Microsoft NT
    > and SQL 7 on ProLiant servers.
    >
    > Get Informed
    > Inform, Compaq's customer magazine, is now available in printed and
    > electronic versions. It's free and available for you to read. Sign your
    > customers up by visiting the U.S. (www.compaq.com/inform/issues/sb.html),
    > Asia Pacific (www.compaq.com.tw/) or EMEA (www.compaq.com/emea/inform)
    > sites.
    >
    > North America eCommerce and CRM marketing activities
    > * North America recently released IMPAQ express, a Web-based tool for
    > Customer Relationship Management (CRM) campaign planning and audience
    > sizing, to its marketing and sales force. For the first time, campaign
    > planning can start with a quick and easy look at the size and scope of a
    > potential installed-based audience.
    > * Compaq recently co-sponsored eLink, a B2B e-commerce event targeted
    > at procurement, IT, marketing and financial executives hosted by Commerce
    > One in Las Vegas. Attendees witnessed the on-stage construction of a live
    > e-marketplace powered by Commerce One and Compaq servers. In addition, we
    > demonstrated our Roundtrip configuration and Auction capabilities.
    >
    > Wins Around the World
    > As always, thanks to everyone for your tremendous efforts this month.
    > Please take a few minutes to look over the complete list of recent wins
    > around the world and continue to write me with your news and success
    > stories. http://inline.compaq.com/wwss/wins/worldwins.asp
    >
    > Let's finish the quarter strongly!!
    >
    > Regards,
    > Peter
    >
    > 
    Tue, 12 Dec 2000 18:49:21 -0600
    Date: Tue, 12 Dec 2000 18:49:21 -0600
    Message-Id: <200012130049.SAA07799@server1.pjdoland.com>
    To: klay@enron.com
    From: David Theroux <DJTheroux@independent.org>
    Reply-to: DJTheroux@independent.org
    X-Mailer: Perl Powered Socket Mailer
    Subject: THE LIGHTHOUSE: December 12, 2000
    
    THE LIGHTHOUSE
    "Enlightening Ideas for Public Policy..."
    VOL. 2, ISSUE 48
    December 12, 2000
    
    Welcome to The Lighthouse, the e-mail newsletter of The Independent 
    Institute, the non-partisan, public policy research organization 
    <http://www.independent.org>. We provide you with updates of the Institute's 
    current research publications, events and media programs.
    
    -------------------------------------------------------------
    
    IN THIS WEEK'S ISSUE:
    1. Pentagon "Shocked" to Find Rivers Dammed with Pork
    2. The Environmental Propaganda Agency
    3. William Lloyd Garrison, Antislavery Crusader
    
    -------------------------------------------------------------
    
    PENTAGON "SHOCKED" TO FIND RIVERS DAMMED WITH PORK
    
    Captain Louis Renault -- Claude Raines's cheerfully duplicitous character in 
    the 1942 film classic "Casablanca" -- asserted glibly that he was "shocked, 
    shocked" to learn that gambling was taking place at Rick's Cafe. Moments 
    later he was only too happy to collect his gambling earnings for the night.
    
    All this is by way of preamble to a new Pentagon investigation of fraud in 
    military construction. The investigation concluded that three senior Army 
    Corps of Engineers officials had, just as one whistle-blowing Corps economist 
    had claimed, engaged in a deceitful campaign to justify what the Washington 
    Post called "a billion-dollar construction binge on the Mississippi and 
    Illinois rivers."
    
    "The [Pentagon] investigators concluded that the agency's aggressive efforts 
    to expand its budget and missions, as well as its eagerness to please its 
    corporate customers and congressional patrons, have helped 'create an 
    atmosphere where objectivity in its analyses was placed in jeopardy,'" the 
    Post reports.
    
    "Even the agency's retired chief economist told them that Corps studies were 
    often 'corrupt,' and that several Corps employees cited 'immense pressure' to 
    green-light questionable projects."
    
    Bureaucratic boondoggles of such magnitude are certainly newsworthy. But they 
    are hardly news. Just as the Soviets derided the failures of previous Five 
    Year Plans (only to implement new, equally flawed versions), so it seems that 
    every few years the Pentagon uncovers massive corruption and waste in its own 
    centrally planned fiefdom -- only to present a new Plan that operates under 
    the same bad incentives that encouraged prior malfeasance.
    
    With corruption and waste seemingly "taken care of," the worst pork-barrel 
    spenders in Congress and the military are then let off the hook, only to 
    enjoy -- like Casablanca's Renault and Rick -- an amicable toast to the 
    beginnings of a beautiful new friendship.
    
    For the Washington Post series on the Army Corp of Engineers boondoggle, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-1.html.
    
    For a summary of the Independent Institute book, ARMS, POLITICS AND THE 
    ECONOMY: Historical and Contemporary Perspectives, edited by Robert Higgs, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-2.html.
    
    -------------------------------------------------------------
    
    THE ENVIRONMENTAL PROPAGANDA AGENCY
    
    Will the neck-to-neck presidential race help reduce -- or intensify -- 
    pressure for the next president of the United States to score points with 
    statist environmental activists?
    
    Except on a few controversial issues, a strong case can be made that the 
    forty-third President of the United States will wish to portray himself as a 
    close friend of "the environment." President George W. Bush, for example, 
    would face strong pressure to show that he is "bipartisan" in his approach to 
    environmental protection; whereas President Al Gore would likely attempt to 
    win back those who supported Nader and the Greens.
    
    All the more reason, then, to call attention to the failures of the current 
    approach to environmental protection -- especially those emanating from the 
    U.S. Environmental Protection Agency, or, as economist Craig Marxsen terms 
    it, the Environmental Propaganda Agency.
    
    The EPA sometimes employs the language of cost-benefit analysis to illustrate 
    its seemingly tremendous success, but it is known to employ it in a highly 
    misleading manner. The EPA claimed, for example, that its Clean Air Act 
    programs produced, from 1970 to 1990, $22.2 trillion dollars in health 
    benefits at a cost of only $523 billion. But, reports Marxsen in THE 
    INDEPENDENT REVIEW, "[The EPA's] study actually represents a milestone in 
    bureaucratic propaganda. Like junk science in the courtroom, the study 
    seemingly attempts to obtain the largest possible benefit figure rather than 
    to come as close as possible to the truth."
    
    In conclusion, writes Marxsen, "Without the illusory benefit of all the lives 
    saved, the actual benefits of the Clean Air Act were very modest and probably 
    could have been achieved nearly as well with far less sacrifice. The Clean 
    Air Act and its amendments force the EPA to mandate reduction of air 
    pollution to levels that would have no adverse health effects on even the 
    most sensitive person in the population. The EPA relentlessly presses forward 
    in its absurd quest, like a madman setting fire to his house in an insane 
    determination to eliminate the last of the insects infesting it."
    
    For more information, see "The Environmental Propaganda Agency," by Craig S. 
    Marxsen (THE INDEPENDENT REVIEW, Summer 2000), at
    http://www.independent.org/tii/lighthouse/LHLink2-48-3.html.
    
    For analysis of other EPA programs, see the Independent Institute book, 
    CUTTING GREEN TAPE: Toxic Pollutants, Environmental Regulation and the Law, 
    edited by Richard Stroup and Roger Meiners, at 
    http://www.independent.org/tii/lighthouse/LHLink2-48-4.html.
    
    For Robert Formaini's insightful review of Kip Viscusi's important book, 
    RATIONAL RISK POLICY (THE INDEPENDENT REVIEW, Winter 1999), see
    http://www.independent.org/tii/lighthouse/LHLink2-48-5.html.
    
    -------------------------------------------------------------
    
    WILLIAM LLOYD GARRISON, Antislavery Crusader
    
    "I will be as harsh as truth, and as uncompromising as justice. On this 
    subject, I do not wish to think, or speak, or write with moderation.... I 
    will not equivocate -- I will not excuse -- I will not retreat a single inch 
    -- and I will be heard."
         -- William Lloyd Garrison, THE LIBERATOR, January 1, 1831
    
    December 12 marks the 195th anniversary of the birth of William Lloyd 
    Garrison, a leading figure in the American abolitionist movement. As the late 
    historian Henry Mayer explained in his National Book Award-Finalist 
    biography, ALL ON FIRE: William Lloyd Garrison and the Abolition of Slavery:
    
    "William Lloyd Garrison (1805-1879) is an authentic American hero who, with a 
    Biblical prophet's power and a propagandist's skill, forced the nation to 
    confront the most crucial moral issue in its history. For thirty-five years 
    he edited and published a weekly newspaper in Boston, THE LIBERATOR, which 
    remains today a sterling and unrivaled example of personal journalism in the 
    service of civic idealism.
    
    "Although Garrison -- a self-made man with a scanty formal education -- 
    considered himself 'a New England mechanic' and lived outside the precincts 
    of the American intelligentsia, he nonetheless did the hard intellectual work 
    of challenging orthodoxy, questioning public policy, and offering a luminous 
    vision of a society transformed. He inspired two generations of activists -- 
    female and male, black and white -- and together they built a social movement 
    which, like the civil rights movement of our own day, was a collaboration of 
    ordinary people, stirred by injustice and committed to each other, who 
    achieved a social change that conventional wisdom first condemned as wrong 
    and then ridiculed as impossible."
    
    Indeed, without Garrison's inflammatory but compelling writing, speaking and 
    organizing, there might have been no effective American anti-slavery movement 
    at all.
    
    For more on William Lloyd Garrison, read historian Henry Mayer's talk from 
    the Independent Policy Forum, "The Civil War: Liberty and American Leviathan" 
    (with Jeffrey Rogers Hummel), at
    http://www.independent.org/tii/lighthouse/LHLink2-48-6.html, or hear it in 
    RealAudio at http://www.independent.org/tii/lighthouse/LHLink2-48-7.html.
    
    Also see Jeffrey Rogers Hummel's review of Henry Mayer's brilliant biography, 
    ALL ON FIRE: William Lloyd Garrison and the Abolition of Slavery (THE 
    INDEPENDENT REVIEW, Fall 2000), at
    http://www.independent.org/tii/lighthouse/LHLink2-48-8.html.
    
    -------------------------------------------------------------
    
    If you enjoy receiving THE LIGHTHOUSE ... please help us support it.
    
    Your supporting Independent Associate Membership enables us to reach 
    thousands of other people. So, please make a contribution to The Independent 
    Institute. See http://www.independent.org/tii/lighthouse/LHLink2-48-9.html. 
    to donate, or contact Ms. Priscilla Busch by phone at 510-632-1366 x105, fax 
    to 510-568-6040, email to <PBusch@independent.org>, or snail mail to The 
    Independent Institute, 100 Swan Way, Oakland, CA 94621-1428.
    All contributions are tax-deductible.  Thank you!
    
    -------------------------------------------------------------
    
    For previous issues of THE LIGHTHOUSE, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-10.html.
    
    -------------------------------------------------------------
    
    For information on books and other publications from The Independent 
    Institute, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-11.html.
    
    -------------------------------------------------------------
    
    For information on The Independent Institute's Independent Policy Forums, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-12.html.
    
    -------------------------------------------------------------
    
    To subscribe (or unsubscribe) to The Lighthouse, please go to 
    http://www.independent.org/subscribe.html, choose "subscribe" (or 
    "unsubscribe"), enter your e-mail address and select "Go."
    
    Copyright , 2000 The Independent Institute
    100 Swan Way
    Oakland, CA 94621-1428
    (510) 632-1366 phone
    (510) 568-6040 fax
    info@independent.org
    http://www.independent.org
    
    Ken,
    
    Beth suggested that I forward a copy of this article to you.  I've highligh=
    ted some relevant points for your convenience.
    
    Regards,
    Billy
    
    ____________________________________________________________
    
    ACCOUNTING IN CRISIS=20
    One Plus One Makes What?=20
    The accounting profession had a credibility problem before Enron. Now it ha=
    s a crisis.=20
    FORTUNE
    Monday, January 7, 2002=20
    Where were the auditors? People ask that question after every corporate col=
    lapse, and lately they've been asking it with disturbing frequency. At Wast=
    e Management, Sunbeam, Rite Aid, Xerox, and Lucent, major accounting firms =
    either missed or ignored serious problems. The number of public companies t=
    hat have corrected or restated earnings since 1998 has doubled to 233, acco=
    rding to a study by Big Five accounting firm Arthur Andersen. Now, followin=
    g the stunning bankruptcy of Andersen's own client Enron, that question--wh=
    ere were the auditors?--has become a deafening refrain. "I believe that the=
    re is a crisis of confidence in my profession," Andersen CEO Joseph Berardi=
    no told a congressional committee investigating Enron's collapse in mid-Dec=
    ember. "Real change will be required to regain the public trust."=20
    The full story of the Enron debacle--and what Andersen did or did not do in=
     its audit--will take months to emerge. In the meantime, no one disagrees w=
    ith Berardino's diagnosis that there's a crisis in accounting--even if his =
    sudden emphasis on industrywide reform springs from a desire to deflect att=
    ention from Andersen's own culpability. But the kind of "real change" requi=
    red is a matter of substantial debate. The government gave the franchise of=
     auditing public companies' financial statements to the accounting industry=
     after the 1929 stock market crash. In the decades since, the accountants h=
    ave adroitly avoided significant government regulation by arguing that they=
     can police themselves. Now, post-Enron, they're doing it again. The Big Fi=
    ve CEOs issued a rare joint statement outlining how they intend to strength=
    en financial reporting and auditing standards. "Self-regulation is right fo=
    r investors, the profession, and the financial markets," the release conclu=
    des.=20
    But is it? Accounting's main self-regulatory body, the Public Oversight Boa=
    rd, is a monument to the profession's failures. The POB was created in the =
    late 1970s, when Congress held hearings on a string of audit failures at pu=
    blic companies that had--much like the recent rash--shaken confidence in th=
    e major auditing firms. The POB, which has no enforcement power, investigat=
    es alleged audit failures and oversees a triennial review process in which =
    the major accounting firms examine one another's procedures. And yet proble=
    ms persist; arguably, they have grown more acute. "Is accounting self-regul=
    ation working? On the face of it, it is not," says Representative John Ding=
    ell, the powerful Michigan Democrat who has long sparred with the accountin=
    g profession.=20
    In their defense, the auditors note that current accounting methods, many o=
    f which were designed 70 years ago, are difficult to apply to today's compl=
    ex financial transactions. And there is no way, they insist, to prevent sop=
    histicated fraud. The American Institute of Certified Public Accountants (A=
    ICPA), the industry's professional association, points out that accountants=
     examine the books of more than 15,000 public companies every year; they ar=
    e accused of errors in just 0.1% of those audits. But oh, the price of thos=
    e few failures. Lynn Turner, former chief accountant of the Securities and =
    Exchange Commission, estimates that investors have lost more than $100 bill=
    ion because of financial fraud and the accompanying earnings restatements s=
    ince 1995.=20
    Perhaps the most glaring example of self-regulation's deficiency has been a=
    ccountants' unwillingness to deal with conflicts of interest. Over the year=
    s, the major auditing firms have transformed themselves into "professional =
    services" companies that derive an increasing portion of revenues and profi=
    ts from consulting: selling computer systems, advising clients on tax shelt=
    ers, and evaluating their business strategies. In 1999, according to the SE=
    C, half of the Big Five's revenues came from consulting fees, vs. 13% in 19=
    81.=20
    Auditing, meanwhile, has become a commodity. Firms have even been accused o=
    f using it as a loss leader, a way of getting in the door at a company to s=
    ell more-profitable consulting contracts. "Audit work is a marvelous market=
    ing tool," says Lou Lowenstein, a professor emeritus of finance and law at =
    Columbia University. "You are already there doing the audit. You say their =
    internal controls are no good. Well, who are they going to call to fix it?"=
     But this requires a firm to work for the public (auditing) and management =
    (consulting). "You cannot serve them both," says former SEC commissioner Be=
    vis Longstreth.=20
    This conflict may have played a role at Enron. Andersen received $25 millio=
    n in auditing fees from Enron last year. That's money Andersen was paid bot=
    h as Enron's outside auditor, certifying its financial statements, and as i=
    ts internal auditor, making sure Enron had the right systems to keep its bo=
    oks and working to detect fraud and irregularities. This double duty alone =
    raised a serious potential for conflict. Besides $25 million in accounting =
    fees, Andersen was paid $23 million for consulting services. "If you are au=
    diting your own creations, it is very difficult to criticize them," says Ro=
    bert Willens, a Lehman Brothers tax expert who disapproves of the accountin=
    g profession's recent move into selling aggressive tax shelters. Andersen h=
    as not revealed the details of its work on Enron's highly controversial off=
    -balance-sheet transactions, but the accounting firms have never believed c=
    onsulting fees compromise their objectivity. "They have militantly refused =
    to ever acknowledge the possibility of a problem," Longstreth says.
    
    I have been an Enron stockholder for several years  and I am very disappoin=
    ted with the events of the last two weeks.  I find  the allegations of acco=
    unting irregularities incredible.  I would like to  know how closely the bo=
    ard had been monitoring the activities of the CFO and  whether it approved =
    the partnerships that have led to the SEC investigation and  the dramatic d=
    ecline in the company's stock price.  I would suggest that  the compensatio=
    n of senior management may be too heavily weighted towards  bonuses (giving=
     some the incentive to manipulate the numbers to increase their  bonuses) a=
    nd not heavily weighted enough towards stock options.  I would  hope in the=
     future the goals of the board and senior management will be aligned  with =
    stockholders (increasing shareholder value).  While I know it is not  reaso=
    n to have expected the stock price to stay in the high 80s, I consider the =
     drop in the last two weeks (apparently as the result of fraud) to be  inex=
    cusable.  I believe the board owes all stockholders an explanation that  wi=
    ll finally clear the air about this mess (maybe the markets will trust Enro=
    n  again if it replaces evasion with candor).  Please help return credibili=
    ty  and ethics to this company, which apparently are badly needed.
    =20
    Wayne Heilman
    Owner of 1,800 Enron shares (in my children's  college fund)
    
    
    I have found the events of the last two months to  be among the most unsett=
    ling in my 15 years of owning stock in public  companies.  I have sold my 1=
    ,800 shares of Enron Corp. because I no longer  have faith in the board to =
    be a good watchdog for the best interests  of the shareholders.  I just don=
    't understand how senior managers can  manipulate the financial results of =
    the company for the past five years and only  recently be discovered.  I ex=
    pect the board to act as more than a lap-dog  for senior management.  I am =
    encouraged you turned down the chance to  accept a $60 million payday for t=
    his merger that salvages what little  shareholder value remains in Enron.  =
    I would expect the board to seek  repayment of any and all bonuses paid to =
    senior managers based on these  inaccurate financial results, including the=
     estimated $30 million paid to the  CFO, who allegedly is the architect of =
    this scheme to defraud  stockholders.  Had I known the company was apparent=
    ly built on smoke and  mirrors, I would have sold my stock long ago and not=
     be forced  to watch my  children's college fund be pocketed by executives =
    who have little regard for the  stockholders they were hired to serve.  I g=
    uess the only lesson I can take  from this is that no company, no matter ho=
    w large or prestigious is safe from  dishonest management.  I do not includ=
    e you in this group, but I do believe  you and the board bear some blame fo=
    r allowing this happen for the past five  years.
    =20
    Wayne Heilman
    Former Enron Stockholder
    
    Ken,
    
    Thanks for having the call yesterday.  I am a believer in Enron and we are
    buying your debt.
    
    Here's short feedback on the call.  I give the call a B-/C+ grade.  If you
    want a good example of a company call listen to the tougher calls So Cal Ed
    had for investors.  They talked with investors twice weekly for months.
    There is an honestness and openness about the calls that worked to keep
    investors informed.
    
    The angry exchange with the short seller was a blunder.  No matter what the
    question, no matter how stupid or obnoxious, the Enron answer should always
    be empathetic and pointedly factual.  Yes, you will have to open the kimono
    more than Enron ever has to restore confidence.
    
    ENE has become more a financial institution, and less an energy company
    with hard assets.  You are in a confidence game pure and simple.  Any
    attempt at keeping things close to the vest will backfire and exacerbate
    the current crisis.  If you want to see disclosure in financials, look at
    Citi's...
    
    ENE is having an encounter session with the entire investment community.
    This is not a time to be aggressive and smart.  It is a time to be open,
    honest and self-effacing.
    
    Third party confirmation will go a long way in restoring confidence which
    means 1) Moody's maintains their rating or limits the change to a one-notch
    downgrade (bond rating) with stable outlook restored (the Moody's analyst
    is pissed you surprised with the write-off), 2) the SEC gives ENE a fairly
    clean report (no self-dealing found) on their transactions with private
    equity partnerships, 3) no more large surprise write-offs, 4) outside
    auditors continue with unqualified audit reports, 5) operating earnings
    targets are met for this year and next, and 6)fuller disclosure is made in
    quarterly releases.
    
    Of course, if any fraud or malfeasance is found, you guys and ENE are
    history.
    
    I have my ass on the line because I believe in ENE.  We have made
    investments.  ENE is in the shitter because of ENE's past communication and
    behavior mistakes.  Be humble, be smart, and change.
    
    Good Luck!
    
    Jeff
    
    P.S.  You state there is a Chinese wall between LJM and ENE.  Why in the
    world then would you have the same guy, Fastow, play key controlling roles
    in both organizations?  If you want to pay him, pay him.  Keep it simple.
    
    I am glad Skilling is gone.  I wouldn't touch your company's debt while he
    was there.  I hated him and his attitude?.and my past exposure to him was
    extremely limited.
    
    
    
    
    
    This transmission may contain information that is privileged, confidential and/or exempt from disclosure under applicable law. If you are not the intended recipient, you are hereby notified that any disclosure, copying, distribution, or use of the information contained herein (including any reliance thereon) is STRICTLY PROHIBITED. If you received this transmission in error, please immediately contact the sender and destroy the material in its entirety, whether in electronic or hard copy format. Thank you.
    For the first time in 22 years of service for Enron I'm ashamed to admit th=
    at I work for Enron!  I have lost all respect for Enron Senior Management a=
    nd agree with the Financial Analyst when they say that Enron Senior Managem=
    ent can not be trusted.  Ethics and Morals are either something everyone el=
    se needs to have except Senior Management or somewhere along the way Senior=
     Management started believing the end justifies the means.  The communicati=
    on the Employee's have received over the last few years about values such a=
    s Respect, Integrity, Communication and excellence must be propaganda inten=
    ded to get Employee's to believe Senior Management really supported these v=
    alues so that no one would really notice that their actions represented som=
    ething just the opposite.
    
    I can not believe that Senior Management lacks the understanding of human n=
    ature to totally ignore the fact that if someone does not trust what you ha=
    ve said in the past you should not try and conceal pertinent information in=
     the future.  Then to allow an Enron Spokesperson to speak for Enron with t=
    he statement "It's just a balance-sheet issue"  implies to me that this ind=
    ividual does not know what a Balance Sheet or Income Statement is and certa=
    inly should not be speaking to the public.  If you are going to play the ga=
    me of lying, cheating and stealing at least be intelligent enough to presen=
    t a plausible story.  To use phrases such as it was a hedge against fluctua=
    ting values in some of Enron's broadband telecommunications and other techn=
    ology investments is a complete insult to my intelligence and to the defini=
    tion of the word "HEDGE".  A hedge by definition implies that the upside an=
    d downside exposure has been limited.  It frightens me that we are out aski=
    ng customers to let Enron help them "HEDGE" their risk when I'm not sure En=
    ron's Senior Management understands what a hedge means?
    
    I'm also confused as to the personal financial involvement of Andy Fastow i=
    n the investment vehicle that generated this write down.  I must be confuse=
    d in that as a Trader Enron has asked me to sign documents declaring that I=
     will not have any personal financial involvement in anything involving the=
     Natural Gas Business.  This must be an example of as my father used to say=
     " do as I say not as I do"? I guess we need to have audits performed of Se=
    nior Managements financial interest to insure their actions are as good as =
    their word?
    
    The fact that Senior Management and the ENE Board of Directors knew these t=
    ransactions were being used to manipulate earnings and the stock price and =
    took advantage of that knowledge to sell their ENE stock options in my opin=
    ion is "CRIMINAL".  It provides me no comfort to know that the biggest perp=
    etrators of this fraud have left Enron in recent months.  These individuals=
     have stolen billions of dollars from ENE stockholders and Employees and ne=
    ed to pay the price for such fraudulent activity.  They are set for life, h=
    aving all the money they could ever need, while Employees and Stockholders =
    have lost their life savings.  You have completely failed at the job you we=
    re hired to perform.  If this type of activity would have occurred farther =
    down the organization no one would hesitate to fire the individuals involve=
    d and to institute criminal charges.  =20
    
    I for one can not wait until the All Employee meeting next week.  I do not =
    think anyone wants to hear about third quarter results because how could we=
     trust what is said anyway.  Instead I feel you owe the ENE Employee's a th=
    orough explanation of how you failed to perform your responsibilities, what=
     actions are going to be taken and most of all apologize for the job perfor=
    mance to date.=20
    
    
    
    
    FYI, the only people who knew who we invited to the meeting (but did not attend) were Dick Riordan and Kevin Sharer...
    
    
    Saturday, May 26, 2001 (SF Chronicle)
    Enron's secret bid to save deregulation/PRIVATE MEETING: Chairman pitches his plan to prominent Californians
    Christian Berthelsen, Scott Winokur, Chronicle Staff Writers
    
    
       Energy executive Kenneth Lay, head of powerful Enron Corp., quietly
    courted Arnold Schwarzenegger, Richard Riordan, Michael Milken and other
    luminaries this week in Beverly Hills to drum up support for his solution
    to California's energy crisis.
       His prescription called for more rate increases, an end to state and
    federal investigations and less rather than more regulation.
       Lay, a close friend of President Bush and one of his largest campaign
    contributors, hosted a private 90-minute meeting in a conference room at
    the Peninsula Hotel in Beverly Hills on Thursday.
       Among the participants were Milken, the former head of the Drexel Burnham
    Lambert investment banking firm who pleaded guilty to fraud charges in
    1990 and who now runs a think tank based in Santa Monica; movie star
    Schwarzenegger;
       and Riordan, the mayor of Los Angeles. Schwarzenegger and Riordan have
    been courted recently as GOP gubernatorial candidates.
       One participant, who agreed to speak on the condition he not be
    identified, said the meeting appeared to be geared toward getting
    participants to support Lay's vision and then champion it to officials who
    are trying to solve the state's energy mess.
       PLAN TO RESCUE DEREGULATION
       The source said the timing and tone of the meeting suggested Lay is
    concerned that California will abandon its disastrous experiment with
    power markets by either re-regulating the system or creating a government
    authority to provide electricity. Gov. Gray Davis signed legislation last
    week to create and fund a state power authority that would build, buy and
    run power plants in California.
       "They're trying to rescue deregulation," the source said of Enron
    executives. "They think the whole state power authority is a bad idea."
       At the meeting, Enron representatives circulated a four-page position
    paper titled "Comprehensive Solution for California," which was obtained
    by The Chronicle. It said ratepayers should bear responsibility for the
    billions in debt incurred by the state's public utilities and that
    investigations of power price manipulation and political rhetoric are
    making matters worse.
       The paper made no mention of the possibility that much of the runaway
    electricity costs in California is due to market manipulation by power
    generators and traders -- a possibility given credibility in studies by
    regulators and economists.
       One of the talking points read: "Get deregulation right this time --
    California needs a real electricity market, not government takeovers."
    Another point suggested giving consumers monetary rebates for conserving
    electricity.
       INVOLVED IN EARLY DAYS
       Lay has been an aggressive champion of deregulated electricity markets and
    was an early advocate in persuading California to begin its experiment
    with a competitive power market system.
       Lay has created a new kind of company in the process, one that essentially
    produces nothing but makes money as a middle-man, buying electricity from
    generators and selling it to consumers. During the first quarter of this
    year, Enron's revenues increased 281 percent to $50.1 billion.
       Asked about the purpose of the meeting, Karen Denne, a spokeswoman for
    Enron, said she would "look into that" and then did not return repeated
    telephone calls seeking comment. One participant said Denne was present at
    the meeting.
       D.C. CONNECTIONS
       Meanwhile, Lay's power in Washington is reported to have reached
    unprecedented heights. According to a story in yesterday's New York Times,
    Lay supplied the Bush administration with a list of candidates for jobs
    regulating the power industry and even interviewed one of them. The story
    also said Lay essentially threatened to seek the removal of the chairman
    of the Federal Energy Regulatory Commission, Curt Hebert, if he does not
    support Lay's desire to further deregulate the nation's electricity
    system. Lay denied the allegation.
       Also in attendance at this week's meeting were Bruce Karatz, chief
    executive of home builder Kaufman & Broad; Ray Irani, chief executive of
    Occidental Petroleum; and Kevin Sharer, chief executive of biotech giant
    Amgen.
       Among those who were invited but did not attend were former Los Angeles
    Lakers star Earvin "Magic" Johnson; supermarket magnate and Bill Clinton
    supporter Ron Burkle; and Dennis Tito, recently returned from the world's
    first civilian space trip.
       Milken, through a spokesman, confirmed that he attended the meeting, but
    declined to be interviewed. Schwarzenegger could not be reached for
    comment through a publicist, and Sharer did not return a call yesterday
    afternoon.
       A spokesman for Riordan, Peter Hidalgo, said the Los Angeles mayor
    attended,
       but was "not intending to formulate any kind of policy position on this
    issue.
       His intent is to listen to all sides."
       Attached to the Enron handout was a two-page open letter, addressed to
    Davis and the state Legislature, apparently prepared for those who support
    Lay's position and would be willing to sign their names to it. The source
    who participated in the meeting said those assembled appeared noncommittal
    and asked a number of questions of Lay, but did not agree to champion his
    agenda.
       E-mail the writers at cberthelsen@sfchronicle.com and Scott Winokur at
    swinokur@sfchronicle.com.
    ----------------------------------------------------------------------
    Copyright 2001 SF Chronicle
    
    Ken,
    
    I wanted to update you on the reaction to your most recent letter to the In=
    dian Prime Minister. You recall that we recommended you write again to the =
    Prime Minister and express disappointment at the lack of progress since you=
    r July trip and your August letter containing our offer to sell our equity =
    at costs. I believe writing this letter was the proper response but only ti=
    me will tell what reaction (if any) finally comes from the Indian governmen=
    t.
    
    I think you should know about the press coverage of the letter, just in cas=
    e the letter were to come up in any conversation you might have with someon=
    e from the US government. I am not anticipating this at all, but I would no=
    t want you to be unprepared about the press coverage. The US embassy over h=
    ere is very much up to speed.  I met again with Ambassador Blackwill on Tue=
    sday of this week and gave him a general update on the current status. His =
    view is that it is very difficult in the post-Sept. 11 environment to get a=
    nyone in Delhi or Washington to pay attention to economic matters such as D=
    abhol, but he is trying as best he can. He is meeting with Brajesh Mishra (=
    Prime Minister's principal secretary) on Saturday, who will have just retur=
    ned from Washington meetings with Ms. Rice, among others, principally to di=
    scuss anti-terrorist matters. John Hardy of our DC office briefed the State=
     Department last week in advance of these meetings, and we are awaiting to =
    hear any discussion or outcome regarding Dabhol.
    
    Regarding press coverage of the letter, as we anticipated, the entire lette=
    r was leaked to the press. Several articles have appeared that describe the=
     "harshly worded" letter and many articles have quoted verbatim sections fr=
    om the letter. The Wall Street Journal article from last week presented the=
     letter fairly, and I think it came out rather sympathetic to our position.=
     The Indian papers have been neutral to negative. For example, an editorial=
     (reprinted below) was in Wednesday's Financial Express, a business daily i=
    n India (but not as widely circulated or respected as The Economic Times or=
     The Business Standard). Note the editorial below says your letter states "=
    that any government found to have expropriated the property of US firms aut=
    omatically faces sanctions from that country." Your letter did not state th=
    at, but this is a holdover from the earlier Financial Times interview.
    
    Many people I have met with in the Indian government have made references t=
    o the letter, saying they do not think it was appropriate. I respond by say=
    ing that given our 9 year history of nothing but frustrations, and the lack=
     of progress over the past 2 months, we do think it is appropriate and it r=
    eflects our current views regarding our experience in India. Generally, whe=
    n they mention it time and again, it probably means the letter is having at=
     least some of its intended effect.=20
    
    I'll keep you, Stan and Jim updated as we hear more from the Indian governm=
    ent.
    
    Wade
    
    
    
    
    
    
    
    
    Nikita Varma
    09/26/2001 12:03 PM
    To:=09Nikita Varma/ENRON_DEVELOPMENT@ENRON_DEVELOPMENT
    cc:=09 (bcc: Wade Cline/ENRON_DEVELOPMENT)
    
    Subject:=09From The Enron India Newsdesk - September 26th Newsclips
    ---------------------------------------------------------------------------=
    ----------------------------------------------------------------------
    THE FINANCIAL EXPRESS, Wednesday, September 26, 2001
    
    
    Lay off, Mr Lay (Editorial)
    Enron tries terror tactics
    
    Enron Corporation's CEO Kenneth Lay has reportedly written a letter to Prim=
    e Minister Atal Bihari Vajpayee warning him of adverse consequences to the =
    Indian economy if the Dabhol Power Company imbroglio is not resolved quickl=
    y. He says that if Enron receives anything less than its full investment in=
     DPC, it would amount to an act of expropriation. He further points out tha=
    t any government found to have expropriated the property of US firms automa=
    tically faces sanctions from that country. Reportedly, the letter was writt=
    en three days after the attack on the World Trade Centre. So Mr Lay probabl=
    y ought to be forgiven for reading more than he should into the "dead or al=
    ive" rhetoric of his pal, George W Bush, whose election campaign Mr Lay gen=
    erously funded. Saner counsel has since prevailed even at the White House; =
    and far from imposing fresh sanctions, the US government is busy buying sup=
    port even from countries such as Pakistan by lifting sanctions and offering=
     a hefty aid package. Enron has never hesitated to use the considerable eco=
    nomic and political clout of the US government in pushing the Dabhol projec=
    t at various times. Post September 11, the US government - which is in fact=
     'rewarding' Pakistan despite its clearly identified role in funding terror=
    ist groups - ought not to be terrorising India on Enron. Hence, it is entir=
    ely appropriate for the Prime Minister to borrow that little Americanism, r=
    ecently made popular by Pervez Musharraf, and ask Kenneth Lay to lay off!
    
    Interestingly, Mr Lay's letter apparently makes no mention of several impor=
    tant facts that will dictate how much India may have to pay Enron. These in=
    clude the findings of the Madhav Godbole committee report which expose subs=
    tantial overcharging by DPC on several counts and reveals excess capacity c=
    reated for port handling and regassification. Moreover, DPC's inability to =
    ramp up the power plant to full contracted capacity within the specified ti=
    me period has enormous penal provisions attached to it. This serious techni=
    cal deficiency apart - which alone is grounds enough for repudiating the DP=
    C contract - the government also has a good case to charge the US company w=
    ith fraud and misrepresentation. The Maharashtra government 's judicial inq=
    uiry only makes India's case stronger. Let us not allow ourselves to be pus=
    hed around by Enron's threats and political connections.
    
    
    Hello again.  Out of all of the stuff in the media, this is the most
    informative of all we have seen.
    Very well said.  We wish all the best for those effected and Houston as we
    move through this.
    When the dust clears, Enron will be remembered as a pioneer and great
    corporate citizen.
    
    Best, Mark
    
    W. Mark Shirley, CPA
    281.374.6700x230
    www.kainon.com
    
    
    Enron Is History, Says History
    By HOLMAN W. JENKINS JR.
    
    November 28, 2001
    Business World
    
    Back in the mid-1980s, a pipeline executive called Ken Lay was fishing
    around for a name for his company, produced by a merger of Houston Natural
    Gas and Omaha-based InterNorth. He consulted with consultants, politicked
    with politicians, and came up with a moniker. The company would be called
    "Enteron."
    
    Three weeks later, fed up with the wisecracks from a press that had looked
    up the dictionary definition of "enteron" (n. the intestine), he changed the
    company's name again. Henceforth it would be known as Enron.
    
    A columnist less devoted to high standards of decorum might be tempted to
    extend the metaphor of the company's misbegotten name. In recent weeks,
    after all, we've seen Enron's stock collapse over indigestible accounting
    and the emergence of dealings between the company and its senior officers
    that exude an odor of genuine malfeasance. The evidence is far from clear,
    but for the sake of Mr. Lay's reputation one hopes these missteps will prove
    one more case of a company fooling itself rather than setting out
    deliberately to defraud the markets.
    
    Enron grew to be much more than a pipeline hauler of natural gas, becoming
    the pre-eminent trader and marketer of all kinds of energy contracts and a
    vocal proponent of deregulation. Now, all but overnight, it's kaput, just
    waiting to find out if its fate will be bankruptcy or absorption by an
    erstwhile rival.
    
    We cannot help be put in mind of another commodity wunderkind in the 1970s,
    Phibro (short for Philipp Brothers). Hard to believe, but Phibro was once a
    name that made grown men quiver on Wall Street. Fattened by trading profits
    from the great commodity inflation of the 1970s, which some mistook for a
    permanent new age of scarcity, it scooped up the Street's oldest
    partnership, Salomon Brothers, tucking it into its back pocket and renaming
    the combined firm Phibro-Salomon. Here was a powerhouse of unlimited
    potential, investors told themselves.
    
    Flash ahead to California's electricity meltdown earlier this year. Enron
    saw its revenues quadruple partly as a result of the inflated prices being
    quoted in the California market. Many foresaw a new scarcity megatrend, but
    there was no true energy shortage. Posted prices on the California power
    exchange may have skyrocketed, but the effective price was zero dollars and
    zero cents, because the utilities had no cash to pay and politicians were
    thumbing their noses at piles of IOUs.
    
    When prices are zero, suppliers take a hike -- that's what economics
    teaches. But once the state government started pumping its own cash into the
    market, the phony posted prices plummeted and supplies became plentiful
    again. Now California is swimming in power and nobody talks about an "energy
    crisis" anymore.
    
    You can date the loss of investor confidence in Enron almost exactly to the
    moment when the California fiasco began to repair itself. Fortune Magazine
    put the inaugural nail in Enron's coffin in March, noting that the company's
    growing dependence on trading had turned it into an oil-patch version of
    Goldman Sachs. Goldman's stock sells at a price-earnings multiple of 17,
    reflecting investors' well-founded distrust of trading earnings to be
    reproduced reliably year after year. So why, the magazine asked, was Enron
    awarded a multiple of 60-plus? Mmm...
    
    Enron did yeoman service as a champion of deregulation. Boss Ken Lay, a
    believer in technology and the power of markets, was a true visionary, to
    the point of annoying people who didn't care for his air of being a man on
    the right side of history. The moldering pipeline he took over would
    certainly have been an also-ran if he had not thrown Enron headlong into
    trading and marketing.
    
    But deregulation doesn't confer permanent advantage on anybody. A
    deregulated environment favors constant innovation and a continual upsetting
    of plans and strategies.
    
    Add the fact that, despite the California bubble, there is no reason to
    believe energy prices won't continue their long-term relative decline as
    technology advances more quickly than the depletion of conventional
    resources. Add also the likelihood that information technology will continue
    to lower the barriers to entry to Enron's trading business, which means more
    competition and shrinking margins. Enron begins to look a lot like Phibro.
    
    The great commodity-trading machine was already running down by 1981, when
    it bought Salomon and Wall Street was swooning. Inflation was being quelled
    by Paul Volcker. The products that Phibro traders bought and sold were
    increasingly being traded transparently on electronic exchanges. "Four or
    five years ago, they used to be able to take other companies to the
    cleaners, because they knew where the market was and others didn't," a
    trader explained. "With everyone knowing, within a few cents, where the
    price of any product was, Phibro's ability to make a profit off its superior
    knowledge disappeared."
    
    Not only is this true of Enron, but of its would-be bottom fisher, Dynegy,
    run by Mr. Lay's Houston homeboy, Chuck Watson. Dynegy's proposed takeover
    of its former nemesis was hanging by a negotiation yesterday.
    
    While Enron in recent years was selling hard assets and concentrating on
    electronic market-making, Mr. Watson was doing the opposite. His big play in
    the Enron deal is to get his hands on the original HNG-InterNorth pipeline,
    now known as Northern Natural Gas. By having both feet planted in the real
    business, he claims his firm will be able to make a profitable sideline out
    of trading despite growing competition and transparency.
    
    We'll see. Dynegy and Enron were born at the same time, and of the same
    motive. Dynegy was originally created by six pipeline companies, a
    Washington law firm and Morgan Stanley to take advantage of new
    opportunities in deregulated natural gas. But the gnats are already
    circling.
    
    Gas producers who have claimed for years that the duo control too much of
    their fate now insist they shouldn't be allowed to merge. Don't listen to
    the fussbudgets. If this was a business in need of trustbusting, Enron
    wouldn't have been resorting to funny accounting to make its earnings. As
    Merrill Lynch's Donato Eassey has pointed out, wholesale margins have been
    steadily thinning as trading becomes more transparent and competitive.
    
    Wishful accounting has time and again proved the last refuge of companies
    whose dearly held "visions" were not panning out. Enron prided itself on
    being realistic and adaptive, but it failed to see that its own beliefs
    about the world needed overhauling.
    
    ----------------------------------------------------------------------------
    ----------------------------------------------------------------------------
    --------
    "It ain't what you don't know that gets you in trouble; it's what you know
    for sure that ain't so."       -Mark Twain
    
    K   A   I   N   O   N      G   R   O   U   P
    Consulting  +  Staffing  +  Recruiting
    Dr. Lay,
    What a joke ... Do realize how lives you and your
    board of directors have ruined >>>> all because of
    greed and power .... I looked up to for a while
    building a great company nice place to work  but then
    greed got in your way ... Lets hear again why Skilling
    quit ?? because of family ... BS. Over the past ten
    years i have worked at
    Enron as a computer contractor in payroll and i saw
    the outlandish executive bonuses and money to the
    islands.  I hope you get justly fined and sent to
    prison for the fraud you have committed. The few
    shares
    of enron i have left are not worth the paper they are
    printed on.....i also saw all the stock options you
    and the board have excersied over thats few years 100s
    of million dollars what a shame ...
      When you opted to replace the oracle Payroll with
    SAP I new you were loosing your mind ....
    
    all the great people that made you now have to find a
    job replace their retirement and hope for the best.
    
    I hope we never meet ...
    
    craig cannon a stock holder
    
    __________________________________________________
    Do You Yahoo!?
    Yahoo! GeoCities - quick and easy web site hosting, just $8.95/month.
    http://geocities.yahoo.com/ps/info1
    Dear Friends and Family,
    We are down to the wire and the race couldn't be
    closer. Every vote counts, even those of you in Texas.
    Please, remember to vote and go early, polls open at
    7am across the country and close at 7pm but don't put
    it off until 7pm as there could be lines and the
    supervisors are not required to keep the polls open
    after 7pm for those in line.
    Remind your friends and family to vote and if someone
    needs transportation to the polls, give them a ride or
    call your local Victory 2000 office (Republicans) or
    your local Democratic campaign office, they will have
    people available to transport people to the polls.
    Keep an eye out while you are at the polls for
    electoral fraud and report any suspicious activity
    including inappropriate campaigning to the poll
    watchers at the polls. The Democrats and the
    Republicans will have representatives at most of the
    polls, let them know if there is suspicious activity.
    Finally, if you have time, call your local offices and
    offer your time to make phone calls to encourage
    people to get out and vote, pass out leaflets, or to
    be available to help prevent of voter fraud.
    
    Finally, enjoy the evening and watch the results come
    in. There are election watch parties everywhere!!!
    
    Just one more day (and no more mass political e-mail's
    from me and everyone else)!
    Take care and VOTE!
    Liz
    
    __________________________________________________
    Do You Yahoo!?
    Thousands of Stores.  Millions of Products.  All in one Place.
    http://shopping.yahoo.com/
    Pleased to send you the November report. Obviously the market is weakening
    in North America which is making the quarter challenging but the underlying
    momentum of the company continues to improve as the report illustrates.
    Look forward to seeing you at the Board meeting.
    
    Regards,
    Peter
    
    
    
    > [Compaq Confidential - Internal Use Only]
    >
    > To:  Global Sales & Services Team
    >
    > Before I report on the great wins and other news this month, I want to
    > express a personal note about the organizational announcement earlier this
    > month.  I'm excited about the changes for all the reasons already
    > communicated - in particular strengthening the integration of our upstream
    > and downstream operations.  I'm also excited about Bo McBee and his
    > worldwide team in Corporate Quality and Customer Satisfaction officially
    > joining our organization.  He and his team are doing a great job, and
    > together we will further our efforts to become the leader throughout the
    > world in satisfying our customers.
    >
    > Most of all, I am extremely pleased and encouraged because I believe these
    > changes confirm the great work you have accomplished this year.  We've
    > already reported a number of major wins as a result of the joint efforts
    > by our Sales and Services teams.  There is an air of excitement and
    > anticipation about Compaq's momentum - I see it in the emails from many of
    > you and as I meet with our teams and customers around the world.  You're a
    > remarkable team and, as Michael puts it, let's keep the pedal to the metal
    > and keep the momentum strong as we work to successfully close 2000!
    >
    > Speaking of my travels...
    > This month I visited Johannesburg, South Africa, Dubai within the United
    > Arab Emirates, and Saudi Arabia.  All of these countries are part of
    > EMEA's Business Development Group (BDG), which is responsible for
    > developing Compaq business in 98 countries. The group is focused on both
    > developed and emerging markets in Eastern Europe, the Middle East and
    > Africa. Over the past six years, BDG has grown its revenue more than
    > 10-fold.
    >
    > In South Africa I visited Vodacom, which with 4 million subscribers, is
    > Africa's largest mobile phone network operator.  The company has just
    > upgraded its billing systems to handle further expansion, and to date is
    > one of the world's largest Wildfire installations with some 21 AlphaServer
    > GS systems.  I also spent time with the management of Mobile Telephone
    > Networks (MTN), South Africa's #2 cellphone operator and another big
    > Wildfire customer.  In fact, we just got word that they've placed a $10M
    > order for four GS320 AlphaServer systems and storage.
    >
    > One of my more interesting activities while there was learning more about
    > Ikageng, a Compaq-led initiative to bring the benefits of the information
    > age to the rural communities of Africa.  Ikageng brings together the
    > provision of safe drinking water, affordable healthcare, distance
    > learning, improved subsistence farming techniques and Internet access.
    > All of this is co-funded by a community bank, together with Compaq,
    > Johnnic, a South African media and information group, and the active
    > participation of the World Bank.  A real example of Inspiration Technology
    > at work!
    >
    > My visit to the United Arab Emirates included a dinner with our top 30
    > customers from across the region, a VIP lunch with our top partners, as
    > well as meetings with employees in the region.  I also attended Gitex, the
    > region's largest IT exhibition, and met with press at that event to convey
    > Compaq's commitment to the UAE.  I was also privileged to have a personal
    > meeting with His Highness Sheik Mohamad bin Rashid al Maktoum, Crown
    > Prince of Dubai and the Minister of Defense.  These meetings were around
    > the official opening of Dubai Internet City, an area of Dubai dedicated to
    > making the city the "Silicon Valley"  of the Middle East.
    >
    > I spent a very interesting day at Aramco in Saudi Arabia, our largest
    > account in the UAE. We are the ProLiant standard in this very large energy
    > company and we have a great opportnuity to build a strong partnership
    > across many additional solutions including high performance technical
    > computing, ZLE applications and enterprise storage, in addition to
    > recapturing client business from the competition
    >
    > Some of our largest wins this month
    > * Tokyo Stock Exchange - We are replacing Hitachi at the world's third
    > largest stock exchange, with a $60-80M order for Himalaya systems. This
    > contract should bring in an additional $20-30M in Professional Services.
    > *      Eli Lilly - Signed the first leg of a three-year global agreement
    > valued at $100M, securing Compaq as the sole supplier for Intel-based
    > products, forcing Dell off the customer's standards list and opening the
    > door for StorageWorks products, Global Services and high-performance
    > servers.
    > *      Winstar - Four-year, $100M contract as the exclusive provider of
    > Windows NT and storage products, including $10M in AlphaServer systems
    > running Tru64 UNIX.
    > *      Mead Corp. - Beat IBM, HP and Dell for a five-year, $50M contract
    > for ProLiant servers, storage, desktops, portables and services.
    > * France Telecom - $30M contract for a global agreement (includes all
    > subsidiaries) for a complete line of AlphaServer systems, including DS, ES
    > and GS series as well as ProLiant servers.
    > * General Motors - Selected as the global Intel-based server standard
    > for new application deployment at GM manufacturing facilities. The
    > anticipated global revenue is $30M over three years.
    > * Electronic Classroom of Tomorrow - $25M for ProLiant 8500 servers,
    > StorageWorks products and legacy-free iPAQ desktops.
    > * FleetBoston Financial - Beat IBM, Dell and HP for a $40M desktops
    > contract
    > * Airgroup (Switzerland) - Beat IBM and NetVista for a  $21M contract
    > for 20,000 iPAQ desktops.
    > * DLI (Korea) - $22M for Professional Services.
    > * AltaVista - Shut out IBM and HP by putting into place a $25M fair
    > market value lease for ProLiant- and Alpha-based servers, increasing the
    > AltaVista lease line to $75M.
    > * ASP Host Centric - One of the eight North America-certified Oracle
    > Authorized Application Providers (OAAP), the firm will standardize its
    > UNIX environment on AlphaServers, replacing Sun systems. This project
    > could generate more than $20M for us over the next 36 months.
    > * Interfusion - Three-year, $20M contract for a Tru64 UNIX-based
    > solution.
    > * Westcoast Energy- Topped Dell for a desktop and portables contract
    > valued between $15-20M.
    > * General Electric - Five-year, $15.4M contract for worldwide Lotus
    > Domino rollout and expansion of Exchange rollout, including NT Server
    > management outsourcing.
    > http://newscpq1.inline.cpqcorp.net/article.cfm?storyid=1146
    > * Moebel Pfister - $15.7M outsourcing contract.
    > * TriRiga - Beat Dell, EMC and Sun for a two-year, $15M contract for
    > storage, Professional Workstations, desktops, portables and services.
    >
    > EMEA to open Wireless Competence Centre in Stockholm
    > Press, customers and partners have been invited to help officially open
    > the Compaq Wireless Competence Centre in Stockholm, Sweden, on November
    > 27.  The centre is the company's first facility to fully display our
    > unique end-to-end capabilities of  solutions, services and products in the
    > mobile Internet and wireless space.  The hands-on centre showcases today's
    > wireless solutions within four environments - car, home, office and public
    > access areas.  Technologies featured include GSM, GPRS, future 3G
    > standards, WLAN and Bluetooth.  Compaq's mobile partners such as Nokia,
    > Oracle, Cisco, Microsoft, Siebel and Ericsson also plan to participate in
    > the opening.  The centre is already hosting customer visits and will
    > engage with thousands of customer and partners over the coming year
    > through a mix of seminars, tours and customized workshops.  For more info,
    > see  http://inline-se.soo.cpqcorp.net/wireless/
    >
    > Planning for Innovate Forum 2001 under way
    > Compaq's premier event for its global and large account customers -
    > Innovate Forum 2001 - is set for May 23-24 at the George R. Brown
    > Convention Center in Houston.  The hand-picked guest list will include
    > some 4,000-5,000 senior-level technical and business executives, including
    > our key channel partners, press, industry and financial analysts, and
    > Compaq's key alliance partners.  The program will feature keynote
    > speeches, plenary sessions, special interest seminars, a solutions
    > pavilion, and social events.  For more information, see the Innovate site
    > on Inline:  http://inline.compaq.com/na/innovate/
    >
    > Cross Border Office files first lawsuit
    > The Cross Border Office has been created to prevent unauthorized movement
    > of Compaq products by dealers and gray market brokers in order to protect
    > profit margins and ultimately, customer satisfaction.  The Cross Border
    > team provides gray market awareness training to all sales personnel, mail
    > and phone hotline access to report gray market activity, works jointly
    > with regional sales, services, business unit and channel teams to create
    > policy and procedures to reduce gray market activity and, working with the
    > Law department, to bring legal action against gray market brokers if
    > warranted.
    >
    > As a result of these efforts, Compaq has filed its first lawsuit against
    > two Canadian technology consulting firms for breach of contract and fraud.
    > The suit, which seeks compensatory damages of more than $17 million,
    > claims the consulting firms fraudulently represented to Compaq that they
    > had a contract with the U.S. Department of Transportation's Federal
    > Aviation Administration to supply a large number of computers and related
    > equipment to U.S. airports. This lawsuit hit many national publications
    > and sends a message to the worldwide gray market community that Compaq
    > will take actions to protect its authorized resellers, product quality and
    > our customers.
    >
    > For further information on the Cross Border Office, gray market red flags
    > and to view the web-based training video, see
    > http://inline.compaq.com/wwsm/crossborder/index.asp
    >
    > Key Channel Partner programs rolling out
    > Early this year the Tigerbite project was established to redefine and
    > simplify Compaq's model with our channel partners.  A key element of the
    > model is worldwide programs that provide profitable growth opportunities
    > for Compaq and its partners.  Two such programs - Internet List Pricing
    > (ILP) and the Compaq Agent Program - are currently being implemented by
    > the regions.
    >
    > Worldwide implementation of ILP is a top priority for the company.
    > Creating and publishing (where needed) competitive List Prices is
    > absolutely essential to establishing a more consistent, worldwide pricing
    > model for both our customers and partners. By the end of this month ILP
    > will have been implemented in North America, Latin America, Japan and
    > Greater China, with pilot programs in Singapore and Malaysia.  EMEA and
    > the remaining Asia Pacific countries are expected to complete the rollout
    > by January 1, 2001.
    >
    > The Compaq Agent Program, which allows partners to earn commissions when
    > they refer customers to purchase products/services directly from us,
    > currently has been implemented in the U.S., Latin America (14 countries)
    > and CKK.  This month, APD is implementing pilot programs in Singapore and
    > Malaysia, and plans to roll out the program in seven additional countries
    > in the first quarter.  EMEA held an Agent Program Summit this month with
    > 10 countries to assess and develop their 2001 rollout plans which include
    > adding Enterprise-class products to their program next year.
    >
    > News from the Compaq Alliances team
    > *      Compaq regained the #1 platform partner position with SAP with 33%
    > market share over all platforms (NT, UNIX with R/3, and mySAP.com). IBM is
    > 2nd in line with 23% share. In North America alone, our overall SAP share
    > increased from 25% to 32% in the third quarter.  As an aside, SAP's entire
    > executive board and senior executive staff use our iPAQ Pocket PCs.
    > Rollout of the product to SAP Sales and Marketing is also in progress -- a
    > very visible endorsement of Compaq's leadership in Internet access as it
    > applies to enterprise applications.
    > * Cable & Wireless CEO and executive visit to Marlboro in October
    > included CEO Graham Wallace and 56 top C&W executives. C&W new ASP
    > 'a-Services' UK launch on October 31 followed the successful U.S. launch
    > in late September.
    > * Compaq secured the notebook business with CGE&Y UK for their
    > internal use.  Toshiba had been the incumbent for 5 years.  CGE&Y is
    > upgrading to Oracle 11i on Alpha Tru64 UNIX.  As one of the first
    > customers globally to upgrade to 11i on Alpha, they have agreed to be a
    > reference site.
    > * Our successful Platinum Sponsorship of Commerce One's Global Trading
    > Web Technical Forum included a Compaq keynote and non-disclosure breakout
    > session on new ProLiant 8-ways.
    > * A  9-city roadshow in EMEA was kicked-off with Intel, starting in
    > Munich.  This is an extension of the successful 11 city roadshow in the
    > U.S. that drove traffic to the speedStart website and should do the same
    > for EMEA..
    > * Strong Compaq presence with Premier sponsorship at Oracle Open World
    > in October included Michael Capellas luncheon speech to 200+ C-level
    > customers, on-stage server presence at Larry Ellison keynote, and
    > excellent Compaq coverage in Oracle publications.
    > * Announced major Mid-Market Initiative Contract with Siebel.  We had
    > very high visibility at Siebel User Week, and also won Siebel's Platform
    > Partner of the Year awards for Excellence in EMEA and NA.  We recently
    > announced a Benchmark Figure of 10,200 Siebel users running Microsoft NT
    > and SQL 7 on ProLiant systems.
    > * Compaq had a strong presence at COMDEX with strategic partner,
    > Microsoft.  In addition to  supporting Bill Gates' keynote address, the
    > Microsoft booth featured iPAQ Pocket PCs demonstrating the award-winning
    > OmniSky wireless Internet and e-mail service running on Metricom's
    > Ricochet network - the world's fastest mobile broadband network.
    > Microsoft also announced the immediate availability of its Windows Media
    > Player Technology Preview Edition on Compaq Pocket PCX devices, which for
    > the first time delivers streamed wireless Windows Media-formatted audio
    > and video to a portable device.
    >
    > Global Accounts news
    > * Do you know about the Discovery, Design and Implementation (DDI)
    > application? Global Accounts has moved the DDI application into
    > production, resulting in a Web-enabled tool that streamlines and automates
    > the DDI phases for signing up new customers.
    > http://vinproapp03.cce.cpqcorp.net/ddi/
    > * More than 130 people from Compaq EMEA Global Accounts attended a
    > conference center at EuroDisney, Paris, for a training program that
    > included a focus on personal development skills and a broader look at how
    > Global Accounts can build sales.
    
    > * A CD and brochure designed to give Global Accounts salespeople and
    > customers a greater insight into the business can be ordered online
    > through the GA catalog.
    > http://inline.compaq.com/corpmktg/globalaccounts/know/resourcekit.asp
    > * For the first time, Compaq has a single, documented global special
    > pricing process, enabling us to be smarter than the competition on global
    > bids.  Implementation of this process is expected to begin January 1. For
    > more information, see
    > http://inline.compaq.com/corpmktg/globalaccounts/div/stratplan/index.asp
    > or e-mail Philip Kyle.
    > * Global Account managers and others whose customers and prospects
    > require multi-platform hardware, operating systems and applications will
    > want to know about the IQ Center. With more than 150 systems engineering
    > personnel, 30,000 square-feet of lab space, 500 CPUs and 100 TB of
    > storage, the Center is a well-equipped, one-stop shop for designing and
    > testing complex solutions.
    > http://inline.compaq.com/corpmktg/globalaccounts/div/gamclose.asp
    >
    > CPCG headlines
    > * Compaq regained total PC and PC server market share leadership in
    > the UK during 3Q.
    > * Among our many announcements at Comdex, we introduced the
    > three-pound, MP2800 - the world's smallest projector -- as well as
    > iPAQnet, a collection of products and solutions designed to redefine the
    > Internet experience for customers demanding wireless access to e-mail and
    > other corporate information. Last, Compaq and Oracle announced an all-new
    > Internet appliance based on ProLiant servers and the latest Oracle
    > software to deliver the fastest cache on the Internet. Oracle is backing
    > up the performance pledge with a $1 million guarantee.
    >
    > Ratings and reviews
    > * Computer Shopper named the iPAQ Pocket PC one of the "Top 100
    > Products of 2000"
    > * "Looking for the perfect present for the technophile who has
    > everything? Then check out the Compaq iPAQ Pocket PC ... the iPAQ is a lot
    > slimmer than most of the competition ... Plus, its brilliant 12-bit,
    > 4,096-color reflective display will be sure to make the holiday season
    > especially bright." - ZDNet
    > * Popular Science recognized the iPAQ Pocket PC at an awards ceremony
    > for being one of the year's 100 "hottest products and eye-opening
    > discoveries." The iPAQ Pocket PC is pictured on the cover of the
    > magazine's December edition, now on newsstands.
    > * "Sure, the Compaq iPAQ Pocket PC PDA has everything a desktop PC has
    > - word processor, Internet browser, e-mail engine, etc., etc. But that's
    > not even half the story: It can crank out color video and blast MP3 music
    > through a stereo headphone jack..." - Stuff Magazine
    >
    > Portables garner praise
    > * The Armada E500S received the "Four-Star Award" from Computer
    > Shopper. "Overall, the Armada E500S is a compelling, well-designed package
    > for small businesses ... you get a solid mix of components for the money."
    > - Computer Shopper
    > * The Notebook 100 was named one of the Top 100 Products of the Year
    > by Computer Shopper. "We were duly impressed with Compaq's price-defying
    > Notebook 100."
    >
    >
    > Consumer Group highlights
    > * Last month, we shipped our 500,000th Configure-to-Order unit. U.S.
    > CTO sales grew 256% in the third quarter.
    > * Worldwide beyond-the-box revenue in Q3 increased 90% year-over-year.
    > * Of the top 25 countries with the highest Consumer sales worldwide,
    > six are in the Latin America region: Mexico (2), Brazil (4), Argentina
    > (8), Chile (16), Peru/Bolivia (21) and Colombia (22).
    > * Consumer's EMEA region hit the $1 billion sales mark in mid-October,
    > two months earlier than in 1999.
    > * More than 50,000 DSL-ready Presario computers have been sold through
    > our deal with Southwestern Bell.
    > http://newscpq1.inline.cpqcorp.net/article.cfm?storyid=1034
    > * Popular Science magazine included the iPAQ Home Internet Appliance
    > in its "Best of What's New" in the computer and software category.
    >
    > Storage Product Group news
    > *      Compaq Belgium and Luxembourg have won four DATANEWS Awards for
    > Excellence, one of which was in the category of Enterprise Storage: Compaq
    > StorageWorks systems. Compaq also received Awards of Excellence for
    > Enterprise Server (ProLiant); High-End Workstations (Compaq Professional
    > Workstation), and Services. The Compaq Aero Professional Digital Assistant
    > (PDA) received a Quality Award. For more info, visit:
    > http://datanews.vnunet.be/dnafe0.asp
    > *      An elite group of storage networking companies has joined our
    > commitment to support VersaStor Technology - the industry's premier
    > implementation of networked storage pooling. These endorsements represent
    > an important milestone in enabling SAN customers to leverage business
    > information as a virtual resource.
    > http://www.compaq.com/newsroom/pr/2000/pr2000103002.html
    > * Construction has begun on the Storage Networking Industry
    > Association Technology Center (SNIA Technology Center) in Colorado
    > Springs, Colo. Upon completion, the 14,000-square-foot center will be the
    > largest independent storage networking lab in the world.
    > http://storage.inet.cpqcorp.net/download/doc/SNIA_Release_final.doc
    > * At last month's Storage Networking World conference, Compaq and IBM
    > demonstrated for the first time true multi-vendor online storage
    > interoperability for the Open SAN Earlier this month we announced three
    > new storage service offerings that accelerate SAN implementation, improve
    > enterprise backup performance and increase availability and reliability of
    > remote storage management.
    > http://www.compaq.com/newsroom/pr/2000/pr2000103001.html
    >
    > Business Critical Server Group highlights
    > * Our Tru64 UNIX business is gaining momentum - growing twice as fast
    > as the market in Q2 and Q3 of this year, according to International Data
    > Corp. IDC reports that Compaq was the fastest growing UNIX vendor in Q2,
    > with 25% growth versus overall UNIX market growth of 13%.
    > * On Oct. 31, we announced new Tru64 UNIX, TruCluster and AlphaServer
    > products and services enhancements to improve scalability and ease of
    > deployment for e-business solutions.
    > http://alphaserver.inet.cpqcorp.net/announcements/30oct00/index.html
    > * The International Tandem Users' Group (ITUG) Summit 2000, held Oct.
    > 15-19 in San Jose, Calif., was the largest ever, drawing 2,900 customers,
    > partners, internal developers and executives. A highlight of the general
    > session was a live demonstration of the Zero Latency Enterprise
    > architecture for customer relationship management, which brings together
    > Himalaya, AlphaServer and ProLiant platforms.
    >
    > North America eBusiness Solutions successes
    > *      Service Provider Winstar has signed Compaq as its exclusive
    > provider of NT and storage products and committed to purchase a minimum of
    > $100M of Compaq products over the next four years, $10M of which will be
    > for Alpha UNIX for their rapidly growing complex hosting business.  We're
    > also providing $50M in financing directly to Winstar and $50M in financing
    > to approved Winstar customers.  Compaq Services has been designated as a
    > Winstar Services Partner.
    > *      Exodus placed an initial order of more than 500 ProLiant servers
    > for their Intel-managed hosting platform.
    > * Compaq also inked a deal with Siebel Systems to create a dedicated
    > partner sales channel and a $30M joint marketing initiative for an
    > integrated hardware/software offering to small and medium enterprises.
    > Over 80 sales agents are being authorized to sell the packages, which are
    > delivered fully integrated by Compaq Direct.
    >
    > Compaq Financial Services making a difference
    > *      Compaq Financial Services was instrumental in helping to shut out
    > IBM and HP from long-time Compaq customer AltaVista by putting into place
    > a $25M fair market value lease for NT and AlphaServers.  Through the deal,
    > CFS increased Alta Vista's lease line to $75M.
    > * CFS scored its first local currency financing in Brazil with a $3M
    > deal for servers and services.  In awarding the contract over competitors
    > HP and IBM and their respective financing groups, Ericsson cited
    > differentiating factors including Compaq's technology and our ability to
    > provide a competitive price in local currency.  CFS invoicing
    > capabilities, including information for each separate Ericsson cost center
    > in Brazil, was also a deciding factor.
    > * CFS helped facilitate the largest delivery of Intel servers
    > (ProLiant ML 370) to the Czech Republic through a $2.8M, 3-year operating
    > lease transaction with Czech Savings Bank.  CFS was the only leasing
    > company to offer a sub-lease structure, a differentiating factor that won
    > the business over Dell and IBM.
    >
    > CEI changes name to Compaq Direct
    > Custom Edge Inc., a wholly owned Compaq subsidiary formed, is now called
    > Compaq Direct. In other "direct" news, did you know that we have more than
    > 230 major accounts now buying from us directly and more in the pipeline?
    > Combined revenue in Q3 from PartnerDirect, DirectPlus, Major Account
    > Direct and GEM Direct totaled nearly 40 percent of CPCG's total revenue in
    > North America. What's more, ISSG revenue was more than 27 percent direct
    > in Q3.
    >
    > Siebel's Platform Partner of the Year
    > I'm pleased to report that Siebel has named Compaq its Platform Partner of
    > the Year for excellence in both EMEA and North America. We recently
    > received high visibility at the Siebel User Week event while also
    > announcing a record benchmark of 10,200 Siebel users running Microsoft NT
    > and SQL 7 on ProLiant servers.
    >
    > Get Informed
    > Inform, Compaq's customer magazine, is now available in printed and
    > electronic versions. It's free and available for you to read. Sign your
    > customers up by visiting the U.S. (www.compaq.com/inform/issues/sb.html),
    > Asia Pacific (www.compaq.com.tw/) or EMEA (www.compaq.com/emea/inform)
    > sites.
    >
    > North America eCommerce and CRM marketing activities
    > * North America recently released IMPAQ express, a Web-based tool for
    > Customer Relationship Management (CRM) campaign planning and audience
    > sizing, to its marketing and sales force. For the first time, campaign
    > planning can start with a quick and easy look at the size and scope of a
    > potential installed-based audience.
    > * Compaq recently co-sponsored eLink, a B2B e-commerce event targeted
    > at procurement, IT, marketing and financial executives hosted by Commerce
    > One in Las Vegas. Attendees witnessed the on-stage construction of a live
    > e-marketplace powered by Commerce One and Compaq servers. In addition, we
    > demonstrated our Roundtrip configuration and Auction capabilities.
    >
    > Wins Around the World
    > As always, thanks to everyone for your tremendous efforts this month.
    > Please take a few minutes to look over the complete list of recent wins
    > around the world and continue to write me with your news and success
    > stories. http://inline.compaq.com/wwss/wins/worldwins.asp
    >
    > Let's finish the quarter strongly!!
    >
    > Regards,
    > Peter
    >
    > 
    Tue, 12 Dec 2000 18:49:21 -0600
    Date: Tue, 12 Dec 2000 18:49:21 -0600
    Message-Id: <200012130049.SAA07799@server1.pjdoland.com>
    To: klay@enron.com
    From: David Theroux <DJTheroux@independent.org>
    Reply-to: DJTheroux@independent.org
    X-Mailer: Perl Powered Socket Mailer
    Subject: THE LIGHTHOUSE: December 12, 2000
    
    THE LIGHTHOUSE
    "Enlightening Ideas for Public Policy..."
    VOL. 2, ISSUE 48
    December 12, 2000
    
    Welcome to The Lighthouse, the e-mail newsletter of The Independent 
    Institute, the non-partisan, public policy research organization 
    <http://www.independent.org>. We provide you with updates of the Institute's 
    current research publications, events and media programs.
    
    -------------------------------------------------------------
    
    IN THIS WEEK'S ISSUE:
    1. Pentagon "Shocked" to Find Rivers Dammed with Pork
    2. The Environmental Propaganda Agency
    3. William Lloyd Garrison, Antislavery Crusader
    
    -------------------------------------------------------------
    
    PENTAGON "SHOCKED" TO FIND RIVERS DAMMED WITH PORK
    
    Captain Louis Renault -- Claude Raines's cheerfully duplicitous character in 
    the 1942 film classic "Casablanca" -- asserted glibly that he was "shocked, 
    shocked" to learn that gambling was taking place at Rick's Cafe. Moments 
    later he was only too happy to collect his gambling earnings for the night.
    
    All this is by way of preamble to a new Pentagon investigation of fraud in 
    military construction. The investigation concluded that three senior Army 
    Corps of Engineers officials had, just as one whistle-blowing Corps economist 
    had claimed, engaged in a deceitful campaign to justify what the Washington 
    Post called "a billion-dollar construction binge on the Mississippi and 
    Illinois rivers."
    
    "The [Pentagon] investigators concluded that the agency's aggressive efforts 
    to expand its budget and missions, as well as its eagerness to please its 
    corporate customers and congressional patrons, have helped 'create an 
    atmosphere where objectivity in its analyses was placed in jeopardy,'" the 
    Post reports.
    
    "Even the agency's retired chief economist told them that Corps studies were 
    often 'corrupt,' and that several Corps employees cited 'immense pressure' to 
    green-light questionable projects."
    
    Bureaucratic boondoggles of such magnitude are certainly newsworthy. But they 
    are hardly news. Just as the Soviets derided the failures of previous Five 
    Year Plans (only to implement new, equally flawed versions), so it seems that 
    every few years the Pentagon uncovers massive corruption and waste in its own 
    centrally planned fiefdom -- only to present a new Plan that operates under 
    the same bad incentives that encouraged prior malfeasance.
    
    With corruption and waste seemingly "taken care of," the worst pork-barrel 
    spenders in Congress and the military are then let off the hook, only to 
    enjoy -- like Casablanca's Renault and Rick -- an amicable toast to the 
    beginnings of a beautiful new friendship.
    
    For the Washington Post series on the Army Corp of Engineers boondoggle, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-1.html.
    
    For a summary of the Independent Institute book, ARMS, POLITICS AND THE 
    ECONOMY: Historical and Contemporary Perspectives, edited by Robert Higgs, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-2.html.
    
    -------------------------------------------------------------
    
    THE ENVIRONMENTAL PROPAGANDA AGENCY
    
    Will the neck-to-neck presidential race help reduce -- or intensify -- 
    pressure for the next president of the United States to score points with 
    statist environmental activists?
    
    Except on a few controversial issues, a strong case can be made that the 
    forty-third President of the United States will wish to portray himself as a 
    close friend of "the environment." President George W. Bush, for example, 
    would face strong pressure to show that he is "bipartisan" in his approach to 
    environmental protection; whereas President Al Gore would likely attempt to 
    win back those who supported Nader and the Greens.
    
    All the more reason, then, to call attention to the failures of the current 
    approach to environmental protection -- especially those emanating from the 
    U.S. Environmental Protection Agency, or, as economist Craig Marxsen terms 
    it, the Environmental Propaganda Agency.
    
    The EPA sometimes employs the language of cost-benefit analysis to illustrate 
    its seemingly tremendous success, but it is known to employ it in a highly 
    misleading manner. The EPA claimed, for example, that its Clean Air Act 
    programs produced, from 1970 to 1990, $22.2 trillion dollars in health 
    benefits at a cost of only $523 billion. But, reports Marxsen in THE 
    INDEPENDENT REVIEW, "[The EPA's] study actually represents a milestone in 
    bureaucratic propaganda. Like junk science in the courtroom, the study 
    seemingly attempts to obtain the largest possible benefit figure rather than 
    to come as close as possible to the truth."
    
    In conclusion, writes Marxsen, "Without the illusory benefit of all the lives 
    saved, the actual benefits of the Clean Air Act were very modest and probably 
    could have been achieved nearly as well with far less sacrifice. The Clean 
    Air Act and its amendments force the EPA to mandate reduction of air 
    pollution to levels that would have no adverse health effects on even the 
    most sensitive person in the population. The EPA relentlessly presses forward 
    in its absurd quest, like a madman setting fire to his house in an insane 
    determination to eliminate the last of the insects infesting it."
    
    For more information, see "The Environmental Propaganda Agency," by Craig S. 
    Marxsen (THE INDEPENDENT REVIEW, Summer 2000), at
    http://www.independent.org/tii/lighthouse/LHLink2-48-3.html.
    
    For analysis of other EPA programs, see the Independent Institute book, 
    CUTTING GREEN TAPE: Toxic Pollutants, Environmental Regulation and the Law, 
    edited by Richard Stroup and Roger Meiners, at 
    http://www.independent.org/tii/lighthouse/LHLink2-48-4.html.
    
    For Robert Formaini's insightful review of Kip Viscusi's important book, 
    RATIONAL RISK POLICY (THE INDEPENDENT REVIEW, Winter 1999), see
    http://www.independent.org/tii/lighthouse/LHLink2-48-5.html.
    
    -------------------------------------------------------------
    
    WILLIAM LLOYD GARRISON, Antislavery Crusader
    
    "I will be as harsh as truth, and as uncompromising as justice. On this 
    subject, I do not wish to think, or speak, or write with moderation.... I 
    will not equivocate -- I will not excuse -- I will not retreat a single inch 
    -- and I will be heard."
         -- William Lloyd Garrison, THE LIBERATOR, January 1, 1831
    
    December 12 marks the 195th anniversary of the birth of William Lloyd 
    Garrison, a leading figure in the American abolitionist movement. As the late 
    historian Henry Mayer explained in his National Book Award-Finalist 
    biography, ALL ON FIRE: William Lloyd Garrison and the Abolition of Slavery:
    
    "William Lloyd Garrison (1805-1879) is an authentic American hero who, with a 
    Biblical prophet's power and a propagandist's skill, forced the nation to 
    confront the most crucial moral issue in its history. For thirty-five years 
    he edited and published a weekly newspaper in Boston, THE LIBERATOR, which 
    remains today a sterling and unrivaled example of personal journalism in the 
    service of civic idealism.
    
    "Although Garrison -- a self-made man with a scanty formal education -- 
    considered himself 'a New England mechanic' and lived outside the precincts 
    of the American intelligentsia, he nonetheless did the hard intellectual work 
    of challenging orthodoxy, questioning public policy, and offering a luminous 
    vision of a society transformed. He inspired two generations of activists -- 
    female and male, black and white -- and together they built a social movement 
    which, like the civil rights movement of our own day, was a collaboration of 
    ordinary people, stirred by injustice and committed to each other, who 
    achieved a social change that conventional wisdom first condemned as wrong 
    and then ridiculed as impossible."
    
    Indeed, without Garrison's inflammatory but compelling writing, speaking and 
    organizing, there might have been no effective American anti-slavery movement 
    at all.
    
    For more on William Lloyd Garrison, read historian Henry Mayer's talk from 
    the Independent Policy Forum, "The Civil War: Liberty and American Leviathan" 
    (with Jeffrey Rogers Hummel), at
    http://www.independent.org/tii/lighthouse/LHLink2-48-6.html, or hear it in 
    RealAudio at http://www.independent.org/tii/lighthouse/LHLink2-48-7.html.
    
    Also see Jeffrey Rogers Hummel's review of Henry Mayer's brilliant biography, 
    ALL ON FIRE: William Lloyd Garrison and the Abolition of Slavery (THE 
    INDEPENDENT REVIEW, Fall 2000), at
    http://www.independent.org/tii/lighthouse/LHLink2-48-8.html.
    
    -------------------------------------------------------------
    
    If you enjoy receiving THE LIGHTHOUSE ... please help us support it.
    
    Your supporting Independent Associate Membership enables us to reach 
    thousands of other people. So, please make a contribution to The Independent 
    Institute. See http://www.independent.org/tii/lighthouse/LHLink2-48-9.html. 
    to donate, or contact Ms. Priscilla Busch by phone at 510-632-1366 x105, fax 
    to 510-568-6040, email to <PBusch@independent.org>, or snail mail to The 
    Independent Institute, 100 Swan Way, Oakland, CA 94621-1428.
    All contributions are tax-deductible.  Thank you!
    
    -------------------------------------------------------------
    
    For previous issues of THE LIGHTHOUSE, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-10.html.
    
    -------------------------------------------------------------
    
    For information on books and other publications from The Independent 
    Institute, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-11.html.
    
    -------------------------------------------------------------
    
    For information on The Independent Institute's Independent Policy Forums, see
    http://www.independent.org/tii/lighthouse/LHLink2-48-12.html.
    
    -------------------------------------------------------------
    
    To subscribe (or unsubscribe) to The Lighthouse, please go to 
    http://www.independent.org/subscribe.html, choose "subscribe" (or 
    "unsubscribe"), enter your e-mail address and select "Go."
    
    Copyright , 2000 The Independent Institute
    100 Swan Way
    Oakland, CA 94621-1428
    (510) 632-1366 phone
    (510) 568-6040 fax
    info@independent.org
    http://www.independent.org
    
    Good morning, Liz -
    
    I left a message at your home this morning that your Dad would like to speak 
    with you when you have a chance to call.  
    
    Rosie
    
    p.s. - P. L. and I did early voting the first Saturday available!!  It was 
    such a good feeling as Tuesdays are tough with trying to get to the office 
    and leave in time to vote!!
    
    
    
    
    
    Elizabeth Lay <lizard_ar@yahoo.com> on 11/06/2000 09:13:26 AM
    To: ealvittor@yahoo.com
    cc:  
    Subject: Get out the Vote
    
    
    Dear Friends and Family,
    We are down to the wire and the race couldn't be
    closer. Every vote counts, even those of you in Texas.
    Please, remember to vote and go early, polls open at
    7am across the country and close at 7pm but don't put
    it off until 7pm as there could be lines and the
    supervisors are not required to keep the polls open
    after 7pm for those in line.
    Remind your friends and family to vote and if someone
    needs transportation to the polls, give them a ride or
    call your local Victory 2000 office (Republicans) or
    your local Democratic campaign office, they will have
    people available to transport people to the polls.
    Keep an eye out while you are at the polls for
    electoral fraud and report any suspicious activity
    including inappropriate campaigning to the poll
    watchers at the polls. The Democrats and the
    Republicans will have representatives at most of the
    polls, let them know if there is suspicious activity.
    Finally, if you have time, call your local offices and
    offer your time to make phone calls to encourage
    people to get out and vote, pass out leaflets, or to
    be available to help prevent of voter fraud.
    
    Finally, enjoy the evening and watch the results come
    in. There are election watch parties everywhere!!!
    
    Just one more day (and no more mass political e-mail's
    from me and everyone else)!
    Take care and VOTE!
    Liz
    
    __________________________________________________
    Do You Yahoo!?
    Thousands of Stores.  Millions of Products.  All in one Place.
    http://shopping.yahoo.com/
    
    
    


```python

```




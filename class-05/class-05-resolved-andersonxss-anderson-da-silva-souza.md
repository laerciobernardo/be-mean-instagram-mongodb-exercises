###MongoDB - Aula 05 - Exercício
Autor: Anderson da Silva Souza

#1. Importar as collections restaurantes e pokemons


POKEMONS

anderson-pc@andersonpc-Lenovo-G400s:~/Documentos/estudos/be-mean-mongodb$ mongoimport --db be-mean --collection pokemons --drop --file pokemons.json
connected to: 127.0.0.1
2015-11-23T22:59:33.937-0300 dropping: be-mean.pokemons
2015-11-23T22:59:33.955-0300 check 9 610
2015-11-23T22:59:33.959-0300 imported 610 objects


RESTAURANTES

anderson-pc@andersonpc-Lenovo-G400s:~/Documentos/estudos/be-mean-mongodb$ mongoimport --db --be-mean-restaurantes --collection restaurantes --drop --file restaurantes.json
connected to: 127.0.0.1
2015-11-23T23:04:30.020-0300 dropping: --be-mean-restaurantes.restaurantes
2015-11-23T23:04:30.574-0300 check 9 25359
2015-11-23T23:04:30.704-0300 imported 25359 objects

#2. Distinct por cuisine na restaurantes

db.restaurantes.distinct('cuisine')
[
  "Bakery",
  "Hamburgers",
  "Irish",
  "American ",
  "Jewish/Kosher",
  "Delicatessen",
  "Ice Cream, Gelato, Yogurt, Ices",
  "Chinese",
  "Other",
  "Chicken",
  "Turkish",
  "Caribbean",
  "Donuts",
  "Sandwiches/Salads/Mixed Buffet",
  "Bagels/Pretzels",
  "Continental",
  "Pizza",
  "Italian",
  "Steak",
  "Polish",
  "Latin (Cuban, Dominican, Puerto Rican, South & Central American)",
  "German",
  "French",
  "Pizza/Italian",
  "Mexican",
  "Spanish",
  "Café/Coffee/Tea",
  "Tex-Mex",
  "Pancakes/Waffles",
  "Soul Food",
  "Seafood",
  "Hotdogs",
  "Greek",
  "Not Listed/Not Applicable",
  "African",
  "Japanese",
  "Indian",
  "Armenian",
  "Thai",
  "Chinese/Cuban",
  "Mediterranean",
  "Korean",
  "Bottled beverages, including water, sodas, juices, etc.",
  "Russian",
  "Eastern European",
  "Middle Eastern",
  "Asian",
  "Ethiopian",
  "Vegetarian",
  "Barbecue",
  "Egyptian",
  "English",
  "Sandwiches",
  "Portuguese",
  "Indonesian",
  "Chinese/Japanese",
  "Filipino",
  "Juice, Smoothies, Fruit Salads",
  "Brazilian",
  "Afghan",
  "Vietnamese/Cambodian/Malaysia",
  "CafÃ©/Coffee/Tea",
  "Soups & Sandwiches",
  "Tapas",
  "Moroccan",
  "Pakistani",
  "Peruvian",
  "Bangladeshi",
  "Czech",
  "Salads",
  "Creole",
  "Fruits/Vegetables",
  "Iranian",
  "Cajun",
  "Scandinavian",
  "Polynesian",
  "Soups",
  "Australian",
  "Hotdogs/Pretzels",
  "Southwestern",
  "Nuts/Confectionary",
  "Hawaiian",
  "Creole/Cajun",
  "Californian",
  "Chilean"
]


#3.Fazer um distinct por types na collection pokemons

be-mean> db.pokemons.distinct('types')
[
  "normal",
  "fire",
  "water",
  "bug",
  "flying",
  "poison",
  "electric",
  "steel",
  "ice",
  "ghost",
  "fighting",
  "psychic",
  "grass",
  "ground",
  "fairy",
  "rock",
  "dark",


#4. As primeiras 3 páginas com .limit() e .skip() de pokemons (5 e 5)

db.pokemons.find({},{name:1,_id:0}).limit(5).skip(5*0)
{
  "name": "Rattata"
}
{
  "name": "Charmander"
}
{
  "name": "Charmeleon"
}
{
  "name": "Wartortle"
}
{
  "name": "Blastoise"
}
Fetched 5 record(s) in 1ms


db.pokemons.find({},{name:1,_id:0}).limit(5).skip(5*1)
{
  "name": "Caterpie"
}
{
  "name": "Metapod"
}
{
  "name": "Butterfree"
}
{
  "name": "Spearow"
}
{
  "name": "Kakuna"
}
Fetched 5 record(s) in 1ms

db.pokemons.find({},{name:1,_id:0}).limit(5).skip(5*2)
{
  "name": "Farfetchd"
}
{
  "name": "Magnemite"
}
{
  "name": "Magneton"
}
{
  "name": "Doduo"
}
{
  "name": "Seel"
}
Fetched 5 record(s) in 1ms

#5 Group ou Aggregate contando a quantidade de pokemons de cada tipo

be-mean> db.pokemons.group({
...   initial: { total: 0 },
...   reduce: function(current, result) {
...     current.types.forEach(function(type) {
...       if (result[type]) {
...         result[type]++;
...       } else {
...         result[type] = 1;
...       }
...       result.total++;
...     });
...   },
... })
[
  {
    "total": 915,
    "normal": 78,
    "fire": 47,
    "water": 105,
    "bug": 61,
    "flying": 77,
    "steel": 37,
    "electric": 40,
    "poison": 54,
    "ghost": 34,
    "ice": 24,
    "fighting": 38,
    "psychic": 62,
    "grass": 75,
    "ground": 51,
    "fairy": 28,
    "rock": 46,
    "dark": 38,
    "dragon": 20
  }
]

#6. Realizar 3 counts na pokemons.

Todos

be-mean> db.pokemons.count()
610


Só do tipo fire
be-mean> db.pokemons.count({types:'fire'})
47


Só dos que tem defesa maior que 70
be-mean> db.pokemons.count({defense:{$gt:70}})
250

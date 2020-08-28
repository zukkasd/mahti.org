---
title: "JavaScript"
author: "Greatman Lim"
muistiinpanoja: ["Ohjelmointi"]
---

Henkilökohtaisia muistiinpanoja javascriptistä, mikä ei ole tarkoitettu oppimateriaaliksi. Sisältö koostuu pääosin suunnittelumalleista ja funktionaalisesta ohjelmoinnista.

## var, let vai ei mitään? [^1]

Lähes aina kannattaa käyttää joko `let` tai `const` muuttujia nimittäessä.

`var` näkyvyysalue vuotaa `for` lauseessa, joka on hieman outo ominaisuus, jos on ohjelmoinut muilla kielillä. `var` näkyvyys pysyy kuitenkin funktion sisällä. Jos muuttuja luodaan ilman avainsanaa, tulkkaaja pyrkii etsimään saman nimisen muuttujan ylemmästä hierarkiasta. Jos muuttujaa ei löydy, javascript luo sen ylimpään hierarkiaan, eli muuttujasta tulee globaali. Yleensä tämä ei ole haluttu ominaisuus, ja se voidaan estää kirjoittamalla javascript tiedoston ensimmäiseksi riviksi `"use strict";`.

## Milloin puolipilkku on tarpeellinen?

Jos seuraava rivi alkaa sululla `(`, on ylempään riviin laitettava puolipiste. Muuten javascript luulee, että nämä kaksi ovat samaa riviä.

## Nuolifunktiot [^2]

Nuolifunktio ei ole pelkästään lyhennös avainsanalle `function`. Nuolifunktioissa `this` ei toimi samalla tavalla kuin perinteisessä funktiossa.

Oletetaan, että on seuraavanlainen taulukko, jossa on objekteja:

{{< highlight javascript >}}

const dragonEvents = [
  { type: 'attack', value: 12, target: 'player-dorkman' },
  { type: 'yawn', value: 40 },
  { type: 'eat', target: 'horse' },
  { type: 'attack', value: 23, target: 'player-fluffykins' },
  { type: 'attack', value: 12, target: 'player-dorkman' }
]

{{< /highlight >}}

Seuraavaksi on laskettava player-dorkmannin yhteinen hyökkäys summa. Tehdään seuraavalla tavalla:

  1. suodatetaan `attack`
  2. suodatetaan `player-dorkman`
  3. kerätään `value` kenttien arvot
  4. lasketaan kerätyt arvot yhteen

Käytetään normaaleja funktioita:

{{< highlight javascript >}}

const totalDamageOnDorkman = dragonEvents
  .filter(function(event) {
  return event.type === 'attack'
  })
  .filter(function(event) {
    return event.target === 'player-dorkman'
  })
  .map(function(event) {
    return event.value
  })
  .reduce(function(prev, value) {
    return (prev || 0) + value
  })

{{< /highlight >}}

Edellinen koodi refraktuoitu nuolifunktioita käyttäen:

{{< highlight javascript >}}

const totalDamageOnDorkman = dragonEvents
  .filter((event) => {
  return event.type === 'attack'
  })
  .filter((event) => {
    return event.target === 'player-dorkman'
  })
  .map((event) => {
    return event.value
  })
  .reduce((prev, value) => {
    return (prev || 0) + value
  })

{{< /highlight >}}

Mutta voidaan saada vielä paremmaksi:

{{< highlight javascript >}}

const totalDamageOnDorkman = dragonEvents
  .filter(event => event.type === 'attack')
  .filter(event => event.target === 'player-dorkman')
  .map(event => event.value)
  .reduce((prev, value) => (prev || 0) + value)

{{< /highlight >}}

Jos funktiolla vain yksi rivi koodia, voidaan poistaa aaltosulut ja `return`. Myöskin parametrien ympäriltä voidaan ottaa sulut pois, paitsi silloin kun parametrejä on kaksi tai enemmän.

## Funktionaalinen ohjelmointi [^3]

Funktio normaalisti:

{{< highlight javascript >}}

function triple(x) {
  return x * 3
}

{{< /highlight >}}

Funktionaalisessa ohjelmoinnissa funktiot ovat arvoja muuttujissa.

{{< highlight javascript >}}

let triple = function(x) {
  return x * 3
}

let foo = triple
foo(30)

{{< /highlight >}}

Koska funktio on arvo muuttujassa, voidaan kyseinen muuttuja antaa argumenttina toiselle funktiolle.

Oletetaan, että on taulukko, jossa on objekteja:

{{< highlight javascript >}}

let animals = [
  { name: 'Fluffyskins', species: 'rabbit' },
  { name: 'Caro', species: 'dog' },
  { name: 'Hamilton', species: 'dog' },
  { name: 'Harold', species: 'fish' },
  { name: 'Ursula', species: 'cat' },
  { name: 'Jimmy', species: 'fish' }
]

{{< /highlight >}}

Kerätään kaikki koirat samalla taulukolle käyttäen toistorakennetta `for`.

{{< highlight javascript >}}

let dogs = []
for (let i = 0; i < animals.length; i++) {
  if (animals[i].species === 'dog')
    dog.push(animals[i])
}

{{< /highlight >}}

Sama kuin edellinen `filter` funktiolla:

{{< highlight javascript >}}

let dogs = animals.filter(animal => animal.species === 'dog' )

{{< /highlight >}}

Uudelleenkäyttettävyys esimerkki: käytetään funktiota `.reject`:

{{< highlight javascript >}}

let isDog = (animal => animal.species === 'dog' )
let dogs = animals.filter(isDog)
let otherAnimals = animals.reject(isDog)

{{< /highlight >}}

### map

Käytetään samaa eläintaulukkoa kuin edellisessä kappaleessa. Nyt tavoitteena on kerätä kaikki eläinten nimet yhdeksi taulukoksi. Ratkaistaan se toistorakenteella:

{{< highlight javascript >}}

let names = []
for ( var i = 0; i < animals.length; i++ ) {
  names.push(animals[i].name)
}

{{< /highlight >}}

Sama `map` funktiolla:

{{< highlight javascript >}}

let names = animals.map(animal => animal.name)

{{< /highlight >}}

Voidaan lisätä:

{{< highlight javascript >}}

let names = animals.map(animal => animal.name + ' is a ' + animal.species)

{{< /highlight >}}

### reduce

Funktio `reduce` on hyvä, kun mikään muu ei toimi. Käytännössä `reduce` voi korvata kaikki muut, eli se on yleispätevä työkalu. Käytetään seuraavanlaista tietokantaa:

{{< highlight javascript >}}

let orders = [
  { amount: 250 },
  { amount: 400 },
  { amount: 100 },
  { amount: 325 }
]

{{< /highlight >}}

Lasketaan kaikki yhteen toistorakenteella:

{{< highlight javascript >}}

let totalAmount = 0
for (let i = 0; i < orders.length; i++) {
  totalAmount += orders[i].amount
}

{{< /highlight >}}

Funktion `reduce` kanssa:

{{< highlight javascript >}}

let totalAmount = orders.reduce((sum, item) => sum + item.amount, 0)

{{< /highlight >}}

Huomaa, että funktio `reduce` ottaa parametrin. Tämä on esimerkissä argumenttille `sum` tarkoitettu arvo.

Katsotaan seuraavaksi hieman vaikeampaa esimerkkiä.Oletetaan, että on tekstitiedosto `data.txt`, missä eri kentät on rajattu tabulaattoria käyttäen:

{{< highlight text >}}

mark johansson waffle iron 80 2
mark johansson blende 200 1
mark johansson knife 10 4
Nikita Smith waffle iron 80 1
Nikita Smith knife 10 2
Nikita Smith pot 20 3

{{< /highlight >}}

Tehtävänä on muuttaa edellinen tekstitiedosto muotoon:

{{< highlight javascript >}}

{
  'mark johansson': [
    { name: 'waffle iron', price: '80', quantity: '2' },
    { name: 'blender', price: '200', quantity: '1' },
    { name: 'knife', price: '10', quantity: '4' },
  ]
  'Nikita Smith': [
    { name: 'waffle iron', price: '80', quantity: '1' },
    { name: 'knife', price: '10', quantity: '2' },
    { name: 'pot', price: '20', quantity: '3' },
  ]
}

{{< /highlight >}}

Vastaus:

{{< highlight javascript >}}

import fs from 'fs'

let output = fs.readFileSync('data.txt', 'utf8')
  .trim() // poistetaan lopusta ylimääräinen merkki
  .split('\n')
  .map(line => line.split('\t'))
  .reduce((customers, line) => {
    customers[line[0]] = customers[line[0]] ||  []
    customers[line[0]].push({
      name: line[1],
      price: line[2]
      quantity: line[3]
    })
  }, {})

console.log('output', JSON.stringify(output, null, 2))

{{< /highlight >}}

## Lähteet

[^1]: https://www.youtube.com/watch?v=sjyJBL5fkp8
[^2]: https://www.youtube.com/watch?v=6sQDTgOqh-I 
[^3]: https://www.youtube.com/watch?v=BMUiFMZr7vk&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84&index=1

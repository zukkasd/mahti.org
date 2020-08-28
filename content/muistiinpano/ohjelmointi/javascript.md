---
title: "JavaScript"
author: "Greatman Lim"
muistiinpanoja: ["Ohjelmointi"]
---

Muistiinpanoja javascriptistä. Tämä on henkilökohtainen muistiinpano, eikä ole tarkoitettu opetusmateriaaliksi, mutta ehkä joku löytää tästä jotain hyödyllistä.

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

## Lähteet

[^1]: https://www.youtube.com/watch?v=sjyJBL5fkp8
[^2]: https://www.youtube.com/watch?v=6sQDTgOqh-I 


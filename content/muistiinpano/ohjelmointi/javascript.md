---
title: "JavaScript"
author: "Greatman Lim"
muistiinpanoja: ["Ohjelmointi"]
---

Muistiinpanoja javascriptistä. Tämä on henkilökohtainen muistiinpano, eikä ole tarkoitettu opetusmateriaaliksi, mutta ehkä joku löytää tästä jotain hyödyllistä.

## var, let vai ei mitään?

Lähes aina kannattaa käyttää joko `let` tai `const` muuttujia nimittäessä.

`var` näkyvyysalue vuotaa `for` lauseessa, joka on hieman outo ominaisuus, jos on ohjelmoinut muilla kielillä. `var` näkyvyys pysyy kuitenkin funktion sisällä. Jos muuttuja luodaan ilman avainsanaa, javascript pyrkii etsimään saman nimisen muuttujan ylemmästä hierarkiasta. Jos muuttujaa ei löydy, javascript luo sen ylimpään hierarkiaan, eli muuttujasta tulee globaali. Yleensä tämä ei ole haluttu ominaisuus, ja se voidaan estää kirjoittamalla javascript tiedoston ensimmäiseksi riviksi `"use strict";`.

https://www.youtube.com/watch?v=sjyJBL5fkp8

## Milloin puolipilkku on tarpeellinen?

Jos seuraava rivi alkaa sululla `(`, on ylempään riviin laitettava puolipiste. Muuten javascript luulee, että nämä kaksi ovat samaa riviä.



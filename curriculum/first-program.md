Ensimmäinen Quil ohjelma
===================================

Nyt tiedät hieman siitä, millaista Clojure ohjelmointi on. Katsotaanpa kuinka
tehdään itsenäinen applikaatio.

Ensimmäiseksi on tehtävä *projekti*. Opit organisoimaan projektiasi
*nimiavaruuksien* (namespace) avulla. Opit myös lisäämään projektiisi
*riippuvuuksia*. Lopuksi opit *kääntämään* applikaatiosi.

## Luo projekti

Tähän asti olet lähinnä käyttänyt REPL:iä. Valitettavasti kaikki tekemäsi
työ häviää REPL:n sulkeuduttua. Projektin voi ajatella koodisi pysyvänä
kotina. Tulet käyttämään työkalua nimeltä "Leiningen" projektisi luomiseen
ja hallintaan.

Luodaksesi uuden projektin, aja seuraava komento:

```clojure
lein new quil drawing
```

Tämä luo seuraavanlaisen hakemistorakenteen:

```
drawing
├── LICENSE
├── README.md
├── project.clj
└── src
    └── drawing
        └── core.clj
```
Tässä ei ole mitään ihmeellistä, saati erityisen Clojuremaista.
Kyseessä on Leiningenin konventio, tapa tehdä asioita. Käytät
Leiningeniä ohjelman kääntämiseen ja ajamiseen, ja Leiningen olettaa
applikaatiosi seuraavan tätä rakennetta. Hakemistot sisältävät
seuraavat asiat:

- `project.clj` sisältää asetuksia Leiningeniä varten. Asetukset auttavat
   Leiningeniä tietämään mitä riippuvuuksia sen pitää hakea, ja mikä funktio
   pitää suorittaa ohjelmaa ajettaessa.
- `src/drawing/core.clj` sisältää varsinaisen clojure-koodin.

Käytämme kirjastoa nimeltä [Quil](https://github.com/quil/quil). Quil:iä voi käyttää
piirtämiseen.

Piirretäänpä jotain ruudulle. Avaa Nightcode.
Klikkaa Import -> etsi hakemisto "drawing" ja klikkaa sitä. Avaa tiedosto `src/drawing/core.clj`

Alaoikealla:

1. klikkaa Run with REPL
2. klikkaa Reload File

Ensimmäinen vaihe voi kestää hetken. Kun näet kehotteen, `user=>`, alhaalla, klikkaa "Reload File":ä.

Näet ikkunan jonka sisällä kimpoilee ympyrä.

Voit sulkea ikkunen vasemmalla ylhäällä olevasta (X) painikkeesta.


## Muokkaa projektia

Luodaan toinen Quil luonnos. Valitse Nightcodessa drawing-projekti vasemmalta hakemistopuusta. Klikkaa "New File":ä
ylhäältä ikkunan oikealta puolelta.


![Create a new file](images/create-new-file.png)

Anna tiedoston nimeksi `lines.clj`.


## Organisointi

Ohjelmiesi muuttuessa monimutkaisemmiksi, ne tarvitsevat enemmän jäsennystä.
Clojure koodi organisoidaan sijoittamalla toisiinsa liittyvät funktiot ja 
data omiin tiedostoihinsa. Clojure olettaa, että jokainen tiedosto muodostaa
*nimiavaruuden*, joten se täytyy *määrittää* jokaisen tiedoston alussa.

Tähän asti nimiavaruuksista ei ole juuri tarvinnut välittää. Nimiavaruudet
mahdollistavat funktioiden nimeämisen ja määrittelyn ilman pelkoa siitä että
ne törmäävät toisiinsa. Voit esimerkiksi määrittää funktion `println` omaan
nimiavaruuteen nimeltä `my-special-namespace` ja se ei törmäisi Clojuren
sisäänrakennettuun `println` funktioon. Voit käyttää *täydellistä nimeä*
`my-special-namespace/println` erottaaksesi oman funktiosi sisäänrakennetusta 
`println` funktiosta.

Luo nimiavaruus tiedostoon `src/drawing/lines.clj`. Avaa se, ja kirjoita:

```clojure
(ns drawing.lines)
```

Tämä rivi riittää siihen, että kaikki tähän tiedostoon määritetyt asiat löytyvät 
jatkossa `drawing.lines` nimiavaruudesta.

Tallenna tiedosto painamalla Save painiketta ylä menusta.

## Riippuvuudet

Jäljellä on enää *riippuvuuksien* hallinta. Sitten olemme käyneet läpi tärkeimmät
koodi-projektin osat. Riippuvuudet ovat muiden kirjoittamia koodi-kirjastoja joita
voit käyttää hyväksesi omassa projektissasi.

Lisätäksesi riippuvuuden, avaa `project.clj`. Etsi seuraava kohta:


```clj
:dependencies [[org.clojure/clojure "1.8.0"]
               [quil "2.4.0"]])
```

Projektisi riippuvuudet määritellään tässä. Kaikki tarvitsemamme kirjastot
on jo lisätty riippuvuuksiksi.

Jotta voimme käyttää näitä kirjastoja, meidän täytyy ladata ne _require_ määreellä.
Tiedostossa `src/drawing/lines.clj` muuta aiemmin kirjoittamaasi ns-lauseketta seuraavasti:

```clojure
(ns drawing.lines
   (:require [quil.core :as q]))
```
Näin pääsemme käytämään tarvitsemaamme quil-kirjastoa.

`:require` määre `ns`:n sisällä käskee Clojuren ladata muita nimiavaruuksia.
`:as` osa määrettä luo *aliaksen* nimiavaruudelle, jotta voit viitata siihen
ilman että sinun täytyy kirjoittaa koko nimiavaruuden nimeä. Esimerkiksi
voit kirjoittaa `q/fill` sen sijaan että kirjoittaisit `quil.core/fill`.

Muista jälleen tallentaa tiedosto painamalla Save-painiketta ylä menusta.

## Ensimmäinen ohjelmasi

### Drawing with Quil

Quil is a Clojure library that provides the powers of [Processing](https://processing.org/), a
tool that allows you to create drawings and animations. We will use
the functions of Quil to create some of our own drawings.

We will define our own functions, like so...

```clojure
(defn draw []
   ; Do some things
   )
```

... that call functions that Quil provides, like so...

```clojure
   ; Call the quil background function
   (q/background 240)
```

Put it together:
```clojure
(defn draw []
   ; Call the quil background function
   (q/background 240)
   )
```

In order to create a drawing (or sketch in Quil lingo) with Quil, you
have to define the `setup`, `draw`, and `sketch` functions. `setup` is
where you set the stage for your drawing. `draw` happens repeatedly,
so that is where the action of your drawing happens. `sketch` is the
stage itself. Let's define these functions together, and you will see
what they do.

In Nightcode, in the lines.clj file, add the following after the
closing parenthesis of the ns statement from before.

```clojure
(defn setup []

  (q/frame-rate 30)

  (q/color-mode :rgb)

  (q/stroke 255 0 0))
```

This is the `setup` function that sets the stage for the
drawing. First, we call quil's `frame-rate` function to say that the
drawing should be redrawn 30 times per second. We put `q/` in front to
say that this is `frame-rate` from quil. Look up at the ns
statement. Since it says `:as q`, we can use q as a short hand for
quil, and `library-name/function-name` is the way you call a function
from a library.

Second, we set the color mode to RGB.

Third, we set the color of the lines we will draw with `stroke`. The
code 255 0 0 represents red. You can [look up RGB codes](http://xona.com/colorlist/) for other
colors if you would like to try something else.

In Nightcode, in the lines.clj file, add the following after the
closing parenthesis of the setup function.

```clojure
(defn draw []

  (q/line 0 0 (q/mouse-x) (q/mouse-y))

  (q/line 200 0 (q/mouse-x) (q/mouse-y))

  (q/line 0 200 (q/mouse-x) (q/mouse-y))

  (q/line 200 200 (q/mouse-x) (q/mouse-y)))
```

Here we call the quil `line` function four times. We also call two
functions repeatedly as the arguments to the `line` function:
`mouse-x` and `mouse-y`. These get the current position (x and y
coordinates on a 2d plane) of the mouse. The `line` function takes
four arguments - two sets of x, y coordinates. The first x and y are
the starting position of the line. The second x and y are the ending
position of the line. So we start each of these lines at a fixed
position, then end them wherever the mouse is when the sketch is
drawn.

```clojure
(q/defsketch hello-lines

  :title "You can see lines"

  :size [500 500]

  :setup setup

  :draw draw

  :features [:keep-on-top])
```

This is our sketch. You can set attributes of the sketch such as the
title and size. You also tell it what are the names of the setup and
draw functions. These have to match exactly the function names we used
above. The last line is to make our drawing app window keep on top
of everything else.

Now click - Run with REPL - Reload File - which evaluates the file.
Your drawing should appear.

If not, try - Save file - Stop - Run with REPL - Reload File.


### Exercise: Rainbow lines

Update your drawing so that:

* the lines are a different color
* the title is different
* the lines start at a different place

Bonus: Make each of the four lines a different color.

Bonus #2: Change the color of the lines based on the mouse position.

Hint: You can browse the [Quil API](http://quil.info/api) for ideas and function definitions.

Hint: You may think this helpful: the [Quil Cheatsheet](https://github.com/ClojureBridge/curriculum/blob/gh-pages/outline/cheatsheet-quil.md) to see selected APIs for ClojureBridge curriculum.

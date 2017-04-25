Ensimmäinen Quil ohjelma
===================================

Nyt tiedät hieman siitä, millaista Clojure ohjelmointi on. Katsotaanpa kuinka
tehdään itsenäinen sovellus.

Ensimmäiseksi on tehtävä *projekti*. Opit organisoimaan projektiasi
*nimiavaruuksien* (namespace) avulla. Opit myös lisäämään projektiisi
*riippuvuuksia*. Lopuksi opit *kääntämään* sovelluksesi.

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

Voit sulkea ikkunan ylhäällä olevasta (X) painikkeesta.


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

Tallenna tiedosto painamalla Save painiketta ylämenusta.

## Riippuvuudet

Jäljellä on enää *riippuvuuksien* hallinta. Sitten olemme käyneet läpi tärkeimmät
koodi-projektin osat. Riippuvuudet ovat muiden kirjoittamia koodikirjastoja, joita
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

### Piirtäminen Quil:n avulla

Quil on Clojure kirjasto joka mahdollistaa [Processing](https://processing.org/) työkalun
käyttämisen. Processing on työkalu, joka mahdollistaa piirtämisen ja animaatioiden tekemisen.
Käytämme Quil-kirjaston funktioita omissa piirroksissamme.

Määritämme funktiomme näin:

```clojure
(defn draw []
   ; Do some things
   )
```

... ja kutsumme quil:n funktiota näin:

```clojure
   ; Call the quil background function
   (q/background 240)
```

Kaikki yhdessä:
```clojure
(defn draw []
   ; Call the quil background function
   (q/background 240)
   )
```
Jotta voimme piirtää Quil:lla, tai tehdä luonnoksen (sketch) kuten Quil piirroksia kutsuu,
meidän täytyy määrittää `setup`, `draw`, ja `sketch` funktiot.
 `setup` funktio määrittää piirroksen asetukset. `draw` tapahtuu toistuvasti, joten
 sinne määritellään kaikki mitä piirroksessa tapahtuu. `sketch` on varsinainen luonnos.
 Määritellään nämä, niin näet mitä tapahtuu.
 
 Avaa Nightcodessa lines.clj tiedosto, ja lisää seuraava ns-määreen viimeisen sulun jälkeen:

```clojure
(defn setup []

  (q/frame-rate 30)

  (q/color-mode :rgb)

  (q/stroke 255 0 0))
```
Tämä on setup funktio joka määrittää asetuksia piirrokselle. Kutsumme ensin
quil:n `frame-rate` funktiota arvolla 30, joka aiheuttaa sen, että piirros päivittyy
30 kertaa sekunnissa. `q/`-osa funktiokutsussa kertoo funktion löytyvän quil:n nimiavaruudesta.
Voit katsoa ns-määrettä tiedoston alussa. Quil kirjaston alias on määritetty `:as q` kohdassa,
joten voimme käyttää q:ta quil-nimiavaruuden lyhenteenä. Kirjastojen funktioita kutsutaan
muodolla `kirjaston-nimi/funktion-nimi`, eli tässä tapauksessa `q/frame-rate`.

Sitten asetamme päälle RGB tilan.

Viimeiseksi asetamme piirrosvärin `stroke`-funktiolla. koodi 255 0 0 tarkoittaa punaista.
Voita katsella [muita mahdollisia värejä täältä](http://xona.com/colorlist/)

Nightcodessa lines.clj tiedostossa, lisää seuraava setup-funktion viimeisen sulkeen jälkeen:

```clojure
(defn draw []

  (q/line 0 0 (q/mouse-x) (q/mouse-y))

  (q/line 200 0 (q/mouse-x) (q/mouse-y))

  (q/line 0 200 (q/mouse-x) (q/mouse-y))

  (q/line 200 200 (q/mouse-x) (q/mouse-y)))
```

Kutsumme quil:n `line` funktiota neljä kertaa. Kutsumme myös kahta
muuta funktiota: `mouse-x` ja `mouse-y`. Funktiot palauttavat hiiren
osoittimen x ja y koordinaatit, ja funktioiden paluuarvo annetaan 
parametrina `line` funktiokutsuille.
`line` funktiolla on neljä argumenttia: ensimmäiset kaksi ovat janan
alkupisteen x- ja y-koordinaatit. Jälkimmäiset kaksi taas janan pääte 
koordinatit. Joten aloitamme janat tietystä määrätystä pisteestä, ja
päätämme ne sinne missä hiiren osoitin sattuu olemaan kun piirrosta 
päivitetään.

```clojure
(q/defsketch hello-lines

  :title "You can see lines"

  :size [500 500]

  :setup setup

  :draw draw

  :features [:keep-on-top])
```
Tämä on luonnoksemme. Voit asettaa ominaisuuksia kuten otsikko ja koko,
sekä määrittää setup ja draw funktiot. Näiden täytyy täsmätä juuri 
luomiemme funktioiden kanssa. Viimeinen rivi saa piirroksen ikkunan
pysymään aina päällimmäisenä.

Klikkaa Run with REPL painiketta, ja sen jälkeen Reload File painiketta.
Tämä evaluoi tiedoston, ja piirroksesi pitäisi ilmestyä näkyviin.

Jos näin ei käy, kokeile tallentaa tiedosto, painaa Stop painiketta, sitten 
Run with REPL painiketta ja viimeiseksi Reload File painiketta.

### Harjoitus: Sateenkaari viivat

Päivitä piirrostasi seuraavasti:

* Muuta viivojen väriä
* Muuta otsikkoa
* Laita viivat alkamaan eri paikasta.

Bonus: Tee viivoista keskenään eri värisiä
Bonus #2: Muuta viivojen väriä riippumaan hiiren osoittimen paikasta.

Vinkki: Voi katsoa [Quil:n dokumentaatiota](http://quil.info/api) saadaksesi vinkkejä ja ideoita.

Vinkki: Tämä voi olla hyödyksi:  [Quil Cheatsheet](https://github.com/Flowa/curriculum/blob/gh-pages/outline/cheatsheet-quil.md). Täältä löydät lyhyen selityksen käytetyistä funktioista.

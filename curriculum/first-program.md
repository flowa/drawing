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

Now let's go ahead and actually run the Quil sketch. Open up Nightcode
and Import - find the drawing folder and click. Open the file `src/drawing/core.clj`

On the bottom of the right side:

1. click Run with REPL
2. click Reload File

Run with REPL may take a while to startup. Once you see the prompt, `user=>`, on the bottom window, you can click Reload.

A window will pop up and a circle bouncing, hitting walls within.

You may close the pop up-ed window by clicking a close (X) icon on the top left.


## Modify Project

Let's create another Quil sketch. In Nightcode, select drawing on the left side of
directory tree. click New File on the top of right side window.


![Create a new file](images/create-new-file.png)

Enter `lines.clj` as the name.


## Organization

As your programs get more complex, you'll need to organize them. You
organize your Clojure code by placing related functions and data in
separate files. Clojure expects each file to correspond to a
*namespace*, so you must *declare* a namespace at the top of each
file.

Until now, you haven't really had to care about namespaces. Namespaces
allow you to define new functions and data structures without worrying
about whether the name you'd like is already taken. For example, you
could create a function named `println` within the custom namespace
`my-special-namespace`, and it would not interfere with Clojure's
built-in `println` function. You can use the *fully-qualified name*
`my-special-namespace/println` to distinguish your function from the
built-in `println`.

Create a namespace in the file `src/drawing/lines.clj`. Open it, and
type the following:

```clojure
(ns drawing.lines)
```

This line establishes that everything you define in this file will be
stored within the `drawing.lines` namespace.

Before going forward, click Save on the top menu bar.


## Dependencies

The final part of working with projects is managing their
*dependencies*. Dependencies are just code libraries that others have
written which you can incorporate in your own project.

To add a dependency, open `project.clj`. You should see a section
which reads

```clj
:dependencies [[org.clojure/clojure "1.8.0"]
               [quil "2.4.0"]])
```

This is where our dependencies are listed. All the dependencies we
need for this project are already included.

In order to use these libraries, we have to _require_ them in our own
project. In `src/drawing/lines.clj`, edit the ns statement you typed
before:

```clojure
(ns drawing.lines
   (:require [quil.core :as q]))
```

This gives us access to the library we will need to make our project.

There are a couple of things going on here. First, the `:require` in
`ns` tells Clojure to load other namespaces. The `:as` part of
`:require` creates an *alias* for a namespace, letting you refer to
its definitions without having to type out the entire namespace. For
example, you can use `q/fill` instead of `quil.core/fill`.

Before going forward, don't forget to save the file.
Click Save on the top menu bar when you change the code.


## Your first real program

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

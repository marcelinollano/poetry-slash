# Poetic Reviews

Generates poems in Spanish from Google Maps reviews. You can read some sample generated poems:

- [**Poemitas al Sol**](doc/poemitas-al-sol.md) ([PDF](doc/poemitas-al-sol.pdf)). Poems dedicated to Plaza Puerta del Sol in Madrid. A mix of positive and negative feelings about the center of Spain. Read live at [Libros mutantes](http://librosmutantes.com/poetry-slash/), April 2018.

  <img src="doc/poetry-slash.png" width="350px">

- [**Poemitas de la ciudad**](doc/poemitas-de-la-ciudad.md) ([PDF](doc/poemitas-de-la-ciudad.pdf)). Poems dedicated to: Plaza Puerta del Sol, Gran Via's Primark, Arahy Restaurant (Mariano Rajoy spent six hours here, it must be a terrific place) and Desperate Literature in Madrid. Read live at [Desperate Literature](https://desperateliterature.com), June 2018.

## Install and compose

Don't expect a great user experience on this project. It was a quick hack, but it is still very usable to me.

First you need to copy the `.env-sample` to `.env` and edit the file to include your [Google Cloud](https://cloud.google.com) credentials. You need to create a project that uses Google Cloud Language API.

I also did a translate option but the output was too weird in Spanish. If you want to use that you will also need the Google Cloud Translate API.

Then install the required RubyGems:

    bundle install

Then you can use a previous database generated by me:

    cp db/db-0009.sqlite3 db/db.sqlite3
    ./bin/compose

Or you can populate a new one with:

    ./bin/schema
    ./bin/import
    ./bin/compose

The import script will read from the `data` folder. Inside that folder you will find html files copied directly from Google Maps.

If you want to create your own just add another folder, paste the whole html page from a place with reviews. Then, you need to edit `./bin/import`, `./bin/compose` to include your new dataset and run:

    ./bin/import
    ./bin/compose

You can tweak a bunch of params inside `./bin/compose`:

```
name              = 'Puerta del sol'  # name of the place
type              = 'assonance'       # consonance | assonance
syllables_count   = 7                 # syllables per line
stanzas_size      = 4                 # lines per stanza
sentiment_score   = -1...1            # -1 | 1
sentiment_order   = 'downer'          # upper | downer
```

You can edit `./bin/compose` to use any of the properties to compose new poems: Syllables count, stressed syllables, sentiment, assonance and consonance rhymes.

When you run `./bin/compose` you get a poor man's distribution chart with the most repeated number of syllables. This helps you generate bigger stanzas that rhyme nicely.

```
Syllables

7    ================================================================
6    ==============================================================
10   ========================================================
12   ====================================================
3    ==================================================
11   =================================================
9    ===============================================
8    =============================================
4    ============================================
5    ===========================================
13   =======================================
16   =======================================
15   =====================================
18   ==================================
14   ==============================
17   ============================
19   =========================
20   =========================

…

----------------------------------------------------------------------

Stanzas

a|o

Muy bien comunicado.       Fermin Tubia                      0.9   0.9
Cualquier día del año…     jose esteban caraballo sarrion    0.1   0.1
Hay muchísimo ladrón.      Gambrinus                        -0.6   0.6
Mal cuidado y caro.        Fernando Martín Vivas            -0.7   0.7

i|a

Buena conectividad.        Graciela Lucchesi                 0.9   0.9
Es una preciosidad,        Gonzalo Muñoz Martínez            0.8   0.8
Hay mucha actividad,       Manlio Ivan Arguello Castillo     0.2   0.2
Cualquier hora del día,    jose esteban caraballo sarrion    0.0   0.0
```

### Credits

- https://github.com/vic/silabas.rb by [@vic](https://github.com/vic)
- https://github.com/gedera/humanize_numbers by [@gedera](https://github.com/gedera)

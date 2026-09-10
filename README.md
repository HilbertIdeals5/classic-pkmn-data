# Classic Pokémon Data

## What is this?

I made this dataset because I wanted to make it less overwhelming to wrap my head around Gen 1-2 postgame and completion. This dataset currently contains **all Pokémon encounter data**, and will later incorporate species data like evolutions and base stats.

The end result of my personal goals will be present on the web in the near future.

## How can I use it?

I recommend importing it into a proper database, like a SQL database, to make best use of it. To understand what each table's columns mean, reference [Schema.md](./Schema.md).

There is no license. The data itself is not mine, so please use it, rework it, and reshare it freely.

**NOTE:** All encounter data is oriented around the **international releases first**. There are two corrections you need to make manually if you want to use this data for the original Japanese releases:

- **Red & Green in-game trade:** give-Nidoran♂-get-Nidoran♀ in international, is give-Nidoran♀-get-Nidoran♂ in Japan. In other words, the Nidorans' genders are swapped.
- **Gold & Silver wild encounters:** for Gold, swap all Teddiursa and Ursaring for Phanpy and Donphan; for Silver, swap all Phanpy and Donphan for Teddiursa and Ursaring.

## Why did you make it?

There are other Pokémon datasets out there, but I think they're too overwhelming and I don't like the way they organize data. This dataset I assembled myself contains a focused set of information, organized in a sensible manner that's human-friendly and not too difficult to wrap your head around.

I'm certain there's many things about this dataset to dislike, so hopefully it'll also be easy to rework it to suit your needs and organizational preferences.

## How did you make it?

I used lots of Python to scrape data from the **pret decomps**. Here's the specific ones I used to get data from:

- Pokémon Red & Green (JP)/Blue (Int.): https://github.com/pret/pokered
- Pokémon Blue (JP): https://github.com/Narishma-gb/pokeblue
- Pokémon Yellow: https://github.com/pret/pokeyellow
- Pokémon Gold & Silver: https://github.com/pret/pokegold
- Pokémon Crystal: https://github.com/pret/pokecrystal

This was only the wild encounter data (the "`_wild.csv`" files). I completed the dataset by manually adding data for other encounter methods from **Bulbapedia**.

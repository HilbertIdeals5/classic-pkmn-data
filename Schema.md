# Schema Information

## Wild encounter data

### Common fields

There are many similar columns across every table. To avoid repetition, they'll all be listed right here, at the start.

- **game** (character): The single-letter abbreviation for a specific game. Possible values:
  - **Generation 1: (`rgby_`)**
    - R: Red
    - G: Green (JP)/Blue (Int.)
    - B: Blue (JP)
    - Y: Yellow
  - **Generation 2: (`gsc_`)**
    - G: Gold
    - S: Silver
    - C: Crystal
- **loc** (string): The name of a high-level location.
- **subloc** (string): The name of a low-level location, most commonly a floor of a building (B1F, 1F, 2F, etc.).
- **method** (string): Specifically a *wild* encounter method. Possible values:
  - Walk (whether in tall grass or throughout the floor of a cave)
  - Surf
  - Old Rod
  - Good Rod
  - Super Rod
  - Tree (Common)
  - Tree (Rare)
  - Rock
- **species** (string): A Pokémon name. All 251 names have been cleaned up to have a consistent spelling across every table, based on the spellings used in Bulbapedia.
- **level** (integer): The level of a specific encounter.

Mainly used in `_casino.csv`:
- **cost** (integer): The price in Coins of a Game Corner prize Pokémon.

Mainly used in `_special.csv`:
- **how** (string): A human-oriented explanation of a "special" encounter's unique encounter method. The most common ones are:
  - Encounter: interacting with the Pokémon in the overworld as opposed to via walking through tall grass or Surfing on water.
  - Choice: for example, choosing a starter. Bulbapedia calls these "mutually-exclusive Pokémon".
  - Gift: self-explanatory.
 
Mainly used in `_trade.csv`:
- **get** (string): The species you receive from an in-game trade.
- **give** (string): The species you must give to conduct an in-game trade.

Mainly used in `gsc_`:
- **held_item** (string): The held item of a Pokémon that you may receive or encounter through special means. It's only used in places where a Pokémon's held item is important to explicitly specify, i.e. where it's different from normal. That's why wild encounter held items are excluded—that information is more appropriate for a future per-species dataset.

#### The definition of an "encounter"

The wild encounter data in these tables is not organized the way it is on Bulbapedia. The pret decomp has revealed that the encounter data is actually *far* more granular than it appears in ordinary encounter tables.

For this dataset, think of each row as being an "encounter datapoint", and each column as a "dimension" of data that defines that highly-granular encounter datapoint. The data was also arranged this way to make search and manipulation easier, since it was originally made to be used via SQL.

### Generation 1 data

#### rgby_wild.csv

- **game**
- **loc**
- **subloc**
- **method**
- **species**
- **level**
- **rate_numerator** (integer): The numerator of an encounter rate.
- **rate_denominator** (integer): The denominator of an encounter rate.

Encounter rates are handled unusually in Generation 1. Instead of being out of 100, most encounter rates are out of various numbers, the most common one being 256. To preserve accuracy and dodge floating-point rounding errors, the numerator and denominator of an encounter rate are kept separate.

#### rgby_casino.csv

- **game**
- **species**
- **level**
- **cost**

#### rgby_special.csv

- **R** (boolean): 1 if the specific special encounter is available in Pokémon Red, 0 if not.
- **G** (boolean): Same as **R**, but for Pokémon Green in Japan, or Pokémon Blue internationally.
- **B** (boolean): Same as **R**, but for Pokémon Blue in Japan.
- **Y** (boolean): Same as **R**, but for Pokémon Yellow.
- **species**
- **level**
- **loc**
- **subloc**
- **how**

#### rgby_trade.csv

- **game**
- **loc**
- **get**
- **held_item**
- **give**

Even though this is a Generation 1 table, there is a **held_item** column. According to Bulbapedia, it turns out that there are a few in-game trade Pokémon with peculiar held items (only visible when transferred to Generation 2))—in particular, a Krabby that holds TM13 instead of TM33.

### Generation 2 data

#### gsc_wild.csv

- **game**
- **loc**
- **subloc**
- **method**
- **time** (character): A single-letter abbreviation for a time of day. Possible values:
  - M: Morning (from 4 AM to just before 10 AM)
  - D: Day (from 10 AM to just before 6 PM)
  - N: Night (from 6 PM to just before 4 AM)
  - *null*: All times of day, i.e. time of day does not matter for this encounter.
- **in_swarm** (boolean): Indicator of an encounter's availability in relation to swarms. Possible values:
  - 0: Outside of swarm
  - 1: Only *during* a swarm
  - *null*: Available at all times, ergo the location has no swarms whatsoever. Can be used to narrow down which locations have swarms at all. **Avoid relying on falsiness when programming**, otherwise 0 might get confused with *null* or vice-versa.
- **species**
- **level**
- **rate_pct_times_ten** (integer): The encounter rate as a number out of 1,000.

Why 1,000? Encounters are otherwise clean numbers out of 100, which is to say percentages; however, Surfing is handled unusually.

In the game's code, the lowest Surfing encounter rate is 10%, but only the *minimum* possible level is actually listed in the code. When a Surfing encounter is actually generated, to add level variation, there are random chances of adding 1, 2, 3, or 4 to that minimum level. The rarest is a 5% chance of adding 4 to the final level.

10% × 5% = 0.5%, meaning that the true lowest encounter rate would end up being a floating-point number. To avoid floating-point rounding errors, the "denominator" of every single rate was bumped up from 100 to 1000.

#### gsc_bugcatchingcontest.csv

- **rate** (integer): The encounter rate as a number out of 100.
- **species**
- **level_min** (integer): The minimum possible level of the encounter.
- **level_max** (integer): The maximum possible level of the encounter.

The Bug-Catching Contest encounter data is kept in a separate table for a few compelling reasons:

1. It's not a swarm. Best not to confuse it with one.
2. Encounter rates are, thankfully, handled completely differently. Every species has its own rate, and within a species, every level has an equal chance of being chosen. There's no reason to flatten this table to shoehorn it into the `gsc_wild.csv` table.

#### gsc_casino.csv

- **game**
- **loc**
- **species**
- **level**
- **cost**

**loc** is included here and not in Generation 1 because in Generation 1, the only Game Corner is the one in Celadon. In Generation 2, there is both the Celadon *and* the Goldenrod Game Corner.

#### gsc_special.csv

- **G** (boolean): 1 if the specific special encounter is available in Pokémon Gold, 0 if not.
- **S** (boolean): Same as **G**, but for Pokémon Silver.
- **C** (boolean): Same as **G**, but for Pokémon Crystal.
- **species**
- **level**
- **held_item**
- **loc**
- **subloc**
- **how**

#### gsc_trade.csv

- **G** (boolean): Same as **G** in `gsc_special.csv`.
- **S** (boolean): Same as **S** in `gsc_special.csv`.
- **C** (boolean): Same as **C** in `gsc_special.csv`.
- **loc**
- **get**
- **held_item**
- **give**

In Generation 1, almost every single game had a different set of in-game trades. In Generation 2, they are instead mostly the same, which is why there is no **game** column here like there is in Generation 1.

## Evolution data

This is all present exclusively in evolution.csv, which can be applied to both Generations.

- **from** (string): The Pokémon that is evolving (i.e. *before* the evolution).
- **to** (string): The Pokémon that will be evolved into (i.e. *after* the evolution).
- **method** (string): How does the Pokémon evolve? The overarching method.
- **condition** (string): The specific criterion within the evolution method, such as a level or item. 
- **additional** (string): The secondary criterion for triggering this evolution, for example time of day.

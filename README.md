# Psychrometrics Reference Guide

A plain-language reference for the properties of humid air, written for HVAC
technicians, mechanical engineers, and students who need a quick refresher
without digging through a textbook. This is a knowledge reference only — no
code, no software.

## Why psychrometrics matters

Psychrometrics is the study of moist air — how temperature and humidity
interact. It matters anywhere people or equipment care about comfort,
condensation, or moisture damage: HVAC sizing, duct design, data center
cooling, greenhouse climate control, drying processes, and building science
(preventing mold and condensation inside walls).

## The core properties

Given a **dry-bulb temperature** (the temperature a regular thermometer
reads) and one other known value, every other property below can be derived:

- **Relative humidity (RH)** — the ratio of actual water vapor pressure to
  the saturation vapor pressure at the same temperature, expressed as a
  percentage. 100% RH means the air is fully saturated.
- **Wet-bulb temperature** — the temperature a thermometer reads when its
  bulb is wrapped in a wet wick and air is moved past it. Evaporative
  cooling drops the reading below dry-bulb temperature unless the air is
  already saturated.
- **Dew point** — the temperature at which air, cooled at constant
  pressure, would become saturated and start condensing moisture. This is
  the number that tells you when a cold surface (a window, a duct, a cold
  drink) will start sweating.
- **Humidity ratio** (a.k.a. specific humidity) — the mass of water vapor
  per unit mass of dry air, usually expressed in grams per kilogram.
- **Enthalpy** — the total heat content of the air (sensible + latent),
  used to size cooling/heating loads.
- **Specific volume** — the volume occupied by a unit mass of dry air,
  used to convert between mass and volume airflow rates.

## Where the math comes from

The standard approach (used throughout HVAC engineering and in ASHRAE's
*Fundamentals* handbook) chains two published equations:

1. A **saturation vapor pressure equation** (commonly the Tetens
   approximation) gives the vapor pressure at which air of a given
   temperature would be fully saturated.
2. The **psychrometer equation** relates wet-bulb temperature to vapor
   pressure, dry-bulb temperature, and barometric pressure. Because it
   can't be solved in closed form for wet-bulb temperature, it's normally
   solved numerically (e.g. by bisection or iteration).

From there, relative humidity, dew point, humidity ratio, enthalpy, and
specific volume all follow from standard mass- and energy-balance
relationships. This repo describes the relationships conceptually; it
doesn't ship an implementation.

## Worked example (sea-level, 25°C dry-bulb, 50% RH)

This is the classic reference case used to sanity-check any psychrometric
calculation:

| Property | Value |
|---|---|
| Wet-bulb temperature | ≈ 17.9 °C |
| Dew point | ≈ 13.9 °C |
| Humidity ratio | ≈ 9.9 g water / kg dry air |
| Enthalpy | ≈ 50.3 kJ/kg |
| Specific volume | ≈ 0.858 m³/kg |

If your own numbers come out noticeably different from this reference case
at the same inputs, something upstream (units, pressure, equation choice)
is worth double-checking.

## Practical notes

- **Barometric pressure matters.** All the properties above are pressure-
  dependent. Sea-level (101.325 kPa) is the default most tools assume —
  at altitude, use the actual local barometric pressure or your numbers
  will be off.
- **Condensation risk** happens whenever a surface's temperature drops
  below the air's dew point — this is the single most useful number for
  diagnosing "sweating" ducts, windows, or pipes.
- **Mixing humid air streams** (e.g. return air + outside air in an HVAC
  system) is *not* a simple average of relative humidities — RH isn't a
  conserved quantity. Humidity ratio and enthalpy are the properties that
  mix linearly by mass, which is why load calculations use them instead of
  RH directly.

## Glossary

| Term | Meaning |
|---|---|
| Dry-bulb temperature | Ordinary air temperature, unaffected by humidity |
| Saturation | The point at which air holds the maximum water vapor possible at that temperature |
| Psychrometric chart | A graphical plot of all these properties against each other, used for quick lookups without calculation |

## Calculate it

Working through these by hand is useful for understanding, but tedious for
repeated use. For an interactive calculator that takes dry-bulb temperature
plus any one known humidity value and returns the rest instantly — including
a full psychrometric chart and CSV batch mode for processing many rows —
see **[psychrometriccalculator.com](https://psychrometriccalculator.com)**.

## References

- ASHRAE, *Fundamentals Handbook*, Chapter 1 (Psychrometrics)
- Full tool: https://psychrometriccalculator.com

## License

This reference text is released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — reuse freely with attribution.

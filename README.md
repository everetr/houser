
<!-- README.md is generated from README.Rmd. Please edit that file -->

# houser

The US Census Bureau conducts [quite a
few](https://www.census.gov/econ/construction.html) surveys of buildings
and housing in the US. None of this data is available in the [official
Census API](https://www.census.gov/data/developers/data-sets.html). This
package provides a convenient way to access housing data from the US
Census.

## Warning

Right now, `houser` can only download and clean **annual** files from
the Building Permits Survey. **Do not attempt to download other BPS
files using `houser`.** I have no idea what could happen. If you don’t
know what annual files are, see the “Data” section below before using
`houser`.

`get_bps()` will download annual files by default, so just don’t change
the `formats` argument in `get_bps()` and you’ll be fine. `clean_bps()`
currently only cleans annual files.

## Data

Right now, `houser` can only download and clean **annual** (a) files
from the [Building Permits
Survey](https://www.census.gov/construction/bps/), in [this
directory](https://www2.census.gov/econ/bps/). See the [BPS
documentation](https://www2.census.gov/econ/bps/Documentation/) for all
the info you’ll ever need. The following is an attempt at a summary.

BPS data are just a bunch of csv files, available for 4 geographies.
Geography is the unit of observation in all BPS files. For each
geography, files come in 5 possible formats for every year since 1980
(see “Formats” below), though not all formats are available for all
geographies. Place (municipality)-level files are organized by the
region of the US in which their places are located (see “Regions”
below).

These tables give you a lay of the land:

| Geography |                           Formats |        Regions |
| --------: | --------------------------------: | -------------: |
|     State |                           c, y, a |            N/A |
|    County |                           c, y, a |            N/A |
|       MSA |                           c, y, a |            N/A |
|     Place | monthly: c, y, r;<br>annual: a, r | mw, ne, so, we |

<img src="./inst/images/bps_years_available.png" width="100%" />

#### Formats

  - Monthly - “YYMM”
      - Current month - “c”
      - Year-to-date - “y”
      - Monthly cumulative - “r”
  - Annual - “YYYY”
      - Annual summary - “a”
      - Annual revised - “r”

#### Regions

*For place files only.*

  - Midwest - “mw”
  - Northeast - “ne”
  - South - “so”
  - [West](https://media.giphy.com/media/f8HlgFSGiCpkk/giphy.gif) - “we”

The name of the BPS file tells you everything you need to know about it.
They all follow the following template:

> \<geography or region\>\<YYMM or YYYY\>\<format\>.txt

The state-level annual file for 2017 is called `st2017a.txt`. The
county-level current month file for January 2016 is called
`co1601c.txt`. The MSA-level year-to-date file for December 2000 is
called `ma0012y.txt`. See the pattern?

Place files are special. The place-level monthly cumulative file for
February 1993 for the Northeast region is called `ne9302r.txt`. The
place-level annual summary file for 1990 for the West region is called
`we1990a.txt`.

### Data Documentation

#### Building Permits Survey

  - [All](https://www.census.gov/construction/bps/sample.html)
      - [Customer Information
        Package](https://www.census.gov/construction/bps/sample/customerinformationpackage2016withinstructions.pdf)
      - Dictionaries for each geography file
      - [Imputation
        procedure](https://www.census.gov/construction/bps/sample/impute.pdf)
  - See also: [HUD Building Permits Database help
    page](https://socds.huduser.gov/permits/help.htm)

## Installation

You can install the released version of houser from
[Github](https://www.github.com/everet/houser) with:

``` r
library(devtools)
devtools::install_github("everetr/houser")
```

The “master” branch, installed by default above, is the latest stable
release. The “devel” branch is the latest development version, still
being tested and therefore not recommended.

## Functions

  - `bps_get()` - Download BPS data for specified geography(ies),
    format(s), year(s), and region(s) (if applicable).
  - `bps_read()` - Load BPS data. Option to repair column names.
  - `bps_doc()` - Open BPS documentation for a specified geography in
    your system’s default PDF viewer.

## Usage

``` r
library(houser)

# Download annual data for one geography, two years.
bps_get(path = ".", geography = "state", years = c(2010, 2017))

# Downloading place data requires a `region` argument.
bps_get(path = ".", geography = "place", region = "ne", years = 2016:2017)

# If the `return_log_df` argument is TRUE, bps_get will download the data in 
# the background, as usual, AND return a data frame containing metadata for all
# files that were downloaded.
bps_log = bps_get(path = ".", 
                  geography = "place", region = "ne", years = 2016:2017,
                  return_log_df = T)
```

## Contributor Code of Conduct

Please note that the ‘houser’ project is released with a [Contributor
Code of Conduct](CODE_OF_CONDUCT.md). By contributing to this project,
you agree to abide by its terms.

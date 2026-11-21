# CS333 Part 3: Interactive Visualization of Dubai Real-Estate Transactions

Two interactive visualizations built with **Vega-Lite**, using a 10% sample of the Dubai Land Department real-estate transactions (`transactions.csv`) combined with the course's other Dubai datasets.

**Live page:** https://dianaqassab-stack.github.io/cs333-part3/

## What is in the repository

| File | What it is |
|---|---|
| `index.html` | The whole project: the data cleaning rules, both visualizations, and their written descriptions (encoding, interaction, value added). |
| `data_quality_checks.ipynb` | The notebook with the code for every data quality check listed on the page. |
| `README.md` | This file. |

## The visualizations

### Visualization 1: Metro stations and prices

A map of Dubai with the metro lines and one circle per station (circle size = number of sales, colour = median price per m²). Clicking a station filters a second view with two linked parts:

- a **box plot** of price per m² by number of rooms, with off-plan and existing sales side by side;
- a **bar chart** of the venues around that station, by category (from `metro_venues_total.csv`).

With nothing selected, the second view shows all metro stations (prices) and the average station (venues).

**What it shows:** property demand follows the metro, but prices vary a lot along it. For example, Burj Khalifa/Dubai Mall and Business Bay are among the busiest and most expensive stations, while stations at the ends of the lines are much cheaper but have far fewer venues around them.

### Visualization 2: Size, price and where the deals are

A scatter plot of property size against price per m² (both log scales, coloured off-plan vs existing). Dragging a box on the scatter selects an area, and a linked bar chart shows the **top 10 communities** with the most sales inside that box (bar length = number of sales, colour = median price per m²).

**What it shows:** how price per m² changes with size, whether off-plan sales are priced differently, and which communities hold the deals for a chosen size and price range.

The full encoding, interaction and value-added description for each visualization is on the live page, under each chart.

## How we handled data quality

All views use the same shared cleaning rules, so their numbers agree. In short:

- **Dates:** kept 1 Jan 2015 to 31 Aug 2026 (the same window as Parts 1 and 2); earlier and later dates were implausible or partial.
- **Transaction types:** price views use **sales only**, because mortgage and gift values are not market prices.
- **Prices:** price views keep sales between 1,000 and 100,000 AED/m².
- **Sizes:** price views keep properties of at least 15 m².
- **Missing rooms:** shown as "Not stated" instead of being dropped.
- **Metro stations:** spelling mistakes fixed; rows with no station are left out of the station views only; Dubai Tram stops are not matched to the metro map.
- **Small groups:** any group with fewer than 10 sales is shown in grey (or hidden), because its median is unreliable.
- **Scatter plot:** draws a random 10,000 of the 50,983 clean sales so it stays fast. The bar chart next to it still counts all sales.

The complete table of problems, fixes and affected row counts is at the top of the live page.

## Data sources

All data comes from the course repository, https://github.com/sharios/CS333/tree/main/2026-27, and is loaded directly by the page:

- `Part 2/transactions.csv`: the transactions (10% sample of Dubai Land Department records).
- `Part 2/dubai.geojson`: community boundaries and names, linked to the sales through `comm_num`.
- `Part 3/dubai_metro_stations.geojson`: the metro stations (2020 map).
- `Part 3/metro_venues_total.csv`: venues around each metro station, from Foursquare (collected around 2020).

Station names in the transactions were matched to the 2020 metro map by hand, because several stations have been renamed since.

## How to run it

The data loads from GitHub, so you only need an internet connection.

1. Download or clone this repository.
2. Open `index.html` in a browser (Chrome works well), or use the **Live Server** extension in VS Code.
3. Wait a few seconds for the data to load, then click a station on the map, or drag a box on the scatter plot.

Note: the charts will not load in sandboxed previews that block requests to other websites. Use a normal browser.

## Built with

- [Vega-Lite 6](https://vega.github.io/vega-lite/) and [Vega-Embed](https://github.com/vega/vega-embed), loaded from the jsDelivr CDN.
- Plain HTML, with no build step.
- Hosted with GitHub Pages.

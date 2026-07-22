# DATASET CATALOG — open-data-datasheets

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## What this is (and what it is not)

This is a **candidate backlog** of real, public open datasets that are plausible targets for an
Hee-Lee Oss documentation **datasheet** deliverable (data dictionary + provenance + license record +
Datasheet-for-Datasets + Croissant metadata + a small validation script). It exists to drive the
roadmap in `PLAN.md`: later milestones scale by triaging entries from this catalog into per-dataset
documentation tasks.

Three things are load-bearing and must not be misread:

1. **Candidate ≠ approved.** Every entry here is a *candidate*. No datasheet task is created from an
   entry until it passes the project's per-dataset **license + provenance + PII verification gate**
   (`open-data-datasheets-gate-002`, governed by the NC/share-alike policy `policy-022`). The
   `License`, `Permits derivatives?`, and `PII risk` columns below are a **first-pass triage signal**,
   not the gate result. Anything marked **"Verify"** must have its license confirmed (with a cited
   clause/URL and `license.permitsDerivatives: true`) before a task is created.
2. **The deliverable is documentation, never the data.** We do not host, mirror, transform, clean,
   or republish any dataset listed here. We describe it, link to it, and contribute metadata back to
   its portal/repo. Inspection of the underlying data follows the bounded access protocol in
   `PLAN.md` (header/streamed reads, ≤ 1,000-row sample, local-only, ephemeral, halt on PII signal).
3. **License discipline is the point.** Entries are deliberately biased toward sources with clear
   permissive terms that permit derivatives (UK Open Government Licence v3, CC0, CC-BY-4.0, US-gov
   public domain, Copernicus licence, French Licence Ouverte/Etalab). Share-alike (ODbL),
   non-commercial (NC), and custom/ambiguous terms are listed honestly and flagged so the gate can
   accept, escalate, or exclude them per `policy-022` — they are not silently treated as open.

**Column legend**

- `License` — best-available license identifier for the dataset's *data* (not the portal chrome).
  Where a portal hosts many datasets under per-dataset licenses, this notes the portal default and
  the column is qualified.
- `Permits derivatives?` — `Yes` (clear permissive license permitting derivative works);
  `Yes (share-alike)` (derivatives allowed but the derivative database must carry the same license,
  e.g. ODbL); `Verify` (terms unconfirmed or mixed/per-dataset — must be checked at the gate);
  `NC — excluded by policy` (non-commercial restriction; default-excluded pending `policy-022`).
- `PII risk` — `none` / `low` / `medium` / `high`. High-PII / person-level micro-data is flagged and
  **excluded unless the publisher documents adequate anonymization** (see PLAN PII methodology).
- IDs are stable: `cat-<domain>-NNN`.

---

## Domain 1 — Government budgets & spending

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-budget-001 | Spending over £25,000 (departmental transparency) | UK Government departments via data.gov.uk | gov-spending | OGL v3.0 | Yes | none | https://www.gov.uk/government/collections/spending-over-25-000 | Line-item public spending across UK central government; canonical OGL example, poorly documented per-department. |
| cat-budget-002 | USAspending.gov federal award & spending data | US Dept. of the Treasury (Bureau of the Fiscal Service) | gov-spending | US-gov public domain | Yes | none | https://www.usaspending.gov/download_center/custom_award_data | Full US federal award/obligation data; huge reuse value, machine-readable but thin field docs. |
| cat-budget-003 | EU Financial Transparency System (FTS) | European Commission (DG Budget) | gov-spending | Verify (Commission reuse / CC-BY-4.0) | Verify | low | https://ec.europa.eu/budget/financial-transparency-system/ | EU grant/contract recipients; reuse decision permits derivatives but per-record terms need confirming. |
| cat-budget-004 | Budget de l'État (PLF / execution data) | France — data.gouv.fr / DGFiP | gov-spending | Licence Ouverte / Etalab 2.0 | Yes | none | https://www.data.gouv.fr/ | National budget data under an explicitly derivative-permitting open licence; good non-anglophone exemplar. |
| cat-budget-005 | BOOST public expenditure database | World Bank | gov-spending | CC-BY-4.0 | Yes | none | https://www.worldbank.org/en/programs/boost-portal | Harmonized cross-country budget data; CC-BY confirmed at World Bank data-licensing page. |

## Domain 2 — Census & demographics

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-census-001 | American Community Survey (ACS) summary tables | US Census Bureau | demographics | US-gov public domain | Yes | low | https://www.census.gov/programs-surveys/acs/data.html | Workhorse US demographic data (aggregated tables); document tables, not the PUMS micro-data. |
| cat-census-002 | Census 2021 (England & Wales) | UK Office for National Statistics (ONS) | demographics | OGL v3.0 | Yes | none | https://www.ons.gov.uk/census | Aggregated census tables; OGL, but field semantics and geography levels are confusing to reusers. |
| cat-census-003 | Population & demography statistics | Eurostat | demographics | CC-BY-4.0 | Yes | none | https://ec.europa.eu/eurostat/web/population-demography | Pan-EU comparable demography; Eurostat moved to CC-BY-4.0 (copyright notice). |
| cat-census-004 | World Population Prospects (WPP) | UN DESA, Population Division | demographics | CC-BY-3.0 IGO | Yes | none | https://population.un.org/wpp/ | Authoritative global population estimates/projections; permissive IGO licence. |
| cat-census-005 | WorldPop gridded population | WorldPop, Univ. of Southampton | demographics | CC-BY-4.0 | Yes | low | https://www.worldpop.org/ | High-res gridded population rasters; CC-BY, low PII (modelled, not person-level). |
| cat-census-006 | IPUMS census micro-data (international/USA) | IPUMS, Univ. of Minnesota | demographics | Custom + registration | Verify | high | https://www.ipums.org/ | **Excluded candidate** — person-level micro-records behind registration/redistribution terms; listed to mark the exclusion boundary, not to document. |

## Domain 3 — Public health

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-health-001 | CDC WONDER (mortality, natality, etc.) | US Centers for Disease Control and Prevention | public-health | US-gov public domain | Yes | low | https://wonder.cdc.gov/ | Core US public-health statistics; small-count suppression already applied by publisher. |
| cat-health-002 | Our World in Data — health datasets | Our World in Data (Global Change Data Lab) | public-health | CC-BY-4.0 | Yes | none | https://ourworldindata.org/ | OWID's *own* compiled data is CC-BY; third-party source layers must be checked per series. |
| cat-health-003 | NHS England published statistics | NHS England | public-health | OGL v3.0 | Yes | none | https://www.england.nhs.uk/statistics/ | Aggregated health-service stats under OGL; documentation quality varies widely by series. |
| cat-health-004 | Global Health Observatory (GHO) | World Health Organization | public-health | CC-BY-NC-SA 3.0 IGO | NC — excluded by policy | none | https://www.who.int/data/gho | High-value global indicators but NC+SA; default-excluded pending `policy-022`. Listed to show the NC boundary. |
| cat-health-005 | Global Burden of Disease (GBD) results | IHME, Univ. of Washington | public-health | Verify (free-of-charge / NC terms) | Verify | low | https://www.healthdata.org/research-analysis/gbd | Definitive disease-burden estimates; IHME's use terms restrict some reuse — confirm before any task. |
| cat-health-006 | ECDC surveillance & disease data | European Centre for Disease Prevention and Control | public-health | Verify (CC-BY / EU reuse) | Verify | none | https://www.ecdc.europa.eu/en/data | EU communicable-disease surveillance; reuse generally open but per-dataset terms need confirming. |

## Domain 4 — Environment & climate

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-climate-001 | Global Historical Climatology Network (GHCN) | NOAA NCEI | climate | US-gov public domain | Yes | none | https://www.ncei.noaa.gov/products/land-based-station/global-historical-climatology-network-daily | Backbone of global temperature/precip records; station-level docs are thin for non-experts. |
| cat-climate-002 | ERA5 reanalysis | Copernicus Climate Change Service (C3S) / ECMWF | climate | Copernicus licence | Yes | none | https://cds.climate.copernicus.eu/ | Flagship reanalysis; Copernicus licence permits derivatives + commercial use with attribution. |
| cat-climate-003 | GISTEMP surface temperature analysis | NASA GISS | climate | US-gov public domain | Yes | none | https://data.giss.nasa.gov/gistemp/ | Widely cited global-temperature index; reproducibility benefits from a formal datasheet. |
| cat-climate-004 | CO2 & Greenhouse Gas Emissions | Our World in Data | climate | CC-BY-4.0 | Yes | none | https://github.com/owid/co2-data | Tidy, citable emissions dataset; CC-BY on OWID-compiled data. |
| cat-climate-005 | Global Carbon Budget | Global Carbon Project | climate | CC-BY-4.0 | Yes | none | https://www.globalcarbonproject.org/carbonbudget/ | Annual authoritative carbon-budget release; complex provenance ideal for a datasheet. |

## Domain 5 — Air quality

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-airq-001 | OpenAQ global air-quality measurements | OpenAQ | air-quality | CC-BY-4.0 | Yes | none | https://openaq.org/ | Aggregated global station readings; CC-BY, strong civic-tech reuse, uneven station metadata. |
| cat-airq-002 | Air Quality System (AQS) | US EPA | air-quality | US-gov public domain | Yes | none | https://www.epa.gov/aqs | Authoritative US monitor data; rich but documentation-heavy parameter codes. |
| cat-airq-003 | European air-quality data | European Environment Agency (EEA) | air-quality | Verify (EEA reuse policy) | Verify | none | https://www.eea.europa.eu/en/datahub | EEA reuse policy is broadly open with attribution; confirm per-dataset before a task. |
| cat-airq-004 | WHO Ambient Air Quality Database | World Health Organization | air-quality | CC-BY-NC-SA 3.0 IGO | NC — excluded by policy | none | https://www.who.int/data/gho/data/themes/air-pollution | Global city PM2.5/PM10 compilation but NC+SA; default-excluded pending `policy-022`. |
| cat-airq-005 | CAMS atmosphere monitoring (reanalysis/forecast) | Copernicus Atmosphere Monitoring Service / ECMWF | air-quality | Copernicus licence | Yes | none | https://atmosphere.copernicus.eu/data | Modelled global air-composition fields; Copernicus licence permits derivatives. |

## Domain 6 — Energy

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-energy-001 | EIA energy data (Open Data API) | US Energy Information Administration | energy | US-gov public domain | Yes | none | https://www.eia.gov/opendata/ | Comprehensive US energy production/consumption/price series; thin per-series docs. |
| cat-energy-002 | ENTSO-E Transparency Platform | ENTSO-E | energy | Verify (custom platform terms) | Verify | none | https://transparency.entsoe.eu/ | European electricity generation/load/flows; custom terms must be confirmed before a task. |
| cat-energy-003 | Energy datasets | Our World in Data | energy | CC-BY-4.0 | Yes | none | https://github.com/owid/energy-data | Tidy global energy dataset; CC-BY on OWID-compiled data. |
| cat-energy-004 | UK energy statistics (DUKES etc.) | UK DESNZ via data.gov.uk | energy | OGL v3.0 | Yes | none | https://www.gov.uk/government/collections/digest-of-uk-energy-statistics-dukes | National energy balances under OGL; reusers struggle with unit/category definitions. |
| cat-energy-005 | Ember electricity & power-sector data | Ember | energy | CC-BY-4.0 | Yes | none | https://ember-energy.org/data/ | Widely reused global electricity dataset; CC-BY, clear attribution model. |

## Domain 7 — Transport & mobility

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-transport-001 | GTFS transit feeds (catalog) | The Mobility Database / individual transit agencies | transport | Verify (per-feed) | Verify | none | https://mobilitydatabase.org/ | Thousands of agency feeds with mixed licences (CC0/CC-BY/custom); each must be verified per feed. |
| cat-transport-002 | Road Safety Data (STATS19) | UK Department for Transport | transport | OGL v3.0 | Yes | low | https://www.data.gov.uk/dataset/road-accidents-safety-data | Collision records, publisher-anonymised to street level; low residual PII, document carefully. |
| cat-transport-003 | Fatality Analysis Reporting System (FARS) | US NHTSA | transport | US-gov public domain | Yes | medium | https://www.nhtsa.gov/research-data/fatality-analysis-reporting-system-fars | Census of US fatal crashes; de-identified but sensitive person-level context — gate scrutiny required. |
| cat-transport-004 | TLC Trip Record Data | NYC Taxi & Limousine Commission | transport | Verify (NYC Open Data terms) | Verify | medium | https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page | Famous re-identification cautionary tale; custom terms + linkage risk → strict gate review. |
| cat-transport-005 | Transport statistics | Eurostat | transport | CC-BY-4.0 | Yes | none | https://ec.europa.eu/eurostat/web/transport | Comparable EU transport indicators; CC-BY-4.0. |

## Domain 8 — Education

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-edu-001 | College Scorecard | US Department of Education | education | US-gov public domain | Yes | none | https://collegescorecard.ed.gov/data/ | Institution-level outcomes/cost; widely reused, dense data dictionary worth restating cleanly. |
| cat-edu-002 | UIS education statistics | UNESCO Institute for Statistics | education | Verify (UIS terms / CC-BY IGO) | Verify | none | https://uis.unesco.org/ | Global comparable education indicators; confirm reuse terms before a task. |
| cat-edu-003 | EdStats education indicators | World Bank | education | CC-BY-4.0 | Yes | none | https://databank.worldbank.org/source/education-statistics | Cross-country education data; CC-BY per World Bank licensing. |
| cat-edu-004 | PISA assessment data | OECD | education | Verify (OECD terms) | Verify | low | https://www.oecd.org/pisa/data/ | Influential learning-outcomes data; OECD custom terms must be confirmed. |
| cat-edu-005 | Get Information about Schools (GIAS) | UK Department for Education | education | OGL v3.0 | Yes | none | https://www.get-information-schools.service.gov.uk/ | Authoritative UK schools register; OGL, useful reference dataset with weak machine-readable docs. |

## Domain 9 — Justice & crime

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-justice-001 | Police recorded crime (street-level) | UK Home Office / police forces via data.police.uk | justice | OGL v3.0 | Yes | low | https://data.police.uk/ | Street-level crime + outcomes, publisher-anonymised (snapped to points); low residual PII. |
| cat-justice-002 | Crime Data Explorer (UCR/NIBRS) | US FBI | justice | US-gov public domain | Yes | none | https://cde.ucr.cjis.gov/ | National US crime statistics (aggregated); complex offense taxonomies need clear docs. |
| cat-justice-003 | Bureau of Justice Statistics data | US Bureau of Justice Statistics | justice | US-gov public domain | Yes | none | https://bjs.ojp.gov/ | Corrections/courts/victimization statistics; aggregated, public domain. |
| cat-justice-004 | Crime & criminal justice statistics | Eurostat | justice | CC-BY-4.0 | Yes | none | https://ec.europa.eu/eurostat/web/crime | Comparable EU crime stats; CC-BY-4.0. |
| cat-justice-005 | Stanford Open Policing Project | Stanford Computational Policy Lab | justice | Verify (project terms) | Verify | medium | https://openpolicing.stanford.edu/data/ | Standardized traffic-stop records; de-identified but person-adjacent → gate scrutiny + terms check. |

## Domain 10 — Geospatial & boundaries

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-geo-001 | OpenStreetMap planet data | OpenStreetMap contributors / OSMF | geospatial | ODbL 1.0 | Yes (share-alike) | low | https://planet.openstreetmap.org/ | The open base map; ODbL share-alike means derivative *databases* inherit ODbL — flag for `policy-022`. |
| cat-geo-002 | Natural Earth vector/raster | Natural Earth | geospatial | Public domain | Yes | none | https://www.naturalearthdata.com/ | Cartographer-friendly global boundaries; public domain, ideal low-risk pilot candidate. |
| cat-geo-003 | GADM administrative areas | GADM | geospatial | Custom (academic/non-commercial) | NC — excluded by policy | none | https://gadm.org/license.html | Detailed admin boundaries but terms forbid redistribution/commercial use → default-excluded. |
| cat-geo-004 | TIGER/Line shapefiles | US Census Bureau | geospatial | US-gov public domain | Yes | none | https://www.census.gov/geographies/mapping-files.html | Authoritative US geographies; public domain, heavily reused, dense field codes. |
| cat-geo-005 | geoBoundaries global admin boundaries | William & Mary geoLab | geospatial | CC-BY-4.0 | Yes | none | https://www.geoboundaries.org/ | Open, attribution-only alternative to GADM; explicitly built for redistribution. |

## Domain 11 — Economy & trade

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-econ-001 | World Development Indicators (WDI) | World Bank | economy | CC-BY-4.0 | Yes | none | https://databank.worldbank.org/source/world-development-indicators | The canonical cross-country development dataset; CC-BY confirmed. |
| cat-econ-002 | IMF macroeconomic databases (IFS/WEO etc.) | International Monetary Fund | economy | Verify (IMF terms) | Verify | none | https://data.imf.org/ | Core macro data; IMF terms are mixed/custom — confirm before a task. |
| cat-econ-003 | UN Comtrade international trade | UN Statistics Division | economy | Verify (Comtrade terms) | Verify | none | https://comtradeplus.un.org/ | Bilateral trade flows; custom usage/redistribution terms must be checked. |
| cat-econ-004 | National accounts & macro indicators | Eurostat | economy | CC-BY-4.0 | Yes | none | https://ec.europa.eu/eurostat/web/national-accounts | Comparable EU macro data; CC-BY-4.0. |
| cat-econ-005 | Labor statistics (CPI, employment) | US Bureau of Labor Statistics | economy | US-gov public domain | Yes | none | https://www.bls.gov/data/ | Foundational US economic series; public domain, complex series IDs. |

## Domain 12 — Agriculture & food

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-agri-001 | FAOSTAT | FAO (UN Food and Agriculture Organization) | agriculture | CC-BY-4.0 IGO | Yes | none | https://www.fao.org/faostat/ | Global food & agriculture statistics; FAO relicensed to CC-BY-4.0 IGO (2020). |
| cat-agri-002 | NASS Quick Stats | USDA National Agricultural Statistics Service | agriculture | US-gov public domain | Yes | none | https://quickstats.nass.usda.gov/ | Granular US ag census/survey data; public domain, intimidating dimensionality. |
| cat-agri-003 | FoodData Central | USDA Agricultural Research Service | agriculture | US-gov public domain (CC0) | Yes | none | https://fdc.nal.usda.gov/ | Authoritative food-composition database; public domain, widely reused in nutrition apps. |
| cat-agri-004 | Open Food Facts | Open Food Facts | agriculture | ODbL 1.0 (data) | Yes (share-alike) | none | https://world.openfoodfacts.org/data | Crowdsourced global food-product database; ODbL share-alike → flag for `policy-022`. |
| cat-agri-005 | EU agri-food data portal | European Commission (DG AGRI) | agriculture | Verify (CC-BY / EU reuse) | Verify | none | https://agriculture.ec.europa.eu/data-and-analysis_en | CAP/market data; generally open but per-dataset terms must be confirmed. |

## Domain 13 — Housing

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-housing-001 | Price Paid Data (property transactions) | UK HM Land Registry | housing | OGL v3.0 | Yes | low | https://www.gov.uk/government/statistical-data-sets/price-paid-data-downloads | Every England/Wales property sale; addresses are public-register data — low but non-zero PII context. |
| cat-housing-002 | HUD open data (housing programs) | US Dept. of Housing and Urban Development | housing | US-gov public domain | Yes | none | https://hudgis-hud.opendata.arcgis.com/ | Housing-program and geography data; public domain, useful for civic analysis. |
| cat-housing-003 | HMDA mortgage data | US CFPB / FFIEC | housing | US-gov public domain | Yes | medium | https://ffiec.cfpb.gov/data-browser/ | Loan-level mortgage records, publisher-modified for privacy; gate must confirm anonymization adequacy. |
| cat-housing-004 | UK House Price Index | UK ONS / HM Land Registry | housing | OGL v3.0 | Yes | none | https://www.gov.uk/government/collections/uk-house-price-index-reports | Aggregated price-index series; OGL, methodology benefits from a clear datasheet. |
| cat-housing-005 | Housing statistics | Eurostat | housing | CC-BY-4.0 | Yes | none | https://ec.europa.eu/eurostat/web/income-and-living-conditions/data | Comparable EU housing-cost/overcrowding indicators; CC-BY-4.0. |

## Domain 14 — Elections

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-elections-001 | US election returns (precinct/county/state) | MIT Election Data + Science Lab (MEDSL) | elections | CC-BY-4.0 | Yes | none | https://electionlab.mit.edu/data | Cleaned, citable US election results; mostly CC-BY (confirm per dataset, some CC0). |
| cat-elections-002 | UK election results | UK Electoral Commission | elections | OGL v3.0 | Yes | none | https://www.electoralcommission.org.uk/research-reports-and-data | Official UK results/registration data under OGL. |
| cat-elections-003 | FEC campaign-finance data | US Federal Election Commission | elections | US-gov public domain | Yes | medium | https://www.fec.gov/data/ | Donor-level records are public by law but contain names/addresses — flag for targeting/abuse review. |
| cat-elections-004 | OpenElections results | OpenElections (community) | elections | Verify (per-state repo) | Verify | none | https://openelections.net/ | Volunteer-standardised US results; licence varies by state repo — verify each. |
| cat-elections-005 | EU / European Parliament election data | European Parliament via data.europa.eu | elections | Verify (CC-BY / per-dataset) | Verify | none | https://data.europa.eu/ | Portal editorial content is CC-BY-4.0 and metadata CC0, but each dataset's licence must be checked. |

## Domain 15 — Science (biodiversity, astronomy, genomics)

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-science-001 | GBIF species occurrence data | Global Biodiversity Information Facility | biodiversity | Per-dataset (CC0 / CC-BY / CC-BY-NC) | Verify | low | https://www.gbif.org/ | Aggregated downloads mix CC0/CC-BY/CC-BY-NC; filter to CC0/CC-BY and watch sensitive-species locations. |
| cat-science-002 | NASA Exoplanet Archive | NASA / IPAC, Caltech | astronomy | US-gov public domain | Yes | none | https://exoplanetarchive.ipac.caltech.edu/ | Authoritative confirmed-exoplanet catalog; public domain, intricate column semantics. |
| cat-science-003 | Sloan Digital Sky Survey (SDSS) | SDSS Collaboration | astronomy | Verify (CC-BY / acknowledgement) | Verify | none | https://www.sdss.org/ | Vast astronomical survey; reuse generally open with acknowledgement — confirm exact terms. |
| cat-science-004 | Ensembl genome annotation | EMBL-EBI / Ensembl | genomics | Open (no restriction; Apache-2.0 code) | Yes | none | https://www.ensembl.org/info/about/legal/ | Reference genome annotations; open terms, dense file formats reusers struggle with. |
| cat-science-005 | GenBank sequence database | US NCBI / NIH | genomics | US-gov public domain | Yes | low | https://www.ncbi.nlm.nih.gov/genbank/ | Reference sequences are public domain; exclude consented human-subject genomic micro-data. |
| cat-science-006 | Gaia mission astrometry | ESA / DPAC | astronomy | CC-BY-4.0 | Yes | none | https://www.cosmos.esa.int/web/gaia/data-access | Billion-star catalog; CC-BY-style ESA terms, famously hard to use without a clear datasheet. |

## Domain 16 — Humanitarian

| ID | Dataset | Publisher | Domain | License | Permits derivatives? | PII risk | Access (URL) | Why it matters |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| cat-humanitarian-001 | Humanitarian Data Exchange (HDX) datasets | UN OCHA Centre for Humanitarian Data | humanitarian | Per-dataset (CC-BY / CC-BY-IGO / CC-BY-SA / ODbL) | Verify | varies | https://data.humdata.org/ | Each dataset carries its own licence; OCHA screens for sensitive data, but PII risk must be re-checked per dataset. |
| cat-humanitarian-002 | ACLED conflict events | Armed Conflict Location & Event Data | humanitarian | Verify (ACLED terms of use) | Verify | low | https://acleddata.com/ | Standard conflict-event dataset; custom attribution/usage terms restrict some reuse — confirm first. |
| cat-humanitarian-003 | Refugee population statistics | UNHCR | humanitarian | Verify (CC-BY / UNHCR terms) | Verify | low | https://www.unhcr.org/refugee-statistics/ | Aggregated forced-displacement statistics; generally open with attribution — confirm exact terms. |
| cat-humanitarian-004 | INFORM Risk Index | EU JRC / IASC | humanitarian | Verify (CC-BY / EU reuse) | Verify | none | https://drmkc.jrc.ec.europa.eu/inform-index | Composite crisis-risk index; JRC reuse generally open — confirm per release. |
| cat-humanitarian-005 | ReliefWeb data (disasters, reports) | UN OCHA | humanitarian | Verify (ReliefWeb terms) | Verify | low | https://reliefweb.int/ | Disaster/situation metadata via API; terms and per-content licences must be confirmed. |
| cat-humanitarian-006 | Displacement Tracking Matrix (DTM) | IOM (UN Migration) | humanitarian | Verify (DTM/IOM terms) | Verify | medium | https://dtm.iom.int/ | Displacement-site data can be locationally sensitive; verify licence and PII/anonymization before any task. |

---

## Catalog summary (first-pass triage signal, not gate results)

- **Total candidates:** 84 datasets across **16 domains**.
- **Clearly permissive** (OGL v3, CC0, CC-BY-4.0 incl. IGO, US-gov public domain, Copernicus
  licence, Licence Ouverte/Etalab, CC-BY-3.0 IGO): **~55**.
- **Share-alike** (ODbL — derivatives allowed but inherit the licence; routed to `policy-022`):
  **2** (`cat-geo-001` OpenStreetMap, `cat-agri-004` Open Food Facts).
- **Excluded / flagged by default** (NC restriction, custom non-redistribution, or high-PII
  micro-data): **4** — `cat-health-004` (WHO GHO, NC), `cat-airq-004` (WHO Air Quality DB, NC),
  `cat-geo-003` (GADM, non-redistribution), `cat-census-006` (IPUMS, high-PII micro-data).
- **Verify before any task** (mixed/per-dataset/custom or unconfirmed terms): **~23**.

These counts are *triage hints*. The authoritative accept/exclude decision is the per-dataset gate
artifact produced by `open-data-datasheets-gate-002` under `policy-022`.

## How to turn a catalog entry into a task

Each accepted catalog row (after passing the gate) becomes one Hee-Lee Oss **Task JSON**
(`packages/schema/src/schemas.ts`). Column → field mapping:

| Catalog column | Task JSON field(s) | Notes |
| --- | --- | --- |
| `ID` (`cat-…`) | informs `id` (e.g. `open-data-datasheets-doc-<dataset>`) | The `cat-…` ID is recorded in `resources`/`context` for traceability; the task gets a project-scheme ID. |
| `Dataset` | `title`, `context` | Task title references the dataset name. |
| `Publisher` | `context`, provenance `attribution` | Drives the required attribution string. |
| `Domain` | `domain[]` | e.g. `["public-data", "climate", "open-science"]`. |
| `License` | `outputLicense` is **always** `CC-BY-4.0` (docs) / `MIT` (code); the *source* licence goes in the canonical-metadata `license.id` + gate artifact, **not** `outputLicense`. | Do not confuse the source dataset licence with our output licence. |
| `Permits derivatives?` | gate gate-result + `license.permitsDerivatives` (canonical model) | Must be `true` with cited evidence before the task is created; `Verify`/NC/SA rows go through `policy-022` first. |
| `PII risk` | `riskTier` + gate PII artifact | `none`/`low` → `riskTier: low`; `medium` or sensitive-domain → `riskTier: medium`; high-PII micro-data is excluded, not tasked. |
| `Access (URL)` | `resources[]`, provenance `sourceUrl` | Source URL captured in provenance + license snapshot. |
| `Why it matters` | `context`, `objective` | Frames the beneficiary value. |

Constant fields for every catalog-derived task: `project: "open-data-datasheets"`, `lane: "donated"`,
`type: "data"`, `deliverable: "document"` (never `dataset`), `urgent: false`, `status: "open"`,
`verifiedNeed: false` until a named portal/maintainer confirms acceptance (per PLAN), `requestor:
"TO BE SECURED"` likewise.

## License sources (verified)

Licences flagged `Yes`/share-alike/NC above were checked against these primary sources; rows marked
`Verify` are explicitly *unconfirmed* and must be verified at the gate (do not assume).

- UK Open Government Licence v3.0 — https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/
- US federal works (public domain, 17 U.S.C. §105) — https://www.usa.gov/government-works
- Eurostat copyright / CC-BY-4.0 — https://ec.europa.eu/eurostat/help/copyright-notice
- data.europa.eu legal notice (editorial CC-BY-4.0; metadata CC0; per-dataset licences vary) — https://data.europa.eu/en/legal-notice
- World Bank data licensing (CC-BY-4.0) — https://datacatalog.worldbank.org/public-licenses and https://data.worldbank.org/summary-terms-of-use
- Our World in Data licensing (CC-BY-4.0; third-party sources vary) — https://ourworldindata.org/faqs#how-can-i-use-or-cite-your-work
- FAO / FAOSTAT (CC-BY-4.0 IGO) — https://www.fao.org/contact-us/terms/db-terms-of-use/en/
- Copernicus licence (free reuse incl. derivatives + commercial, attribution) — https://www.copernicus.eu/en/access-data/copyright-and-licences
- French Licence Ouverte / Etalab 2.0 — https://www.etalab.gouv.fr/licence-ouverte-open-licence/
- GBIF data-use licences (CC0 / CC-BY / CC-BY-NC per dataset) — https://www.gbif.org/terms and https://docs.gbif.org/gbif-data-licensing/en/
- OpenStreetMap ODbL (share-alike) — https://www.openstreetmap.org/copyright and https://opendatacommons.org/licenses/odbl/
- Open Food Facts (ODbL data licence) — https://world.openfoodfacts.org/terms-of-use
- WHO data terms (CC-BY-NC-SA 3.0 IGO for GHO) — https://www.who.int/about/policies/publishing/data-policy
- GADM licence (academic/non-commercial; no redistribution) — https://gadm.org/license.html
- NYC Open Data terms of use — https://opendata.cityofnewyork.us/overview/ (Terms of Use)
- Humanitarian Data Exchange licences (per-dataset CC-BY / CC-BY-IGO / CC-BY-SA / ODbL) — https://data.humdata.org/faqs/licenses
- geoBoundaries (CC-BY-4.0) — https://www.geoboundaries.org/index.html#usage
- UN DESA World Population Prospects (CC-BY-3.0 IGO) — https://population.un.org/wpp/ (terms of use)

*Unverified at time of cataloging (marked `Verify` above), to be confirmed at the gate:* EU FTS,
ENTSO-E, IMF, UN Comtrade, OECD/PISA, UNESCO UIS, EEA air quality, ECDC, IHME GBD, GTFS per-feed,
Stanford Open Policing, SDSS, OpenElections, EU/EP election datasets, EU agri-food datasets, ACLED,
UNHCR, INFORM, ReliefWeb, IOM DTM, IPUMS.

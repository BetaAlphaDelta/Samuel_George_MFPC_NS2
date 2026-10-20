# NS2 Event Map

### Interactive map of attacks in Somalia and neighboring countries, 2005–2015

> **At a glance:** 3,305 recorded events · 6 countries · 2005–2015 · filterable by year, month, group and weapon type

---

## 1. Overview

This notebook turns a spreadsheet of recorded attack events into an interactive map. You can narrow the map by **year range**, **month**, **group** and **weapon type**, then click any marker to read the details of that event.

The map focuses on three groups: **Al-Shabaab**, the **Islamic Courts Union (ICU)** and **Al-Ittihaad al-Islami (AIAI)**.

---

## 2. Data

| | |
|---|---|
| **File** | `NS2_Database(Finished).csv` |
| **Rows** | 3,305 events (one row per event) |
| **Time span** | 2005 – 2015 |
| **Countries** | Somalia (2,742), Kenya (427), Ethiopia (53), Uganda (47), Tanzania (34), Djibouti (2) |
| **Most affected city** | Mogadishu (1,069 events), then Kismayo (166) and Baidoa (122) |

> **To confirm:** add the citation for the original database here. Field names such as `iyear` and `gname` follow the Global Terrorism Database style.

### Columns used

| Column | What it holds |
|---|---|
| `iyear`, `imonth`, `iday` | Date of the event |
| `country_txt`, `city` | Where it happened |
| `coordinates` | Map position, written as `POINT (lat, long)` |
| `location` | Extra place detail (often blank) |
| `summary` | Written description of the event |
| `success` | `1` = success, `0` = failure |
| `targsubtype1_txt`, `target1` | Type and name of the target |
| `gname` | Group responsible |
| `weaptype1_txt` | Primary weapon type |

### Events by group

| Group | Events | Active years |
|---|---|---|
| Al-Shabaab | 2,179 | 2007 – 2015 |
| Islamic Courts Union (ICU) | 12 | 2005 – 2008 |
| Al-Ittihaad al-Islami (AIAI) | 1 | 2005 |
| Other / unknown groups | 1,113 | 2005 – 2015 |

### Events by weapon type

| Weapon | Events |
|---|---|
| Explosives | 1,535 |
| Firearms | 1,145 |
| Unknown | 504 |
| Melee | 70 |
| Incendiary | 42 |
| Other | 6 |
| Chemical | 2 |
| Sabotage Equipment | 1 |

Of the 3,305 events, **3,056 (92.5%)** are marked as successful and **249 (7.5%)** as failures. The busiest year is 2014 with 1,007 events.

---

## 3. Requirements and setup

| Requirement | Used for |
|---|---|
| **Python 3** | Running the notebook |
| **Jupyter / JupyterLab** | The notebook environment |
| **folium** | Building the interactive map, marker clusters and popups |
| **ipywidgets** | The year, month, group and weapon controls |
| `csv`, `html` | Reading the data and safe popup text (built into Python, nothing to install) |

Install the two extra libraries with:

```python
%pip install folium ipywidgets
```

Restart the kernel after installing, then run the cells **in order from the top**. Each cell builds on the one before it.

---

## 4. How the data is prepared

1. **Read the CSV.** `csv.DictReader` loads each row into a dictionary, and only the columns listed in `keys` are kept in `records`.
2. **Split the coordinates.** Each `POINT (lat, long)` value is separated into numeric `latitude` and `longitude`.
3. **Handle gaps.** 27 rows have no coordinates, so they get `None` and are left off the map.

---

## 5. Using the map

| Control | What it does |
|---|---|
| **Year range slider** | Shows events between the two selected years |
| **Month slider** | `0` shows all months, `1`–`12` shows a single month |
| **Group dropdown** | Al-Shabaab, ICU, AIAI, or All |
| **Weapon dropdown** | Filters by primary weapon type |

**Marker popups** show the group, location, target, target type, weapon, whether the attack succeeded (Yes or No) and the full summary. Popup details are for reading only and do not affect the filters.

Nearby markers are grouped into numbered clusters. Zoom in or click a cluster to split it into individual events.

---

## 6. Limitations

- **Missing coordinates:** 27 events cannot be plotted.
- **Blank locations:** 2,004 events have no `location` text, so their popup shows *N/A*.
- **Partial dates:** 10 events have no recorded day.
- **Small groups:** ICU has 12 events and AIAI has 1, so many filter combinations show an empty map.
- **Exact matching:** group and weapon names must match the file's spelling exactly.
- **Coordinate order:** values are stored as `POINT (lat, long)`, which is the reverse of the common mapping format `POINT (long lat)`.

---

## 7. Next steps

- Add country and target-type filters.
- Add an animated time slider with `TimestampedGeoJson`.
- Compare Al-Shabaab activity before and after a chosen year.
- Export the filtered map as a standalone `.html` file with `m.save("map.html")`.

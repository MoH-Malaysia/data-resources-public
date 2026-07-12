# Malaysia Health Facilities & Bed Utilisation — Public Data Resources

Open reference datasets on **Ministry of Health Malaysia (KKM)** healthcare
facilities and hospital bed / ICU / ventilator utilisation. Published as plain
CSV so they can be loaded directly into pandas, R, spreadsheets, GIS tools, or
any BI dashboard without preprocessing.

> **Disclaimer:** This is a personal, public data resource. It is **not** an
> official KKM data release. Field names are retained in their original
> Bahasa Malaysia form for traceability to source. Verify against official
> sources before operational or clinical use.

---

## Datasets

| File | Rows | Grain | Description |
|------|-----:|-------|-------------|
| [`facilities_master.csv`](facilities_master.csv) | 5,472 | One facility | Master registry of public health facilities (hospitals, clinics, dental clinics, health offices, labs) with address and geocoordinates. |
| [`bedutil_facility.csv`](bedutil_facility.csv) | 149 | One hospital | Bed, ICU and ventilator capacity and utilisation per hospital. |
| [`bedutil_state.csv`](bedutil_state.csv) | 17 | One state (+ Malaysia total) | State-level aggregate of the same bed / ICU / ventilator metrics. |

---

## `facilities_master.csv`

Registry of **5,472** facilities across all **16 states and federal
territories**. Of these, **5,166** are active (`STATUS = BUKA`) and carry full
geocoordinates.

| Column | Description |
|--------|-------------|
| `Index` | Running row number. |
| `KOD_FASILITI` | Facility code (KKM identifier). |
| `STATUS` | Operational status (e.g. `BUKA` = open). |
| `SEKTOR` | Sector — `AWAM` (public). |
| `SUBSEKTOR` | Owning agency — `KKM`, `KPT` (higher education), `ATM` (armed forces). |
| `PROGRAM_GROUP` | Programme grouping (may be blank). |
| `NEGERI` | State / federal territory. |
| `DAERAH` | District. |
| `KATEGORI_FASILITI` | Facility category (`KLINIK`, `KLINIK PERGIGIAN`, `HOSPITAL`, `PEJABAT KESIHATAN`, `MAKMAL`, …). |
| `JENIS_FASILITI` | Facility type / sub-type. |
| `NAMA_FASILITI` | Facility name. |
| `ALAMAT` | Street address. |
| `BANDAR` | Town / city. |
| `POSKOD` | Postcode. |
| `DAERAH_PENTADBIRAN` | Administrative district. |
| `TELEFON` | Phone number. |
| `EMEL` | Email. |
| `URBAN_RURAL` | Urban / rural classification. |
| `LATITUD` | Latitude (WGS84, decimal degrees). |
| `LONGITUD` | Longitude (WGS84, decimal degrees). |

**Facility mix:** ~2,915 clinics · ~1,684 dental clinics · ~166 hospitals ·
~155 health offices · plus labs, health-promotion centres and others.

## `bedutil_facility.csv` / `bedutil_state.csv`

Bed and critical-care capacity and utilisation. `bedutil_facility.csv` is per
hospital; `bedutil_state.csv` is the same metrics aggregated by state, with a
`Malaysia` grand-total row.

| Column | Description |
|--------|-------------|
| `hospital` / `state` | Facility name (facility file) or state (state file). |
| `beds_nonicu` | Non-ICU bed count. |
| `util_nonicu` | Non-ICU bed utilisation (%). |
| `beds_icu` | ICU bed count. |
| `util_icu` | ICU bed utilisation (%). |
| `vent` | Ventilator count. |
| `util_vent` | Ventilator utilisation (%). |

Blank cells indicate the metric was not reported for that facility.

---

## Usage

```python
import pandas as pd

facilities = pd.read_csv("facilities_master.csv")

# Active hospitals in Selangor
selangor = facilities[
    (facilities["NEGERI"] == "SELANGOR")
    & (facilities["KATEGORI_FASILITI"] == "HOSPITAL")
    & (facilities["STATUS"] == "BUKA")
]

# Join hospital bed utilisation
beds = pd.read_csv("bedutil_facility.csv")
```

```r
facilities <- read.csv("facilities_master.csv")
beds       <- read.csv("bedutil_facility.csv")
```

**Notes**
- Files are UTF-8, comma-delimited, with a header row.
- Text fields (names, addresses) may be quoted and can contain embedded
  commas or line breaks — use a proper CSV parser rather than naive splitting.
- Coordinates are WGS84 decimal degrees, suitable for direct plotting.

---

## License

Released for public reference and educational use. Attribution appreciated.
Source data derives from publicly available Ministry of Health Malaysia
information.

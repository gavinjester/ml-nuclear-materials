# DATA_SOURCES.md — Annotated Dataset Bibliography

## Tier 1: Primary Structured Databases (Direct Download)

### NRC ADAMS (Public Domain)
Access: https://www.nrc.gov/reading-rm/adams.html
Search method: Use "Full-Text Search" with accession numbers below.

| Accession Number | Title | Key Data | Specimen Count (est.) |
|---|---|---|---|
| ML093560076 | NUREG/CR-6960 | CGR in irradiated SS, BWR/PWR | ~300 |
| ML11319A030 | NUREG/CR-7027 | Irradiated SS mechanical properties | ~500 |
| ML062470240 | NUREG/CR-6916 | EAC review, compiled crack growth | ~150 |
| ML050700005 | NUREG/CR-6428 | IASCC of core internals | ~80 |
| ML003737269 | NUREG/CR-4667 Vol.20 | Fatigue/EAC ANL series | ~200 |

Extraction note: Tables are in PDF format. Use camelot-py with `flavor='lattice'`
for bordered tables, `flavor='stream'` for whitespace-delimited.

### IAEA Resources
- TECDOC-1033: "Neutron irradiation embrittlement of reactor pressure vessel steels"
  URL: https://www.iaea.org/publications/5728
  Note: RPV focus but contains austenitic SS radiation hardening data

- IAEA-TECDOC-1502: "Effects of irradiation on materials"
  URL: https://www.iaea.org/publications/7218

- OECD/NEA IASCC Database (restricted membership):
  Contact: NEA Data Bank, https://www.oecd-nea.org/jcms/pl_20464

## Tier 2: High-Value Review Papers (Digitize Tables)

### Must-Extract Papers (listed by data density)

1. **Chopra & Rao (2011)**
   ANL/ENG/NE-11-001, "Review of IASCC of austenitic SS in LWR environments"
   - Appendix A: ~200 specimens with composition, dpa, CGR
   - Argonne Reports: https://www.osti.gov/biblio/1007917
   - OSTI.gov search: "IASCC Chopra Rao 2011"

2. **Was & Bruemmer (1994)**
   "New understanding of irradiation hardening and embrittlement mechanisms"
   J. Nuclear Materials 216:326-347
   - Tables 1-3: Composition, dpa, yield strength, % IGSCC for ~60 alloys

3. **Bruemmer et al. (1999)**
   "Microstructural and microchemical mechanisms controlling IASCC"
   J. Nuclear Materials 274(3):299-314
   - Figure data extractable with WebPlotDigitizer: https://apps.automeris.io/wpd/
   - Contains CGR vs. dpa for multiple SS grades

4. **Andresen & Motta (2012)**
   "Stress corrosion cracking of irradiated stainless steels"
   Comprehensive Nuclear Materials, Vol. 5, pp. 177-205
   - Best single compilation of BWR and PWR CGR data

5. **Jiao & Was (2011)**
   "Novel features of radiation-induced segregation in austenitic stainless steels"
   Acta Materialia 59(4):1220-1238
   - RIS profiles at grain boundaries vs. dpa: ~40 data points

6. **Herrera et al. (2015)**
   "Influence of alloy composition on IASCC susceptibility"
   J. Nuclear Materials 462:91-101
   - Systematic composition variation study: clean dataset

### ML-Specific Papers with Released Data

7. **Kamboj et al. (2023)**
   "Machine learning for radiation embrittlement of reactor pressure vessel steels"
   NPJ Computational Materials
   GitHub: https://github.com/yankang84/RPV-embrittlement-ML
   Note: RPV data but directly adaptable; features overlap with IASCC

8. **Mamivand et al. (2021)**
   "Predicting yield stress of irradiated austenitic stainless steels"
   Computational Materials Science 191:110222
   - Supplementary Table S1: ~180 irradiated SS yield strength data points
   - Features: Cr, Ni, Si, dpa, T_irr, CW, T_test

9. **Skokov & Gutfleisch (2020)** [for transfer learning]
   AFLOWLIB irradiation database — not IASCC directly but
   composition-property mappings for FCC alloys

## Tier 3: OSTI.gov (DOE Open Science)
Search URL: https://www.osti.gov/search/
Recommended queries:
- "irradiation assisted stress corrosion cracking data"  
- "IASCC stainless steel dose"
- "austenitic stainless steel neutron irradiation mechanical properties"
- "crack growth rate BWR irradiated"

Key contractors to filter by: ANL, PNNL, ORNL, GE-GRC

## Tier 4: Raw Experimental Data Requests

### Open Data Policy Contacts
- ANL Materials Science Division: Contact PI directly via published papers
- PNNL IASCC group (Bruemmer group): Published data often available on request
- University of Michigan (Was group): Active ML collaboration interest reported

## Data Format Standardization

When digitizing from multiple sources, use this master schema:

```python
MASTER_SCHEMA = {
    # Identifier
    'specimen_id': str,
    'data_source': str,          # 'ANL', 'PNNL', 'JAERI', 'Halden', etc.
    'publication_doi': str,
    'year': int,
    
    # Material
    'alloy_designation': str,    # '304SS', '316SS', 'Alloy625', etc.
    'Cr_wt_pct': float,
    'Ni_wt_pct': float,
    'Mo_wt_pct': float,
    'Si_wt_pct': float,
    'Mn_wt_pct': float,
    'C_wt_pct': float,
    'P_wt_pct': float,
    'N_wt_pct': float,
    'cold_work_pct': float,
    'heat_treatment': str,
    'grain_size_um': float,
    
    # Irradiation
    'dpa_NRT': float,
    'dpa_ARC': float,
    'dose_rate_dpa_s': float,
    'fluence_fast_n_cm2': float,
    'T_irradiation_C': float,
    'reactor_type': str,         # 'BWR', 'PWR', 'BOR60', 'ATR', 'EBR2'
    'He_appm': float,
    'H_appm': float,
    
    # Environment
    'water_chemistry_regime': str,  # 'BWR_NWC', 'BWR_HWC', 'PWR_primary', etc.
    'ECP_mV_SHE': float,
    'DO_ppb': float,
    'DH_cc_kg': float,
    'T_environment_C': float,
    'conductivity_uS_cm': float,
    'pH_at_temperature': float,
    'SO4_ppb': float,
    'Cl_ppb': float,
    
    # Mechanical
    'specimen_type': str,        # 'CT', 'DCB', 'SSRT', 'Ubend', 'CLT'
    'K_applied_MPa_sqrtm': float,
    'stress_MPa': float,
    'strain_rate_s': float,
    'loading_mode': str,
    
    # Irradiated properties (measured)
    'yield_strength_irr_MPa': float,
    'yield_strength_unirr_MPa': float,
    'UTS_irr_MPa': float,
    'elongation_irr_pct': float,
    
    # Targets
    'cracked': int,              # 0 or 1 (binary label)
    'IGSCC_pct': float,          # % intergranular fracture surface [0-100]
    'CGR_m_s': float,            # Crack growth rate (m/s)
    'log10_CGR': float,          # Derived: log10(CGR_m_s)
    'time_to_initiation_h': float,
    'censored': int,             # 1 if specimen removed without cracking
}
```

## WebPlotDigitizer Workflow for Figure Data

Many CGR vs. dpa plots can be digitized:
1. Download WebPlotDigitizer: https://apps.automeris.io/wpd/
2. Load figure as PNG (screenshot from PDF at 300 DPI minimum)
3. Calibrate axes (log scale if applicable)
4. Use automatic point detection for scatter plots
5. Export as CSV with columns [dpa, log10_CGR]
6. Record DOI, figure number, axis labels in metadata

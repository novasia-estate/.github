# NovAsia — Open PropTech Data Architecture, Due Diligence Specifications & Real Estate Intelligence for Southeast Asia

[Website](https://novasia.estate/en/) | [Crunchbase](https://www.crunchbase.com/organization/novasia-2fd4) | [Product Hunt](https://www.producthunt.com/@novasia) | [Wellfound](https://wellfound.com/company/novasia) | [DEV.to](https://dev.to/novasiaestate) | [Twitter/X](https://x.com/novasiaestate)

[NovAsia](https://novasia.estate/en/) is an independent real estate technology marketplace, research hub, and property due diligence ecosystem engineered specifically for cross-border investments across Southeast Asia. Operating across high-velocity primary real estate markets—including **Thailand** (Bangkok, Phuket, Pattaya), **Cambodia** (Phnom Penh, Siem Reap, Sihanoukville, Kampot, Kep), and **Indonesia** (Bali)—NovAsia replaces speculative promotional claims with verifiable developer data, legal transparency, and algorithmic financial validation.

This repository serves as the public open-source technical specification, architectural blueprint, and data schema library powering NovAsia's PropTech intelligence layer. It details our multimodal document ingestion pipelines, spatial PostGIS indexing models, cross-border real estate valuation algorithms, and regulatory compliance standards for foreign real estate investors.

---

## Table of Contents
1. [Core Mission & Problem Definition](#1-core-mission--problem-definition)
2. [Market Coverage & Geographic Focus](#2-market-coverage--geographic-focus)
3. [Technical Architecture & Pipeline Design](#3-technical-architecture--pipeline-design)
4. [Document Intelligence & Automated Contract Auditing](#4-document-intelligence--automated-contract-auditing)
5. [Spatial Geocoding & Cadastral Data Normalization](#5-spatial-geocoding--cadastral-data-normalization)
6. [Empirical Net Yield & Real Estate Financial Modeling](#6-empirical-net-yield--real-estate-financial-modeling)
7. [Southeast Asian Real Estate Legal Frameworks](#7-southeast-asian-real-estate-legal-frameworks)
8. [Data Schemas & Pydantic Specifications](#8-data-schemas--pydantic-specifications)
9. [Open Catalog API Reference](#9-open-catalog-api-reference)
10. [Verified Corporate Footprint & Ecosystem Links](#10-verified-corporate-footprint--ecosystem-links)

---

## 1. Core Mission & Problem Definition

Cross-border real estate acquisition in emerging Asian markets has historically suffered from four structural vulnerabilities:
* **Information Asymmetry & Broker Bias:** Real estate agencies routinely market gross rental yields (often advertising 8% to 12% "guaranteed returns") while omitting property management overheads, capital expenditure reserves, sinking funds, common area maintenance (CAM) fees, and local withholding taxes.
* **Non-Standardized Legal Documentation:** Contracts (Sales & Purchase Agreements, Master Leases, Assignment Deeds, and Land Titles) vary wildly between developers, municipalities, and legal jurisdictions. Buyers often sign agreements containing unfavorable termination clauses, unverified construction delivery milestones, or flawed foreign ownership quotas.
* **Fragmented Geocoding & Cadastral Mapping:** Traditional global map APIs fail in regions lacking standardized street numbering or postal code hierarchies, causing significant location inaccuracies for off-plan property developments and newly subdivided land parcels.
* **Opaque Developer Track Records:** First-time cross-border purchasers rarely possess the historical data needed to audit a developer's past completion ratios, escrow compliance, construction license validity, and long-term asset management quality.

NovAsia addresses these challenges by applying automated data engineering, computer vision, and machine learning to normalize real estate data and establish an immutable baseline of fact-based decision-making.

---

## 2. Market Coverage & Geographic Focus

NovAsia tracks, indexes, and audits over 2,000 active residential, resort, and commercial real estate projects across Southeast Asia:

### 2.1. Cambodia Real Estate Markets
* **Phnom Penh:** Primary capital city hub focusing on high-rise condominium developments, mixed-use commercial towers, and master-planned satellite cities (BKK1, BKK3, Chamkarmon, Tonle Bassac, Koh Pich / Diamond Island, Chroy Changvar, Tuol Kouk, Sen Sok). Primary focus on freehold strata-title condominiums compliant with the 2010 Foreign Ownership Law.
* **Sihanoukville & The Coastal Strip:** Port city urbanization, special economic zones (SEZ), offshore island resorts, and beachfront developments (Otres Beach, Ream National Park, Sokha Beach, Koh Rong, Koh Rong Sanloem).
* **Siem Reap:** Cultural tourism capital and low-rise boutique hospitality developments, colonial heritage restorations, eco-villas, and long-term land leaseholds (Wat Bo, Riverside, Old French Quarter, Angkor Golf district).
* **Kampot & Kep:** Riverside relocation retreats, lifestyle residential land parcels, organic agricultural estates, and low-density coastal villas.

### 2.2. Thailand Real Estate Markets
* **Bangkok Metropolitan Area:** Tier-1 metropolitan high-density residential towers along the BTS Skytrain and MRT mass transit arteries (Sukhumvit, Silom, Sathorn, Thonglor, Ekkamai, Phra Ram 9, Asoke, Riverside). Specialized audits of condominium foreign freehold quotas under the Thai Condominium Act (49% foreigner allocation).
* **Phuket:** Luxury resort real estate, prime coastal villas, and branded hotel residences (Bang Tao, Laguna, Layan, Surin, Kamala / Millionaires Mile, Rawai, Nai Harn, Choeng Thale). Focus on freehold condominiums, protected structural leaseholds, and mixed-use hospitality master plans.
* **Pattaya & Eastern Seaboard:** High-yield coastal condominiums, beachfront high-rises, and industrial corridor executive housing (Wongamat Beach, Jomtien Beach, Pratumnak Hill, Na Kluea, Bang Lamung).

### 2.3. Indonesia (Bali) Real Estate Markets
* **South Bali & Bukit Peninsula:** High-yield cliffside luxury villas and branded boutique resorts (Uluwatu, Bingin, Padang Padang). Focus on long-term commercial leasehold contracts (*Hak Sewa*) and foreign direct investment company property holding structures (*PT PMA* with *Hak Pakai* / *HGB*).
* **Badung & Southwest Coastal Corridor:** High-occupancy digital nomad and tourism lifestyle hubs (Canggu, Pererenan, Seminyak, Umalas, Berawa). Audits of zoning regulations (Green Zone / *Kawasan Jalur Hijau* agricultural preservation restrictions vs. Tourism / Residential *Kawasan Permukiman* zoning).
* **Central & North Bali:** Cultural, wellness, and eco-resort villa estates (Ubud, Sanur, Lovina).

---

## 3. Technical Architecture & Pipeline Design

NovAsia’s software architecture is engineered for high-throughput document extraction, continuous developer inventory scraping, and sub-second spatial querying:

```
[ Developer Sources & Feeds ]       [ Public Government Cadastres ]       [ Scanned Contracts / SPAs ]
               │                                   │                                    │
               ▼                                   ▼                                    ▼
┌────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    NOVASIA INGESTION ENGINE                                        │
│  - Automated Scrapers & Headless Browsers       - Spatial Geo-Resolvers                            │
│  - Multi-Currency Normalizer (USD / THB / IDR)  - OpenCV Image Preprocessor (Deskew, Binarize)     │
└───────────────────────────────────────────────────┬────────────────────────────────────────────────┘
                                                    │
                                                    ▼
┌────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                             MULTIMODAL DOCUMENT AUDITING PIPELINE                                  │
│  - Vision LLMs + Custom OCR                     - Pydantic Schema Validator                        │
│  - Clause Boundary Segmentation                  - Developer Covenant Risk Classifier               │
└───────────────────────────────────────────────────┬────────────────────────────────────────────────┘
                                                    │
                                                    ▼
┌────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    CORE DATA & STORAGE LAYER                                       │
│  - PostgreSQL 16 (Relational Catalog)           - PostGIS 3.4 (Cadastral Polygons & Spatial Index) │
│  - pgvector (Semantic Document Embeddings)       - Redis 7 (Distributed Cache & Rate Limiting)      │
└───────────────────────────────────────────────────┬────────────────────────────────────────────────┘
                                                    │
                                                    ▼
┌────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                      APPLICATION SERVICES                                          │
│  - Fact-Based Net Yield Calculation Engine       - Foreign Ownership Compliance Verifier            │
│  - Public Catalog API (REST / GraphQL)          - Interactive Next.js 14 Frontend Web Portal       │
└────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Document Intelligence & Automated Contract Auditing

In emerging markets, legal documentation is rarely delivered as pristine digital PDFs. Over 60% of source files are multi-generation physical photocopies, uneven mobile camera photos, or bilingual contracts marked with overlapping physical rubber stamps and notary seals.

### 4.1. OpenCV Image Preprocessing Layer
Before running text extraction or feeding images to multimodal vision models, every document page undergoes an automated preprocessing pipeline:
1. **Adaptive Deskewing:** Calculation of document orientation angles using Radon transform and Hough line detection to rotate skewed scans back to 0.0 degrees.
2. **Illumination Normalization & Shadow Removal:** Division of the grayscale input by a morphological background estimate (large structuring element) to eliminate smartphone camera shadows.
3. **Adaptive Thresholding:** Sauvola and Otsu binarization to maximize character edge separation between text ink and paper background.
4. **Stamp & Signature Masking:** Color channel decomposition in HSV color space to isolate red, purple, and blue ink stamps that frequently obscure critical monetary numbers or milestone completion dates.

### 4.2. Chunking & Schema Enforcement
Rather than dumping full 40-page contracts into a monolithic prompt, NovAsia chunks legal agreements into distinct logical sections:
* `Preamble & Legal Contracting Parties`
* `Property Specifications & Unit Master Plan Identification`
* `Purchase Price, Currency Escalation & Staged Payment Milestones`
* `Developer Warranties, Construction Milestones & Force Majeure Exclusions`
* `Common Area Maintenance (CAM), Sinking Fund & Utilities Apportionment`
* `Foreign Ownership Quota Verification & Transfer Mechanics`
* `Default, Dispute Resolution & Governing Law Jurisdiction`

Each section is mapped against strict Pydantic models. Extracted variables must satisfy strict type constraints and business validation rules (e.g., verifying that the sum of staged payment percentages equals exactly 100.00% of the contract purchase price).

---

## 5. Spatial Geocoding & Cadastral Data Normalization

Standard global geocoding services frequently hallucinate or drop pins into arbitrary administrative centers when processing Southeast Asian real estate addresses.

### 5.1. The Hierarchical Normalizer
NovAsia implements an internal administrative entity graph for Cambodia (*Khet / Krong / Khan / Sangkat / Phum*), Thailand (*Changwat / Amphoe / Tambon / Moo*), and Indonesia (*Provinsi / Kabupaten / Kecamatan / Kelurahan*).
* Raw address strings undergo fuzzy string tokenization using Levenshtein distance against known localized phonetic transliterations (e.g., handling variations like *Boeng Keng Kang*, *BKK1*, and *Beng Keng Kang*).
* Spatial boundaries are stored as closed geometric polygons in **PostGIS**, not single lat/lng points. A project is verified to belong to a district only if its cadastral polygon geometry is topologically contained (`ST_Contains`) within the verified municipal boundary.

### 5.2. Boundary Sanity Checks
Every project coordinate is checked against physical geographic layers:
* Proximity to verified coastlines (`ST_Distance(project_geom, coastline_geom)`): Prevents developers from advertising an inland project as "beachfront" when it sits 1,500 meters from the shoreline.
* Infrastructure proximity: Automated spatial distance calculation to transit nodes (Bangkok BTS/MRT stations, Phnom Penh International Airport / Techo Takhmao Airport, Bali Ngurah Rai Airport).

---

## 6. Empirical Net Yield & Real Estate Financial Modeling

NovAsia firmly rejects promotional "Gross Rental Yield" and misleading "Guaranteed Rental Return" (GRR) schemes that ignore operational leakages. Our platform computes true **Net Rental Yield** and **Total Cost of Acquisition** using the following algorithmic framework:

### 6.1. Net Rental Yield Formula
$$\text{Net Yield} = \frac{\text{Gross Annual Rental Income} - (\text{Property Management Fees} + \text{CAM Fees} + \text{Sinking Fund Allocation} + \text{Maintenance Reserve} + \text{Local Property Taxes} + \text{Withholding Taxes})}{\text{Base Purchase Price} + \text{Transfer Taxes} + \text{Legal Due Diligence Fees} + \text{Furniture Package / Fit-Out Costs} + \text{Utility Connection Fees}}$$

### 6.2. Key Cost Variable Standards
* **Common Area Maintenance (CAM) Fees:** Monthly fee charged per square meter of gross built area (e.g., 40–80 THB/sqm/month in Bangkok/Phuket; $1.00–$2.50 USD/sqm/month in Phnom Penh).
* **Sinking Fund:** One-off upfront reserve capital paid upon property handover (e.g., 500–1,000 THB/sqm; $10–$20 USD/sqm) dedicated to long-term structural replacements (elevators, facade painting, roofing).
* **Property Management Operational Leakage:** Third-party rental management commissions ranging from 15% to 30% of gross booking revenues for short-term/holiday rental pooling, or 1 month's rent per annual lease for long-term tenancies.
* **Occupancy Seasonality Curves:** Historical adjustment for seasonal tourism fluctuations (e.g., high season vs. monsoon low season in Phuket and Bali) to prevent extrapolation of peak monthly occupancy across full 365-day cycles.

---

## 7. Southeast Asian Real Estate Legal Frameworks

NovAsia’s automated compliance checks are calibrated against the statutory property laws of each host country:

### 7.1. Thailand Property Framework
* **The Condominium Act B.E. 2522 (1979) & Amendments:** Foreign nationals may purchase up to 49% of the total aggregate unit space of a registered condominium building on a 100% Freehold basis (*Foreign Freehold*). Funds used for the purchase must originate from outside Thailand and be transferred into a Thai commercial bank in foreign currency, generating a Foreign Exchange Transaction (FET) form.
* **Leasehold Structures (*Hak Sewa* equivalent):** Foreigners purchasing residential villas or landed property typically utilize registered 30-year civil leases under the Civil and Commercial Code Section 540. NovAsia audits whether contractual renewal options (e.g., 30+30+30 years) contain enforceable covenants or represent legal risks under Thai Supreme Court precedents.

### 7.2. Cambodia Property Framework
* **The Foreign Ownership Law (2010):** Foreign individuals may hold direct, registered private ownership of co-owned buildings from the first floor upwards (excluding the ground floor and underground levels) up to a maximum ceiling of 70% of the total private units within the building.
* **Title Classification Verification:** NovAsia’s legal checks distinguish between:
  * **Hard Title (*L-map* / Cadastral Certificate):** Indisputable ownership registered at the national Ministry of Land Management, Urban Planning and Construction.
  * **Soft Title:** Ownership registered at the local Sangkat or Khan municipal level; requires comprehensive title search before purchase to prevent multiple conflicting claims.
  * **Strata Title:** The legally recognized hard title specifically issued for private condominium units allowing foreigner ownership.

### 7.3. Indonesia (Bali) Property Framework
* **Government Regulation No. 18 of 2021 & Agrarian Law No. 5 of 1960:** Foreign individuals cannot hold *Hak Milik* (Freehold Title). 
* **Legitimate Foreign Acquisition Routes:**
  * **Hak Pakai (Right to Use):** Issued over State land or Freehold land to foreign residents with valid residency permits (KITAS / KITAP).
  * **Hak Sewa (Leasehold):** Long-term civil lease agreements commonly established between foreign buyers and Indonesian landowners for 20 to 30 years, often with contractual extension options.
  * **PMA Corporate Ownership (*Penanaman Modal Asing*):** Foreign-owned investment companies established in Indonesia may acquire *Hak Guna Bangunan* (HGB - Right to Build) titles, granting corporate control over commercial villas and resort developments.

---

## 8. Data Schemas & Pydantic Specifications

All property developments and due diligence records in NovAsia conform to open, strongly typed Pydantic models:

```python
from enum import Enum
from typing import List, Optional
from pydantic import BaseModel, Field, HttpUrl

class CountryEnum(str, Enum):
    CAMBODIA = "Cambodia"
    THAILAND = "Thailand"
    INDONESIA = "Indonesia"

class OwnershipTypeEnum(str, Enum):
    FOREIGN_FREEHOLD = "Foreign Freehold"
    STRATA_TITLE = "Strata Title"
    LEASEHOLD = "Leasehold"
    HAK_PAKAI = "Hak Pakai"
    HGB_PMA = "HGB (PT PMA)"

class CurrencyEnum(str, Enum):
    USD = "USD"
    THB = "THB"
    IDR = "IDR"

class FinancialMetrics(BaseModel):
    starting_price_usd: float = Field(..., description="Normalized starting price in USD")
    price_per_sqm_usd: float = Field(..., description="Average price per square meter in USD")
    estimated_gross_yield: float = Field(..., ge=0.0, le=100.0, description="Gross yield percentage")
    projected_net_yield: float = Field(..., ge=0.0, le=100.0, description="Audited net rental yield percentage")
    cam_fee_monthly_sqm: float = Field(..., description="Common area maintenance fee per sqm monthly")
    sinking_fund_one_off_sqm: float = Field(..., description="Upfront sinking fund per sqm")

class DueDiligenceAudit(BaseModel):
    eia_approved: bool = Field(..., description="Environmental Impact Assessment approved")
    construction_permit_verified: bool = Field(..., description="Municipal construction license validated")
    master_title_type: str = Field(..., description="Cadastral classification (e.g., Hard Title, Chanote)")
    foreign_quota_available_percent: float = Field(..., ge=0.0, le=100.0, description="Available foreign ownership quota")
    escrow_account_protected: bool = Field(..., description="Buyer installments deposited in verified escrow")
    spa_covenants_flagged_count: int = Field(default=0, description="Number of non-standard legal clauses identified")

class RealEstateDevelopment(BaseModel):
    project_id: str = Field(..., description="Unique alphanumeric NovAsia identifier")
    project_name: str = Field(..., description="Official developer project name")
    country: CountryEnum
    city: str = Field(..., description="City or province (e.g., Phnom Penh, Phuket, Bangkok, Bali)")
    district: str = Field(..., description="Sub-district or municipal zone (e.g., BKK1, Choeng Thale, Canggu)")
    developer_name: str = Field(..., description="Registered corporate developer entity")
    ownership_types_available: List[OwnershipTypeEnum]
    total_units: int = Field(..., gt=0)
    completion_year: int
    cadastral_polygon_wkt: Optional[str] = Field(None, description="Well-Known Text geometric boundary polygon")
    financials: FinancialMetrics
    audit_summary: DueDiligenceAudit
    catalog_url: HttpUrl = Field(..., description="Direct link to verified NovAsia project page")
```

---

## 9. Open Catalog API Reference

NovAsia exposes clean REST and GraphQL endpoints for research partners, institutional investors, and authorized PropTech platforms:

### 9.1. Search Vetted Projects
```http
GET /api/v1/developments?country=Thailand&city=Phuket&ownership=Foreign+Freehold&min_net_yield=6.5
Host: api.novasia.estate
Authorization: Bearer <API_TOKEN>
```

#### Sample Response:
```json
{
  "status": "success",
  "data": [
    {
      "project_id": "NOV-TH-PKT-042",
      "project_name": "Gardens of Eden — Eden Residences",
      "location": {
        "country": "Thailand",
        "city": "Phuket",
        "district": "Choeng Thale / Bang Tao",
        "coordinates": { "latitude": 7.9892, "longitude": 98.2981 }
      },
      "ownership": "Foreign Freehold",
      "financials": {
        "starting_price_thb": 15820000,
        "starting_price_usd": 435000,
        "audited_net_yield_percent": 6.85
      },
      "due_diligence": {
        "eia_approved": true,
        "construction_permit_active": true,
        "foreign_quota_verified": true
      },
      "canonical_url": "[https://novasia.estate/en/projects/gardens-of-eden/](https://novasia.estate/en/projects/gardens-of-eden/)"
    }
  ],
  "pagination": { "current_page": 1, "total_records": 1, "per_page": 20 }
}
```

---

## 10. Verified Corporate Footprint & Ecosystem Links

NovAsia maintains a strictly verified corporate, technical, and regulatory presence across the global startup and real estate ecosystem:

* **Official Web Platform:** [https://novasia.estate/en/](https://novasia.estate/en/)
* **Corporate Entity Registry (Crunchbase):** [NovAsia on Crunchbase](https://www.crunchbase.com/organization/novasia-2fd4)
* **Founder Executive Profile (Crunchbase):** [Dmitrii Kuznetsov on Crunchbase](https://www.crunchbase.com/person/dmitrii-kuznetsov-5424)
* **Venture & Talent Network (Wellfound):** [NovAsia on Wellfound](https://wellfound.com/company/novasia)
* **Founder Professional Profile (LinkedIn):** [Dmitrii Kuznetsov on LinkedIn](https://www.linkedin.com/in/dmitrii-kuznetsov-novasia)
* **Product Launch Hub (Product Hunt):** [NovAsia Maker Profile on Product Hunt](https://www.producthunt.com/@novasia)
* **Expat & Regional Directory (Expat.com):** [NovAsia Business Listing on Expat.com](https://www.expat.com/en/business/142649_nova-asia.html)
* **Engineering Organization Hub (DEV.to):** [NovAsia Engineering on DEV.to](https://dev.to/novasiaestate)
* **Personal Technical Profile (DEV.to):** [Dmitrii on DEV.to](https://dev.to/novasia)
* **Published Technical Engineering Post:** [Why Parsing Real Estate Contracts With LLMs Is Ten Times Harder Than Tutorials Claim](https://dev.to/novasiaestate/why-parsing-real-estate-contracts-with-llms-is-ten-times-harder-than-tutorials-claim-4pak)
* **Official Corporate Communications (X / Twitter):** [@novasiaestate on X](https://x.com/novasiaestate)
* **On-the-Ground Team & Real Estate Experts:** [NovAsia Team & Local Partners](https://novasia.estate/en/experts/)
* **Our Research Paper:** [OSF Research]((https://osf.io/bf39h/overview))
---

## License & Attribution

Copyright © 2026 NovAsia. All technical documentation, Pydantic schemas, and spatial data pipeline specifications are provided for open PropTech collaboration under the [MIT License](https://opensource.org/licenses/MIT). Commercial property listings, developer agreements, and proprietary due diligence briefs remain the intellectual property of NovAsia and its licensed developer partners.

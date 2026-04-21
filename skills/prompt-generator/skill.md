---
name: prompt-generator
description: Generate high-quality prompts for Adobe LLM Optimizer (LLMO) in any language. Use when users request prompt generation for GEO tracking, ask to create prompts for specific companies/industries/regions, need CSV exports for Adobe LLMO with region and journey stage mapping, want scaled prompt sets for multiple markets, or mention creating/generating prompts with region codes. Supports any language via region codes (e.g., en-CH, de-CH, it-CH, fr-CH, es-ES, pt-BR, it-IT).
---

# Prompt Generator

Generate categorized, journey-mapped prompts for Adobe LLM Optimizer across multiple languages and regions.

## Critical CSV Format Rules

**ATOMIC ROWS ONLY**: Each row must contain EXACTLY ONE value per column. NO arrays, NO multiple values, NO concatenation.

- Each prompt = ONE region = ONE journey stage = ONE row
- 100 prompts for 2 regions = 100 total rows (50 per region)
- Never put multiple regions or journey stages in a single row

**CATEGORY & TOPIC LANGUAGE CONSISTENCY**: Category and topic columns MUST remain in the user's original query language across ALL rows. Only the prompt column changes language based on the target region.

- User asks in Spanish → All categories/topics in Spanish, regardless of prompt language
- User asks in English → All categories/topics in English, regardless of prompt language

This is non-negotiable for Adobe LLMO compatibility.

## Workflow

### 1. Gather requirements
Extract from user request:
- Company name (required)
- Industry (optional, helps with discovery)
- Number of prompts (default: 100)
- Languages/regions (user can specify as language-country codes like en-US, de-CH, fr-CH, es-MX)
- Region codes for CSV output (extract country ISO code only: US, CH, MX, ES, GB)

**Important:** Language-country codes (e.g., en-US, de-CH, fr-CH) are used to:
1. Determine what language to generate prompts in
2. Extract the country code for the CSV region column

Example:
- User requests "de-CH, fr-CH, it-CH" → Generate prompts in German, French, Italian → CSV region = "CH" for all

See references/regions.md for common language-country codes, but remember: CSV only needs country ISO codes.

Ask for missing information only.

### 2. Research and validate company structure

**CRITICAL: Crawl the customer's commercial website FIRST**

**Step 2a - Site crawl (required):**
1. Use web_fetch on the company's main commercial site (e.g., repsol.es, iberia.com, hsbc.com)
2. Identify the site's navigation structure: main menu categories, subcategories, product pages
3. Extract the EXACT terminology they use (not generic industry terms)
4. Map their product/service hierarchy as it appears on their site

**Crawl depth: 2 levels maximum**

Level 1: Main navigation categories (what's in the top menu)
Level 2: Subcategories/product lines under each

Stop at Level 2. Don't crawl:
- Individual product detail pages
- FAQ/support sections
- Legal/privacy pages
- Blog posts (unless specifically relevant)

**Example crawl output:**
```
Site: repsol.es
├── Carburantes (Level 1)
│   ├── Gasolina 95, 98 (Level 2)
│   ├── Diésel e+ (Level 2)
│   └── AutoGas (Level 2)
├── Luz y Gas (Level 1)
│   ├── Tarifas Luz (Level 2)
│   ├── Tarifas Gas (Level 2)
│   └── Energía solar (Level 2)
├── Waylet (Level 1)
│   ├── Pagos (Level 2)
│   └── Descuentos (Level 2)
```

**Step 2b - Supplementary web search:**
- Use web_search to fill gaps (pricing, competitors, recent news)
- Validate findings against the actual site structure
- **NEVER use Claude in Chrome, browser automation, or navigation tools**

**Why crawl first matters:**
- Prompts use the brand's actual terminology
- Categories match their real offerings
- Ensures prompts are relevant to what they actually sell

**Step 2c - Present findings to user for validation:**
Create a summary showing:
- Site structure discovered (categories/subcategories)
- Company overview from supplementary research
- Proposed topics for prompt generation using their terminology

**Example validation summary:**
```
I crawled repsol.es and found the following structure:

1. Carburantes (from site nav)
   - Gasolina 95, Gasolina 98, Diésel e+, AutoGas
   
2. Luz y Gas (from site nav)
   - Tarifas Luz, Tarifas Gas, Energía solar
   
3. Waylet App (from site nav)
   - Pagos móviles, Descuentos y promociones

4. Lubricantes y Especialidades (from site nav)
   - Aceites de motor, Productos mantenimiento

Should I proceed with all these categories, or would you like to add/remove any?
```

**Wait for user confirmation** before proceeding to step 3.

### 3. Build industry structure (after user approval)
**Create structure based on validated business lines:**
- Define categories from approved business lines
- List specific topics under each category
- Map to industry-appropriate customer journey stages

**Journey stages:** Use industry-specific stages when applicable. See references/industries.md for examples:
- Generic: Awareness → Consideration → Purchase → Retention
- Travel/Airlines: Discovery & Browsing → Conversion → Pre-Travel → Day of Travel → Flight Disruption Event → Post-Travel → Retention & Loyalty
- Energy/Utilities: Research → Comparison → Signup → Onboarding → Service Usage → Billing → Retention

### 4. Generate prompts (after structure approval)
Create prompts distributed across:

**Batch Generation (for 50+ prompts):**
Generate in batches of 25-30 prompts to maintain quality and avoid output token limits:
- Batch 1: First set of categories, all archetypes
- Batch 2: Next set of categories, all archetypes
- Continue until target reached

Between batches:
- Verify no duplicate patterns
- Check quality scores remain consistent
- Ensure branded/non-branded ratio is maintained

**Branded vs Non-Branded Distribution (CRITICAL):**
- **35-40% Branded prompts**: Explicitly mention the company/brand name
- **60-65% Non-branded prompts**: Generic queries that DON'T mention the brand

**Why this matters for GEO:**
- Non-branded prompts test if the brand appears in AI responses without being explicitly requested
- Example branded: "What mortgage rates does HSBC offer in February 2026?"
- Example non-branded: "Which bank offers the best mortgage rates in February 2026?"

**Archetype distribution (across both branded and non-branded):**
- **Informational** (20%): Questions seeking understanding/comparison
- **Solution Matching** (20%): Finding best option for specific needs
- **Objection Handling** (20%): Addressing concerns about value
- **Competitive Inference** (20%): Comparing alternatives
- **Action Co-Pilot** (20%): Help with specific tasks

**Important**: Both branded and non-branded prompts should be distributed equally across all 5 archetypes.

**Language-region matching:**
- Extract language code from region (e.g., en-CH → English, de-CH → German, it-CH → Italian, fr-CH → French)
- Region format: `{language}-{country/region}` where language is ISO 639-1 code
- Supports ANY language: es, en, de, fr, it, pt, nl, sv, no, da, fi, pl, cs, etc.

**Quality scoring (target 0.7+):**
Score each prompt on:
- Specificity: Has concrete constraints (not generic)
- Context richness: Includes relevant situational details
- Deliverable clarity: Clear expected outcome
- Conversational: Natural language, not keywords

### 5. Present for review
Display prompts in organized table with:
- Quality scores and statistics
- Distribution across archetypes, regions, journey stages
- **Branded vs Non-branded distribution** (should be 35-40% branded, 60-65% non-branded)
- Average quality score

### 6. Export CSV after confirmation
Generate CSV with exactly 4 columns matching Adobe LLMO template:
```
category,topic,prompt,region
```

**UTF-8 Encoding:**
Always use `encoding='utf-8-sig'` to preserve Spanish 'ñ' and special characters:

```python
import csv

with open('output.csv', 'w', encoding='utf-8-sig', newline='') as f:
    writer = csv.writer(f)
    writer.writerows(data)
```

Without BOM: `España` → `Espa√±a` in Excel. With `utf-8-sig`: displays correctly.

**Column definitions:**
- **category**: Business line/service category (e.g., "Pre-Booking", "Customer Service")
- **topic**: Specific topic within category (e.g., "Baggage Policies & Fees", "Flight Status")
- **prompt**: The actual query text with real brand names
- **region**: ISO country code only (e.g., US, GB, ES, CH, MX, BR) - NO language prefix

**IMPORTANT - Region Format:**
- ✅ CORRECT: US, GB, ES, CH, MX, BR, DE, FR, IT
- ❌ WRONG: en-US, en-GB, es-ES, de-CH, fr-CH, GLOBAL, en-GLOBAL
- **If no region specified: leave region column EMPTY** (not "GLOBAL" or any placeholder)

**Language rules for CSV columns:**

| Column | Language | Example |
|--------|----------|---------|
| category | User's query language (fixed) | "Perfumes" (not "Perfumi" for Italian) |
| topic | User's query language (fixed) | "Fragancias femeninas" (not "Profumi femminili") |
| prompt | Target region language (varies) | Changes per language-country code |
| region | ISO code only | CH, US, ES |

**Why this matters:** Categories and topics must be consistent across all language variants for Adobe LLMO filtering and reporting. Only prompts are localized.

**Example - User asks in Spanish for es-ES, it-IT, fr-FR:**
```csv
category,topic,prompt,region
Perfumes,Fragancias femeninas,¿Cuál es el mejor perfume floral para verano?,ES
Perfumes,Fragancias femeninas,Qual è il miglior profumo floreale per l'estate?,IT
Perfumes,Fragancias femeninas,Quel est le meilleur parfum floral pour l'été?,FR
```

**Language-region mapping for prompts only:**
- User requests "de-CH" → German prompts → CSV region = "CH"
- User requests "fr-CH" → French prompts → CSV region = "CH"
- User requests "it-CH" → Italian prompts → CSV region = "CH"
- User requests "en-US" → English prompts → CSV region = "US"
- User requests "es-MX" → Spanish prompts → CSV region = "MX"

**Row generation rules:**
- For N prompts across M regions: Create separate rows for each region
- Same prompt in different languages for same country = separate rows with SAME region code
- **35-40% of prompts must be branded** (mention the actual company name)
- **60-65% of prompts must be non-branded** (generic queries without brand mention)

**Use real brand names:**
- Branded: "What's the baggage limit for Iberia?"
- Branded: "How much does Lufthansa charge for extra baggage?"
- Non-branded: "Which airlines have the lowest baggage fees?"
- Comparative: "Is Repsol cheaper than Iberdrola for electricity?"

Example CSV output:
```csv
category,topic,prompt,region
Pre-Booking,Research & Comparison,Which airlines allow free cancellation?,US
Pre-Booking,Research & Comparison,What's the difference between Iberia basic economy and regular economy?,US
Pre-Booking,Baggage Policies & Fees,How much does checked baggage cost for Lufthansa?,GB
Pre-Booking,Baggage Policies & Fees,Which airlines allow free checked bags?,GB
Customer Service,Flight Status,What's the best app for real-time flight tracking?,US
Customer Service,Flight Status,What happens if I miss my British Airways flight?,GB
```

**Multi-language example (Switzerland) - User query in English:**
```csv
category,topic,prompt,region
Energy Plans,Electricity Rates,What's the best electricity plan for a 3-bedroom apartment?,CH
Energy Plans,Electricity Rates,Welcher Stromtarif ist am besten für eine 3-Zimmer-Wohnung?,CH
Energy Plans,Electricity Rates,Quel est le meilleur plan d'électricité pour un appartement de 3 pièces?,CH
Energy Plans,Electricity Rates,Qual è il miglior piano elettrico per un appartamento di 3 stanze?,CH
```
Note: Category and topic stay in English (user's query language), only prompts are localized per target language

## Quality Standards

High-quality prompt example:
"What's the best unlimited data plan for someone who streams 4K video 3 hours daily and uses mobile hotspot for work?"
- Specific usage details
- Clear need
- Rich context
- Conversational

Low-quality prompt example:
"cheap data plans"
- Too generic
- Keywords not conversation
- No context

## Reference Files

Load these as needed:

- **references/regions.md**: Complete list of supported region codes (es-ES, en-US, etc.)
- **references/industries.md**: Pre-built structures for airlines, telecom, retail, financial services
- **references/prompt-library.md**: Adobe LLMO official prompt library with examples by industry

## Assets

- **assets/template.csv**: Example CSV structure for reference

## Statistics to Include in Export

- Total rows generated
- Average quality score
- **Branded vs Non-branded**: X prompts branded (35-40%), Y prompts non-branded (60-65%)
- Distribution by archetype (should be 20% each, across both branded and non-branded)
- Distribution by region
- Distribution by category
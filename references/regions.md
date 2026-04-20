# Supported Region Codes

## How Region Codes Work

**User Input Format:** `{language}-{country}` (e.g., en-US, de-CH, fr-CH)
- Language code determines what language to generate prompts in
- Country code becomes the CSV region column value

**CSV Output Format:** Country ISO code only (e.g., US, CH, ES, GB, MX, BR)

### Example Workflow

**User requests:** "Generate prompts for de-CH, fr-CH, it-CH"

**Claude processes:**
1. de-CH → Generate German prompts → CSV region = "CH"
2. fr-CH → Generate French prompts → CSV region = "CH"
3. it-CH → Generate Italian prompts → CSV region = "CH"

**CSV output:**
```csv
category,topic,prompt,region
Energy Plans,Welcher Stromtarif ist am besten?,CH
Energy Plans,Quel est le meilleur plan d'électricité?,CH
Energy Plans,Qual è il miglior piano elettrico?,CH
```

All three languages, same region code "CH".

---

## Common Language-Country Codes

Use these codes when user specifies languages/regions. Extract the country code for CSV.

### Spanish Markets
- `es-ES` → Spanish prompts → CSV region: **ES** (Spain)
- `es-MX` → Spanish prompts → CSV region: **MX** (Mexico)
- `es-AR` → Spanish prompts → CSV region: **AR** (Argentina)
- `es-CO` → Spanish prompts → CSV region: **CO** (Colombia)
- `es-PE` → Spanish prompts → CSV region: **PE** (Peru)
- `es-CL` → Spanish prompts → CSV region: **CL** (Chile)

### English Markets
- `en-US` → English prompts → CSV region: **US** (United States)
- `en-GB` → English prompts → CSV region: **GB** (United Kingdom)
- `en-AU` → English prompts → CSV region: **AU** (Australia)
- `en-CA` → English prompts → CSV region: **CA** (Canada)
- `en-IN` → English prompts → CSV region: **IN** (India)

### Switzerland (Multi-language)
- `de-CH` → German prompts → CSV region: **CH**
- `fr-CH` → French prompts → CSV region: **CH**
- `it-CH` → Italian prompts → CSV region: **CH**
- `en-CH` → English prompts → CSV region: **CH**

### German Markets
- `de-DE` → German prompts → CSV region: **DE** (Germany)
- `de-AT` → German prompts → CSV region: **AT** (Austria)
- `de-CH` → German prompts → CSV region: **CH** (Switzerland)

### French Markets
- `fr-FR` → French prompts → CSV region: **FR** (France)
- `fr-CA` → French prompts → CSV region: **CA** (Canada)
- `fr-BE` → French prompts → CSV region: **BE** (Belgium)
- `fr-CH` → French prompts → CSV region: **CH** (Switzerland)

### Italian Markets
- `it-IT` → Italian prompts → CSV region: **IT** (Italy)
- `it-CH` → Italian prompts → CSV region: **CH** (Switzerland)

### Portuguese Markets
- `pt-PT` → Portuguese prompts → CSV region: **PT** (Portugal)
- `pt-BR` → Portuguese prompts → CSV region: **BR** (Brazil)

### Other European Languages
- `nl-NL` → Dutch prompts → CSV region: **NL** (Netherlands)
- `nl-BE` → Dutch prompts → CSV region: **BE** (Belgium)
- `sv-SE` → Swedish prompts → CSV region: **SE** (Sweden)
- `no-NO` → Norwegian prompts → CSV region: **NO** (Norway)
- `da-DK` → Danish prompts → CSV region: **DK** (Denmark)
- `fi-FI` → Finnish prompts → CSV region: **FI** (Finland)
- `pl-PL` → Polish prompts → CSV region: **PL** (Poland)
- `cs-CZ` → Czech prompts → CSV region: **CZ** (Czech Republic)

---

## CSV Region Column - Country ISO Codes Only

**Always use 2-letter ISO country codes in CSV:**

| Country | ISO Code | Examples |
|---------|----------|----------|
| United States | US | NOT en-US |
| United Kingdom | GB | NOT en-GB or UK |
| Spain | ES | NOT es-ES |
| Mexico | MX | NOT es-MX |
| Switzerland | CH | NOT de-CH, fr-CH, it-CH |
| Germany | DE | NOT de-DE |
| France | FR | NOT fr-FR |
| Italy | IT | NOT it-IT |
| Brazil | BR | NOT pt-BR |
| Canada | CA | NOT en-CA or fr-CA |
| Argentina | AR | NOT es-AR |
| Netherlands | NL | NOT nl-NL |

---

## Examples

### Example 1: Single Language, Single Country
**User:** "Generate 100 prompts for en-US"
**Process:** Generate English prompts
**CSV region column:** US (100 rows with region=US)

### Example 2: Multi-Language, Same Country
**User:** "Generate 300 prompts for de-CH, fr-CH, it-CH"
**Process:** 
- 100 German prompts
- 100 French prompts
- 100 Italian prompts
**CSV region column:** CH (all 300 rows have region=CH)

### Example 3: Same Language, Multiple Countries
**User:** "Generate 200 prompts for es-ES, es-MX"
**Process:**
- 100 Spanish prompts (Spain context)
- 100 Spanish prompts (Mexico context)
**CSV region column:**
- 100 rows with region=ES
- 100 rows with region=MX

### Example 4: Multiple Languages, Multiple Countries
**User:** "Generate 400 prompts for en-US, es-ES, de-DE, fr-FR"
**Process:**
- 100 English prompts
- 100 Spanish prompts
- 100 German prompts
- 100 French prompts
**CSV region column:**
- 100 rows with region=US
- 100 rows with region=ES
- 100 rows with region=DE
- 100 rows with region=FR

---

## Key Takeaways

1. **User input:** Language-country codes (en-US, de-CH, fr-CH)
2. **Prompt generation:** Use language code to determine prompt language
3. **CSV output:** Use country code only (US, CH, FR)
4. **Multi-language countries:** Same country code for all languages (e.g., CH for de-CH, fr-CH, it-CH)
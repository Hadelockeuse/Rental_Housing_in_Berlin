# Rental_Housing_in_Berlin
## Introduction
After struggling to find a convenient flat at a reasonable price in Berlin, I became interested in understanding the characteristics of the rental housing market in this city. This project aims to explore the Berlin rental market through web scraping, data cleaning, and visualization. 

## Project Structure
This repository contains three main Jupyter Notebook files that document the different steps of the analysis:
1. **Appartment_listings_Scraper.ipynb** – Web scraping property listings.
2. **multiple_to_one_cvs_merging.ipynb** – Merging multiple CSV files into a single dataset.
3. **Rental_property_listings_in_Berlin.ipynb** – Cleaning and preparing the rental data for visualization.
4.  **rental_housing_project.pbix.pbix** – Dataset visualization of correlations and trends with Power BI.

## Datasets Available in This Repository
To ensure privacy and compliance with data protection policies, I have made the datasets available with certain modifications:

### Web-Scraped Listings (before cleaning)
📂 File: `merged_listings_4_github.csv`

- This file contains the raw, web-scraped rental listings before any cleaning, filtering, or feature engineering.
- Modifications made for privacy reasons: I removed URLs, addresses, and real estate agency details.

### Cleaned and Processed Listings (after cleaning - see third step)
📂 File: `updated_listings_4_github.csv`

- This file contains the fully cleaned dataset after all preprocessing steps (outlier removal, feature engineering, etc.).
- Modifications made for privacy reasons: I removed URLs and addresses.
- For columns decription and explanation 📂 See: `col_description_updated_listings.md`

  
## Steps in the project
### First step: Webscraping with Python
📂 See: `Appartment_listings_Scraper.ipynb`

**Objective**: The goal of this step was to collect rental property listings from a well-known German real estate website. These listings contain essential information such as rent price, surface area, number of rooms, and various property features. Since this data is not openly available in a structured format, web scraping was used to extract and compile it into a dataset.

**Steps taken**:
1. **Identifying the Data Source**
- The target website lists thousands of rental properties in Berlin.
- Each listing contains multiple elements of interest:
  - Basic rental information (cold rent, warm rent, additional costs).
  - Property details (size, number of rooms, floor level).
  - Amenities (balcony, garden, elevator, etc.).
  - Rental conditions (furnished/unfurnished, pet policies, availability date).
- Listings are paginated, requiring automated navigation to scrape multiple pages.

2. **Choosing the Scraping Method**
Two main scraping approaches were considered:
- Static Scraping using requests and BeautifulSoup:
  - Works well when the website serves static HTML content.
  - Faster and more lightweight, but does not handle dynamic elements.
- Dynamic Scraping using Selenium:
  - Required because certain elements (e.g., rental prices) load dynamically via JavaScript.
  - Allows interaction with the webpage (clicking "Next" to load more listings).
  - Necessary for extracting full property details.

3. **Implementation**
The scraping process was implemented using the following tools:
- _requests_ – For fetching raw HTML content (when possible).
- _BeautifulSoup_ – For parsing and extracting structured data from HTML.
- _Selenium_ – For interacting with dynamic elements such as pagination and JavaScript-rendered content.
- _pandas_ – For storing extracted data in a structured format (DataFrame).
______________________________________________________________
### Second step: Data merging with Python
📂 See: `multiple_to_one_cvs_merging.ipynb`

- **Objective:** Combine multiple CSV files (each containing rental listings from different scraping runs) into a single dataset.
  
- **Steps taken:**
  - Read multiple CSV files.
  - Standardized column names and formats.
  - Concatenated data into one structured dataset.
______________________________________________________________
### Third step: Data cleaning and preparation with Python
📂 See: `Rental_property_listings_in_Berlin.ipynb`

**Objective**: The raw scraped data contained inconsistencies, missing values, and irrelevant listings that needed cleaning before any analysis. The goal of this step was to:
- Remove listings that do not represent standard rental apartments.
- Ensure consistent and structured data.
- Create new useful features to enhance analysis.
- Detect and correct potential errors in the dataset.

**Steps taken**:
1. **Filtering Out Non-Rental Listings**
- The initial dataset contained listings for shared flats (WG - Wohngemeinschaften) and apartment swaps. Since these do not reflect standard rental properties, they were removed.
- Any listing mentioning "_WG-geeignet_" (WG-friendly) or "_Wohnen auf Zeit_" (temporary housing) was also filtered out to ensure only full rental listings were included.
- Properties with a surface area ≤ 22 m² were excluded since most of them were private rooms rather than full apartments.

2. **Handling Missing Values**
- Columns with high proportions of missing values were carefully assessed.
- Missing values in "_Warmmiete (€/month)_" (warm rent) were imputed using:
    - _Warmmiete = Kaltmiete + Nebenkosten + Heizkosten_
- If "_Kaltmiete_" (cold rent) was missing but "_Warmmiete_" was available, it was estimated by subtracting known cost components.
- Missing "_Surface Area (m²)_" values were manually retrieved from the original listings whenever possible.
- Price per Square Meter (_price per sqm_) was calculated for missing values using:
-   - _Kaltmiete/Surface Area_
  
3. **Feature Engineering: Creating New Useful Columns**
- Availability Period (_days_until_available_): The number of days until the apartment becomes available, derived from the move-in date.
- Rental Conditions: Extracted keywords from listing titles and features to determine:
  - If the apartment requires a _Wohnberechtigungsschein_ (WBS - social housing permit).
  - If it is explicitly furnished.
  - If pets are allowed.

4. **Standardizing Address and Location Data**
- The raw addresses were split to extract Berlin districts ("_Bezirk_") and subdistricts for better geographic analysis.
- The zip codes were verified.

5. **Handling Erroneous and Outlier Values**
- Unrealistic Rental Prices:
  - Listings with extreme price per sqm values were flagged as outliers.
  - Any rental price below 5€/m² or above 50€/m² was manually checked and corrected if needed.
- Service Charges (_Nebenkosten_) Outliers:
  - The average Nebenkosten was calculated per square meter, and extreme deviations were flagged.
  - Several listings had Nebenkosten > 50% of the Kaltmiete, which were corrected or removed.
- Deposits (Kaution) Outliers:
  - Normal deposits range between 2-3 times the Kaltmiete.
  - Listings with deposits significantly higher or lower than expected were manually reviewed and adjusted.
  
6. **Converting Data to Proper Formats**
- Numeric Conversion:
  - Surface area, rent, and deposit values were converted to numerical data types.
  - Dates were formatted as YYYY-MM-DD for easy filtering.
- Boolean Features (1 = Yes, 0 = No):
  - Furnished
  - Balcony
  - Elevator
  - Pets Allowed
  - Barrier-Free (accessible)
- One-Hot Encoding for Heating Types (Gas, Electric, District Heating, etc.).

7. **Exporting the Cleaned Dataset**
- After cleaning, the dataset was saved in CSV format for further analysis in Power BI.
______________________________________________________________

**Keys features of the cleaned Dataset**
This cleaned dataset is structured to provide insights into the Berlin rental market.

- **Rental Price Details:**
  - Cold rent (Kaltmiete), warm rent (Warmmiete), and additional costs (Nebenkosten).
  - Price per square meter (€/m²).

- **Property Characteristics:**
  - Surface area in square meters.
  - Number of rooms (whole and half rooms included).
  - Floor level and whether it spans multiple floors.
  - Year of construction and energy efficiency rating.

- **Location Details:**
  - District (Bezirk) and subdistrict extracted from addresses.
  - Zip code for geospatial analysis.

- **Amenities & Features:**
  - Balcony, terrace, or garden.
  - Furnished or unfurnished.
  - Elevator availability.
  - Heating type (gas, electric, district heating, etc.).
  - Accessibility (barrier-free / wheelchair accessible).

- **Rental Conditions:**
  - Maximum rental duration when relevant(time_lim (in months)).
  - Move-in availability date.

______________________________________________________________
### Fourth step: Data visualization with Power BI
📂 See: `rental_housing_project.pbix.pbix`

📂 See: `rental_housing_project.pbix.pdf`

**Objective**: To gain a deeper understanding of the rental market in Berlin, I visualized the cleaned dataset using Power BI. The goal of this step was to highlight key trends and correlations in the rental housing market.

## Findings
**Availability:** Nearly **70% of listings** were available for immediate move-in (see day 0 in light blue).
**Energy Efficiency:** The energy efficiency ratings range from A+ (best) to H (worst). Fortunately, aspiring renters are more likely to come across a listing with one of the higher-rated categories (i.e., A+, A, B, or C), which account for more than 75% of the ratings in the market.
**District Pricing Trends:** Unsurprisingly, the data confirms that some districts have significantly higher rents than others, with Charlottenburg-Wilmersdorf and Mitte being the two most expensive, while Lichtenberg-Hohenschönhausen, Marzahn-Hellersdorf, and Spandau are the three most affordable. 
**Subdistrict Pricing Trends:** It was possible to analyze the rental market at a more granular level by looking at subdistricts. However, since the number of cleaned listings was limited to 2,294, the median warm rent calculated for each subdistrict may not fully represent the actual rental situation in Berlin. Subdistricts with fewer than six listings were grouped under "Other subdistrict," which explains why not all Berlin subdistricts appear in the visualization.
**Flooring & Rent Price:** Rental listings mentioning plastic, linoleum, or laminate flooring tend to be more affordable than those featuring stone flooring. 
**Heating & Rent Price:** Flats with a floor heating system tend to be the least expensive, whereas those with a central heating system are the most expensive.
**Social Housing Permit (WBS):** Apartments requiring a social housing permit (Wohnberechtigungsschein or WBS in German) tend to be €541 cheaper than those that do not require one.
**Listing Photos & Price:** An unexpected finding from this visual data exploration is that listings with 10 or fewer pictures tend to be €386 cheaper than those with more than 10 pictures. However, this may be correlated with other factors influencing rent, such as the number of rooms or the total surface area of the property. It could also reflect the willingness of real estate agents to invest more in advertising for higher-priced properties.

### Summary 
This project provides insights into Berlin’s rental market and highlights key factors that influence warm rent prices. Renters looking for affordable options should consider:
- Choosing properties with **floor heating** rather than central heating
- Opting for apartments with **plastic** or **laminate flooring** instead of stone
- Exploring listings in less expensive districts like **Spandau** or **Marzahn-Hellersdorf**. At the subdistrict level, the most affordable areas were **Oberschöneweide**, **Heinersdorf**, and **Staaken**.

It is also interesting to note two key characteristics of Berlin’s rental market:
- Around **70%** of the flats are **available for immediate move-in**.
- **Three-quarters** of the listings have **relatively good energy efficiency ratings** (from A+ to C).

Additionally, individuals with a WBS (Wohnberechtigungsschein) can **significantly benefit from lower-cost housing options**. It's not just a common assumption, but a concrete advantage that helps those with a WBS find more affordable flats.

### Limitations
**Sample Size:** The dataset includes only 2294 listings, limiting generalizability, particularly for subdistrict analysis.
**No Statistical Testing:** Findings are based on exploratory visualization rather than statistical significance tests.
**Data Source Bias:** Listings were scraped from a single real estate website, which may not represent the full market.
**Imputed Rent Values:** Some warm rent values were calculated from cold rent + utility costs + eventual parking costs, as not all listings provided this information explicitly.

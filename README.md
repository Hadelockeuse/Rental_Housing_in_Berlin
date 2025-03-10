# Rental_Housing_in_Berlin
## Introduction
After struggling to find a convenient flat at a reasonable price in Berlin, I became interested in understanding the characteristics of the rental housing market in this city. This project aims to explore the Berlin rental market through web scraping, data cleaning, and preparation. 

## Project Structure
This repository contains three main Jupyter Notebook files that document the different steps of the analysis:
1. Appartment_listings_Scraper.ipynb – Web scraping apartment listings.
2. multiple_to_one_cvs_merging.ipynb – Merging multiple CSV files into a single dataset.
3. Rental_property_listings_in_Berlin.ipynb – Cleaning and preparing the rental data for analysis.

## Steps in the project
______________________________________________________________
### First step: Webscraping with Python
- **Objective:** Collect rental property listings from one of Germany's most popular real estate websites.
- **Tools Used:** requests, BeautifulSoup, Selenium, and pandas.
- **Key Output:** A dataset containing raw rental listings extracted from the website.

📂 See: Appartment_listings_Scraper.ipynb
______________________________________________________________
### Second step: Data merging with Python
- **Objective:** Combine multiple CSV files (each containing rental listings from different scraping runs) into a single dataset.
  
- **Steps taken:**
  - Read multiple CSV files.
  - Standardized column names and formats.
  - Concatenated data into one structured dataset.

📂 See: multiple_to_one_cvs_merging.ipynb
______________________________________________________________
### Third step: Data cleaning and preparation with Python
**Objective**: The raw scraped data contained inconsistencies, missing values, and irrelevant listings that needed cleaning before any analysis. The goal of this step was to:
- Remove listings that do not represent standard rental apartments.
- Ensure consistent and structured data.
- Create new useful features to enhance analysis.
- Detect and correct potential errors in the dataset.

**Steps taken**:
1. **Filtering Out Non-Rental Listings**
- The initial dataset contained listings for shared flats (WG - Wohngemeinschaften) and apartment swaps. Since these do not reflect standard rental properties, they were removed.
- Any listing mentioning "WG-geeignet" (WG-friendly) or "Wohnen auf Zeit" (temporary housing) was also filtered out to ensure only full rental listings were included.
- Properties with a surface area ≤ 22 m² were excluded since most of them were private rooms rather than full apartments.

2. **Handling Missing Values**
- Columns with high proportions of missing values were carefully assessed.
- Missing values in "Warmmiete (€/month)" (warm rent) were imputed using: Warmmiete = Kaltmiete + Nebenkosten + Heizkosten
- If "Kaltmiete (cold rent)" was missing but "Warmmiete" was available, it was estimated by subtracting known cost components.
- Missing "Surface Area (m²)" values were manually retrieved from the original listings whenever possible.
- Price per Square Meter (price per sqm): Calculated for missing values using: Kaltmiete/Surface Area
- 
3. **Feature Engineering: Creating New Useful Columns**
- Availability Period (days_until_available): The number of days until the apartment becomes available, derived from the move-in date.
- Rental Conditions: Extracted keywords from listing titles and features to determine:
  - If the apartment requires a Wohnberechtigungsschein (WBS - social housing permit).
  - If it is explicitly furnished.
  - If pets are allowed.

4. **Standardizing Address and Location Data**
- The raw addresses were split to extract Berlin districts ("Bezirk") and subdistricts ("Ortsteil") for better geographic analysis.
- The zip codes were verified.

5. **Handling Erroneous and Outlier Values**
- Unrealistic Rental Prices:
  - Listings with extreme price per sqm values were flagged as outliers.
  - Any rental price below 5€/m² or above 50€/m² was manually checked and corrected if needed.
- Service Charges (Nebenkosten) Outliers:
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

📂 See: Rental_property_listings_in_Berlin.ipynb

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
  - District (Bezirk) and subdistrict (Ortsteil) extracted from addresses.
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
Lastly, I visualized the data using Power BI.

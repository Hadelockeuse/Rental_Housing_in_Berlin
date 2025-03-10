# Column Descriptions for `updated_listings_4_github.csv`

This rental property dataset contains 2,294 rental property listings (rows) from Berlin with 63 columns providing details on rent, property characteristics, location, and amenities.

## **Rental Information**
- **Title:** Title of the property listing from the real estate website (string).
- **Kaltmiete zzgl. Nebenkosten (€/month):** Cold rent in euros per month (continuous numeric).
- **Warmmiete (€/month):** Warm rent (base rent + heating and service costs) in euros per month (continuous numeric).
- **Nebenkosten (€/month):** Additional service costs (e.g., maintenance, water) in euros per month (continuous numeric).
- **Heizkosten_1 (€/month):** Indicates whether heating costs are included in warm rent or additional costs (string).
- **Heizkosten_2 (€/month):** Heating costs in euros per month (continuous numeric).
- **Miete pro Stellplatz (€/month):** Monthly rent for a parking space in euros (continuous numeric).
- **Price per sqm:** Rent per square meter (continuous numeric). If missing, it was estimated using:
  - `Kaltmiete / Surface Area` when possible.
  - Otherwise, estimated from `Warmmiete / Surface Area`.
- **Deposit:** Security deposit amount (continuous numeric).
- **time_lim (in months):** If applicable, the maximum rental duration in months (continuous numeric).

## **Property Characteristics**
- **Rooms_number:** Number of rooms (discrete numeric). Includes whole numbers (e.g., 1, 2, 3) and half numbers (e.g., 1.5, 2.5).
- **Surface Area (m²):** Total surface area of the property in square meters (continuous numeric).
- **Floor:** The floor level the property is located on (discrete numeric).
  - 0 represents the ground floor (Erdgeschoss).
  - Half numbers (e.g., 1.5) indicate properties spanning multiple floors.
- **Year of Construction:** Binary category:
  - "Neubau" (New construction, built after 1945).
  - "Altbau" (Old construction, built in 1945 or earlier).
- **Condition:** 3 possible values:
  - "neuwertig" (like new).
  - "Erstbezug" (first occupancy).
  - "Gepflegt" (well maintained).
- **energy_efficiency:** Energy efficiency rating (discrete numeric), coded from 0 (A+) to 8 (H).
- **Multiple floors:** Property spans multiple floors (1 = Yes, 0 = No).

## **Location Details**
- **Zip Code:** Postal code of the property location (discrete numeric).
- **Bezirk:** The district in Berlin where the property is located (string).
- **district_name:** The subdistrict (Ortsteil) where the property is located (string).

_(Note: Addresses and URLs have been removed for privacy reasons.)_

## **Amenities & Features**
- **balcony:** Includes a balcony and/or winter garden (1 = Yes, 0 = No or not specified).
- **furnished:** The property is furnished (1 = Yes, 0 = No or not specified).
- **garden:** Has a garden (1 = Yes, 0 = No or not specified).
- **Terrasse:** Has a terrace (1 = Yes, 0 = No or not specified).
- **pets_allowed:** Pets are allowed (1 = Yes, 0 = No or not specified).
- **Garage:** Includes a parking facility (garage, carport, or parking space) (1 = Yes, 0 = No or not specified).
- **shower:** Has a shower (1 = Yes, 0 = No or not specified).
- **bath:** Has a bathtub (1 = Yes, 0 = No or not specified).
- **Einbaukueche:** Includes a built-in kitchen (1 = Yes, 0 = No or not specified).
- **kitchenette:** Includes a kitchenette (1 = Yes, 0 = No or not specified).
- **lift:** Has an elevator (1 = Yes, 0 = No or not specified).
- **open kitchen:** Has an open kitchen (1 = Yes, 0 = No or not specified).
- **basement:** Has a basement (1 = Yes, 0 = No or not specified).
- **window in bathroom:** Bathroom has a window (1 = Yes, 0 = No or not specified).
- **WG-geeignet:** Suitable for shared living (1 = Yes, 0 = No or not specified).
- **accessible:** Barrier-free and/or wheelchair-accessible (1 = Yes, 0 = No or not specified).

## **Rental Conditions**
- **lim_inhabitants:** Indicates whether there is a restriction on the number of tenants (1 = Yes, 0 = No or not specified).
- **Wohnen auf Zeit in title:** Indicates temporary housing in the title (1 = Yes, 0 = No).
- **nur mit Wohnberechtigungsschein in title:** Requires a WBS (public housing eligibility certificate) (1 = Yes, 0 = No).
- **availability_date:** The earliest move-in date (datetime format).
- **days_until_available:** Number of days between the web scraping date (12.02.2025) and the move-in date.

## **Heating Types** (One-hot encoded: 1 = Yes, 0 = No or not specified)
- **Heating_Elektro:** Electric heating.
- **Heating_Fernwaerme:** District heating.
- **Heating_Gas:** Gas heating.
- **Heating_Oel:** Oil heating.
- **Heating_Luft-/Wasser-Waermepumpe:** Air/water heat pump.
- **Heating_Blockheizkraftwerk:** Block heating plant.
- **Heating_Solar:** Solar heating.
- **Heating_Erdwaerme:** Geothermal heating.
- **Heatingsys_Zentralheizung:** Central heating system.
- **Heatingsys_Fussbodenheizung:** Underfloor heating system.
- **Heatingsys_Etagenheizung:** Floor heating system.

## **Floor Coverings** (One-hot encoded: 1 = Yes, 0 = No or not specified)
- **Floor_cov_Parkett:** Parquet flooring.
- **Floor_cov_Fliesen:** Tiled flooring.
- **Floor_cov_Laminat:** Laminate flooring.
- **Floor_cov_Holzdielen:** Wooden plank flooring.
- **Floor_cov_Kunststoff:** Plastic flooring.
- **Floor_cov_Linoleum:** Linoleum flooring.
- **Floor_cov_Stein:** Stone flooring.


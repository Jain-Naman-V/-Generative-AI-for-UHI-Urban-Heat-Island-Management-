# NYC Urban Heat Island (UHI) Analysis & Prediction Tool 🏙️🔥➡️🌳
[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://namanuhi.streamlit.app/)

**Explore, understand, and get insights into Urban Heat Island effects in New York City.**

The **Urban Heat Island (UHI)** effect describes the phenomenon where cities are significantly warmer than surrounding rural areas, creating "islands" of heat. This project combines geospatial data analysis, machine learning interpretability, and generative AI to provide a tool for visualizing and understanding the drivers of the UHI Index (a measure of this relative temperature difference) in specific boroughs of NYC, based on data from July 24, 2021.

It leverages ground-based UHI Index measurements, local weather station data, Sentinel-2 satellite imagery, and elevation data, processed via the Microsoft Planetary Computer. The tool features interactive maps and utilizes Google's Gemini API to provide localized explanations and potential **mitigation strategies** aimed at reducing the UHI effect.

This work originated from contributions to the **2025 EY Open Science Data Challenge: Urban Heat Islands**.

---

## Understanding UHI & Key Parameters 🌡️🌿🏢

The UHI effect is driven by several factors, many of which are captured by the parameters used in this tool:

*   **What is UHI?** Urban areas replace natural, cooling landscapes with materials (asphalt, concrete) that absorb and retain heat. Reduced vegetation limits cooling through evapotranspiration, and urban geometry (tall buildings) traps heat. Human activities also release heat directly.
*   **How We Measure/Analyze It Here:** This tool uses various data points to analyze local UHI conditions:
    *   **UHI Index:** The core target variable, representing the relative temperature difference at a specific point compared to a baseline (higher value = stronger local heat effect).
    *   **Weather Variables (Temp, RH, Wind, Solar Flux):** Direct meteorological drivers. Higher temperature and solar radiation generally increase UHI, while wind and sometimes humidity can have cooling effects.
    *   **Elevation (NASADEM):** Higher elevations are often cooler due to atmospheric effects.
    *   **NDVI (Normalized Difference Vegetation Index):** Measures vegetation density and health. Higher NDVI indicates more greenery, which typically **reduces** UHI through shade and evapotranspiration.
    *   **NDBI (Normalized Difference Built-up Index):** Highlights built-up areas (buildings, pavement). Higher NDBI is often associated with **increased** UHI due to heat absorption and retention by artificial surfaces.
    *   **Albedo (Broadband):** Measures surface reflectivity. Low albedo (dark surfaces like asphalt) absorbs more solar energy, contributing to **higher** UHI. High albedo (bright surfaces like white roofs) reflects more energy, helping to **reduce** UHI.
    *   **MNDWI (Modified Normalized Difference Water Index):** Identifies water bodies, which usually have a moderating/cooling effect on their immediate surroundings.
    *   **BSI (Bare Soil Index):** Identifies bare soil areas, which can become very hot when dry, contributing to **higher** UHI.

By visualizing these parameters and providing AI-driven analysis, the tool aims to help users understand *why* certain areas are hotter than others and identify potential avenues for targeted **UHI mitigation**.

---

## Key Features ✨

*   **Multi-Source Data Integration:** Combines UHI Index observations, NYSMesonet weather data (temperature, humidity, wind, solar flux), Sentinel-2 L2A satellite bands, and NASADEM elevation data.
*   **Geospatial Feature Calculation:** Computes key spectral indices (NDVI, NDBI, MNDWI, BSI, Albedo) relevant to understanding UHI drivers.
*   **Interactive Visualization:**
    *   **3D Map (PyDeck):** Displays UHI Index measurement points as vertical columns, with height proportional to the UHI Index. Allows coloring columns based on various features (UHI Index, Temp, RH, NDBI, Albedo) to explore correlations visually.
    *   **2D Map (Folium):** Provides a familiar base map view with data points represented as circles, also color-coded by user-selected variables.
*   **AI-Powered Local Analysis & Mitigation Advice (via Google Gemini):**
    *   Enter specific latitude/longitude coordinates within the study area (Bronx/Manhattan).
    *   Retrieves data for the nearest point and its immediate vicinity (within ~150m).
    *   Generates easy-to-understand explanations of the local heat situation by analyzing the local values of the UHI index, temperature, humidity, NDBI, albedo, etc.
    *   Provides context-specific, actionable **mitigation suggestions** based on the identified local conditions (e.g., suggests greening options if NDVI is low, or cool surfaces if albedo is low and NDBI is high).
*   **Underlying ML Insights:** While the tool doesn't run predictions live, the analysis builds upon research using models like TabPFN, XGBoost, and Random Forest, interpreted using SHAP to understand non-linear driver effects (as detailed in the associated research paper).

---

## Data Sources & Context 💾

*   **Target Variable (UHI Index):** Ground traverse measurements for Bronx & Manhattan on **July 24, 2021 (19:00-19:59 UTC)**. Provided by CAPA Strategies, LLC, hosted by the Center for Open Science (COS). *Note: The dataset is downsampled to 10,000 points for performance.*
*   **Weather Data:** New York State Mesonet (NYSMesonet) stations (Manhattan, Bronx).
*   **Satellite Imagery:** Sentinel-2 Level-2A optical data accessed via **Microsoft Planetary Computer**.
*   **Elevation Data:** NASADEM (derived from SRTM) accessed via **Microsoft Planetary Computer**.

---

## Technical Approach Highlights ⚙️

*   **Data Acquisition & Preprocessing:** Uses `pystac-client` and `planetary-computer` for accessing cloud-based geospatial data. `geopandas`, `rioxarray`, `xarray`, and `pandas` handle data manipulation, CRS transformations, and raster sampling.
*   **Machine Learning Context:** The analysis framework is informed by underlying research comparing Linear Regression, Random Forest, XGBoost, and TabPFN models for UHI Index prediction, achieving high accuracy (R² ≈ 0.78 with TabPFN).
*   **Interpretability:** Leverages insights from SHAP (SHapley Additive exPlanations) analysis performed on the ML models to understand feature contributions and interactions (e.g., NDVI saturation, temperature-solar flux interaction).
*   **Generative AI Integration:** Uses the `google-generativeai` library to interact with the Gemini API, constructing detailed prompts with localized data to generate tailored explanations and recommendations focused on UHI mitigation.
*   **Web Application:** Built with `streamlit` for rapid development of the interactive interface.

---

## Technology Stack 🛠️

*   **Core:** Python 3.9+
*   **Web Framework:** Streamlit
*   **Data Handling:** Pandas, NumPy
*   **Geospatial:** GeoPandas, rioxarray, xarray, shapely
*   **Cloud/API:** pystac-client, planetary-computer, google-generativeai
*   **Visualization:** PyDeck, Folium, Matplotlib (for colormaps)
*   **(For Underlying Research):** scikit-learn, XGBoost, TabPFN, SHAP

---

## Screenshots 📸

*(Consider adding a screenshot or GIF of the application interface here)*
*   *Example: A view showing the 3D map, 2D map, and the AI analysis panel with mitigation suggestions.*

---

## Setup & Installation 🚀

1.  **Clone the Repository:**
    ```bash
    git clone <your-repo-url>
    cd <your-repo-directory>
    ```
2.  **Create a Virtual Environment (Recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate # On Windows use `venv\Scripts\activate`
    ```
3.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
    *(Ensure you have a `requirements.txt` file listing all necessary libraries)*
4.  **Obtain API Key:** Get a Google Gemini API key from [Google AI Studio](https://aistudio.google.com/app/apikey).
5.  **Prepare Data:**
    *   Place the UHI Index CSV file (e.g., `check.csv`) in the root directory or a designated `data` folder.
    *   Place the weather CSV files (e.g., `manhattan.csv`, `bronx.csv`) in the same location. *(Update paths in the script if necessary)*.

---

## Usage ▶️

1.  **Run the Streamlit App:**
    ```bash
    streamlit run app.py
    ```
    *(Replace `app.py` with the actual name of your main script file)*
2.  **Enter API Key:** Paste your Google Gemini API key into the input field in the sidebar.
3.  **Load Data:** Select the desired date (defaults to July 24, 2021) and adjust the max cloud cover if needed. Click the "Load/Refresh Data" button. Wait for the data processing to complete (this may take some time on the first run).
4.  **Explore Maps:**
    *   Interact with the 3D and 2D maps (pan, zoom).
    *   Use the dropdown menus to change the variable used for coloring the map elements (UHI Index, Temp, RH, NDBI, Albedo). The map legend will update accordingly.
    *   Hover over points/columns to see specific data values.
5.  **Get Local Analysis:**
    *   Enter Latitude and Longitude coordinates for a location within Manhattan or the Bronx.
    *   Click the "Get Analysis & Advice" button.
    *   View the AI-generated explanation of the local heat drivers and potential mitigation suggestions in the panel below the coordinate inputs.

---

## Limitations ⚠️

*   **Temporal Scope:** Analysis is based on a single hour on a specific date (July 24, 2021). Findings may not generalize to other times, seasons, or weather conditions.
*   **Geographic Scope:** Focused on the Bronx and Manhattan boroughs of NYC.
*   **Input Features:** Does not include potentially relevant factors like detailed urban morphology (building height/density, Sky View Factor), Land Surface Temperature (LST), or direct anthropogenic heat emissions.
*   **Performance:** Initial data loading and processing, especially satellite data sampling, can be time-consuming. Map rendering with thousands of points requires browser resources.
*   **Mitigation Scope:** Suggestions are generated by AI based *only* on the provided data parameters; they do not account for all real-world constraints (cost, policy, social factors).

---

## Future Work (Ideas) 💡

*   Incorporate multi-temporal data analysis.
*   Integrate LST and urban morphology features.
*   Expand geographic scope or allow user data uploads.
*   Quantify potential impact of suggested mitigation strategies.
*   Implement caching more aggressively for faster reloads.
*   Compare different Generative AI models or refine prompting strategies for mitigation advice.

---

## Acknowledgements 🙏

*   UHI Index data provided by **CAPA Strategies, LLC** and hosted by the **Center for Open Science (COS)**.
*   Weather data from the **New York State Mesonet**.
*   Satellite (Sentinel-2) data courtesy of the **European Space Agency (ESA) Copernicus program**.
*   Elevation (NASADEM) data courtesy of **NASA**.
*   Geospatial data access facilitated by the **Microsoft Planetary Computer**.
*   Project developed as part of the **2025 EY Open Science Data Challenge**.

---


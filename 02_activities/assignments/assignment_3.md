# Data Visualization

## Assignment 3: Final Project

### Requirements:
- We will finish this class by giving you the chance to use what you have learned in a practical context, by creating data visualizations from raw data. 
- Choose a dataset of interest from the [City of Toronto’s Open Data Portal](https://www.toronto.ca/city-government/data-research-maps/open-data/) or [Ontario’s Open Data Catalogue](https://data.ontario.ca/). 
- Using Python and one other data visualization software (Excel or free alternative, Tableau Public, any other tool you prefer), create two distinct visualizations from your dataset of choice.  


- For each visualization, describe and justify: 
    > What software did you use to create your data visualization?
    -R
    > Who is your intended audience? 
    -Toronto residents and visitors who want to explore StrollTO points of interest and understand where they are located.
    -Community organizations, city planning who may be interested in broad patterns of coverage and clustering across the city.

    > What information or message are you trying to convey with your visualization? 
    -The map shows the spatial distribution of StrollTO Points of Interest (POIs) across Toronto. It helps viewers see where POIs cluster (higher-density areas) and where they are more dispersed, making it easier to understand coverage and geographic concentration.

    > What aspects of design did you consider when making your visualization? How did you apply them? With what elements of your plots? 
    -Reducing clutter: I used marker clustering, so many POIs don’t overlap at the same zoom level. Clusters show counts and expand as the user zooms in.
    -Explorability: The map supports pan and zoom, allowing users to explore neighbourhoods of interest.
    -Clear information hierarchy: The basemap provides context (streets/landmarks), while POI markers remain the main visual layer. Details are shown when clicks a marker.


    > How did you ensure that your data visualizations are reproducible? If the tool you used to make your data visualization is not reproducible, how will this impact your data visualization? 
    -The visualization is reproducible because it is generated from an R script that documents the full workflow: 

library(readr)
library(dplyr)
library(jsonlite) #Geometry中的GeoJSON
library(leaflet) 
library(htmlwidgets) 


df <- read_csv("~/Library/CloudStorage/OneDrive-UniversityofToronto/Data visulization class:Data Science Certificate/visualization/StrollTO POI Details - 4326.csv", show_col_types = FALSE)

#Extract lon/lat from GeoJSON geometry string ----
extract_lonlat <- function(geom_str) {
  if (is.na(geom_str) || geom_str == "") return(c(lon = NA_real_, lat = NA_real_)) #如果 geometry 是缺失值 NA 或空字符串，就直接返回 lon 和 lat 都是 NA。
  g <- tryCatch(fromJSON(geom_str), error = function(e) NULL)
  if (is.null(g) || is.null(g$coordinates)) return(c(lon = NA_real_, lat = NA_real_))

  coords <- g$coordinates 

 
  if (is.numeric(coords) && length(coords) >= 2) {
    return(c(lon = coords[1], lat = coords[2]))
  }
  if (is.list(coords) && length(coords) >= 1 && is.numeric(coords[[1]]) && length(coords[[1]]) >= 2) {
    return(c(lon = coords[[1]][1], lat = coords[[1]][2]))
  }

  c(lon = NA_real_, lat = NA_real_)
}

lonlat <- t(vapply(df$geometry, extract_lonlat, numeric(2)))
df$lon <- lonlat[, "lon"]
df$lat <- lonlat[, "lat"]

df_map <- df %>%
  filter(!is.na(lat), !is.na(lon))

# Build interactive map ----
center_lat <- mean(df_map$lat)
center_lon <- mean(df_map$lon)

m <- leaflet(df_map) %>%
  addProviderTiles(providers$OpenStreetMap) %>%
  setView(lng = center_lon, lat = center_lat, zoom = 11) %>%
  addCircleMarkers(
    lng = ~lon, lat = ~lat,
    radius = 4, stroke = FALSE, fillOpacity = 0.8,
    clusterOptions = markerClusterOptions(),
    popup = ~paste0(
      "<b>", Title, "</b><br/>",
      "<b>Ward:</b> ", Ward_Name, "<br/>",
      "<b>Neighbourhood:</b> ", Neighbourhood, "<br/>",
      ifelse(is.na(Link_URL) | Link_URL == "", "", paste0('<a href="', Link_URL, '" target="_blank">More info</a>'))
    )
  )

m  

saveWidget(m, "strollto_pois_map.html", selfcontained = TRUE)

TO open this interactive graph, please copy the following link to the browser '/Users/guanwang/Library/CloudStorage/OneDrive-UniversityofToronto/Data visulization class:Data Science Certificate/visualization/strollto_pois_map.html'

    -If the map were created manually in a tool without a saved workflow, it would be harder to reproduce, or update consistently when the dataset changes.
  

    > How did you ensure that your data visualization is accessible?  
    -I exported the map as an HTML file, so it can be opened in any standard web browser. I also upload it to my github to make it publically visiable.
    

    > Who are the individuals and communities who might be impacted by your visualization?  
    -Residents and visitors may use the map to choose where to explore.
    -Business improvement area may use it for promotion or to advocate for visibility.
    


    > How did you choose which features of your chosen dataset to include or exclude from your visualization? 
    -Included: geometry (coordinates), POI title and ward/neighbourhood fields.
    -Excluded: long descriptive were not displayed to avoid overwhelming the map interface. Records missing usable coordinates were excluded because they cannot be mapped reliably.

    > What ‘underwater labour’ contributed to your final data visualization product?
    -Locating the dataset and reviewing the portal information.
    -Parsing the geometry column into usable longitude/latitude and handling format inconsistencies.
    -Iterating on map design choices (clustering, zoom level) to improve readability and usability.
    -Exporting to HTML and verifying that it opens and functions correctly outside the R environment.












- For each visualization, describe and justify: 
    > What software did you use to create your data visualization?
    -Python
    > Who is your intended audience? 
    -Toronto residents who want to explore local points of interest and walking itineraries; City planners interested in how tourism-related POIs are distributed across the city.

    > What information or message are you trying to convey with your visualization? 
    -The chart shows how StrollTO Points of Interest (POIs) are distributed across Toronto wards between 2024-2025. By aggregating POIs “by ward,” the visualization makes it easy to compare areas and see whether StrollTO POIs are more concentrated in some wards than others. This kind of comparison matters because wards are meaningful civic units for Toronto city planning ,and the pattern can highlight where StrollTO content is more concentrated.

    > What aspects of design did you consider when making your visualization? How did you apply them? With what elements of your plots? 
    -Comparability & ranking: I used a sorted horizontal bar chart so the reader can quickly scan from highest to lowest.
    -Direct labeling: I added value labels (counts) at the end of each bar so readers don’t need to estimate from the axis.
    -A clear title with “Number of StrollTO POIs” and “Ward”.
    -Minimal chart junk: I kept the design simple (length = count) to avoid confusing the message.


    > How did you ensure that your data visualizations are reproducible? If the tool you used to make your data visualization is not reproducible, how will this impact your data visualization? 

    -This plot is reproducible because the workflow is scripted as following:

import pandas as pd
import matplotlib.pyplot as plt

# ---------- Load data ----------
path = "/Users/guanwang/Library/CloudStorage/OneDrive-UniversityofToronto/Data visulization class:Data Science Certificate/visualization/StrollTO POI Details - 4326.csv"

df = pd.read_csv(path)


df_counts = df.dropna(subset=["Ward_Name"]).copy()
df_counts["Ward_Name"] = df_counts["Ward_Name"].astype(str).str.strip()

# ---------- Count POIs by ward ----------
ward_counts = (
    df_counts.groupby("Ward_Name")["_id"]
    .count()
    .sort_values(ascending=False)
    .reset_index(name="poi_count")
)

out_csv = "ward_poi_counts.csv"
ward_counts.to_csv(out_csv, index=False)
print("Saved summary CSV:", os.path.abspath(out_csv))

# ---------- Plot (ranked horizontal bar chart) ----------
plt.figure(figsize=(10, 12))
plt.barh(ward_counts["Ward_Name"], ward_counts["poi_count"])
plt.gca().invert_yaxis()  # highest on top
plt.xlabel("Number of StrollTO POIs")
plt.ylabel("Ward")
plt.title("Number of StrollTO POIs by Ward")

for i, v in enumerate(ward_counts["poi_count"]):
    plt.text(v, i, f" {v}", va="center")

plt.tight_layout()

out_png = "ward_poi_counts_ranked.png"
plt.savefig(out_png, dpi=300)
plt.show()

print(f"Saved bar chart to: {out_png}")



If I used other tools like Tableau or Excel without saving a workbook, reproducibility would drop, although the figure might still be repeatable under manual revision but could be harder for others to audit.

    > How did you ensure that your data visualization is accessible?  
    I ensured the visualization is accessible by making it easy to view and interpret across different devices and user needs. I can publish the figure in PNG format and share it on an open platform like GitHub, or Tableau, so it can be accessed without specialized software.  To support users who may not be able to view the graphic easily, I provided the underlying summary table (POI counts by ward) alongside the figure, and added a short caption that explains what the chart shows.
    
    To open this graph: 
    from PIL import Image
    import matplotlib.pyplot as plt
    import os

    base = "/Users/guanwang/Library/CloudStorage/OneDrive-UniversityofToronto/Data visulization class:Data Science Certificate/visualization"
    img_path = os.path.join(base, "ward_poi_counts_ranked.png")

    img = Image.open(img_path)
    plt.figure(figsize=(10,6))
    plt.imshow(img)
    plt.axis("off")
    plt.show()
    

    > Who are the individuals and communities who might be impacted by your visualization?  
    -Residents and visitors choosing where to explore; Neighbourhood groups and Business Improvement Areas using this to advocate for tourism support.


    > How did you choose which features of your chosen dataset to include or exclude from your visualization? 
    -Included: Ward_Name (grouping unit) and a unique record identifier (to count POIs).
    Excluded: geometry/coordinates and descriptive text fields, because the goal here is distribution by ward, not mapping or qualitative content.
    I also dropped missing ward values and trimmed whitespace so the same ward wasn’t counted under multiple slightly different strings.

    > What ‘underwater labour’ contributed to your final data visualization product?
    -Finding and interpreting the dataset: locating the StrollTO dataset on the City of Toronto Open Data Portal, checking the documentation, and confirming what each field represents (e.g., Ward_Name, POI records).
    Defining the analysis unit: deciding what “by ward” should mean (counting POIs per ward) and choosing an appropriate aggregation method.
    Data cleaning: handling missing Ward_Name values, trimming inconsistent text formatting including extra spaces, punctuation, and checking for potential duplicates that could distort counts.

- This assignment is intentionally open-ended - you are free to create static or dynamic data visualizations, maps, or whatever form of data visualization you think best communicates your information to your audience of choice! 
- Total word count should not exceed **(as a maximum) 1000 words** 
 
### Why am I doing this assignment?:  
- This ongoing assignment ensures active participation in the course, and assesses the learning outcomes: 
* Create and customize data visualizations from start to finish in Python
* Apply general design principles to create accessible and equitable data visualizations
* Use data visualization to tell a story  
- This would be a great project to include in your GitHub Portfolio – put in the effort to make it something worthy of showing prospective employers!

### Rubric:

| Component         | Scoring  | Requirement                                                                 |
|-------------------|----------|-----------------------------------------------------------------------------|
| Data Visualizations | Complete/Incomplete | - Data visualizations are distinct from each other<br>- Data visualizations are clearly identified<br>- Different sources/rationales (text with two images of data, if visualizations are labeled)<br>- High-quality visuals (high resolution and clear data)<br>- Data visualizations follow best practices of accessibility |
| Written Explanations | Complete/Incomplete | - All questions from assignment description are answered for each visualization<br>- Explanations are supported by course content or scholarly sources, where needed |
| Code              | Complete/Incomplete | - All code is included as an appendix with your final submissions<br>- Code is clearly commented and reproducible |

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `23:59 - 11/02/2025`
* The branch name for your repo should be: `assignment-3`
* What to submit for this assignment:
    * A folder/directory containing:
        * This file (assignment_3.md)
        * Two data visualizations 
        * Two markdown files for each both visualizations with their written descriptions.
        * Link to your dataset of choice.
        * Complete and commented code as an appendix (for your visualization made with Python, and for the other, if relevant) 
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/visualization/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-3`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.


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

    -This plot is reproducible because the workflow is scripted in Python(see Appendix).

    If I used other tools like Tableau or Excel without saving a workbook, reproducibility would drop, although the figure might still be repeatable under manual revision but could be harder for others to audit.

    > How did you ensure that your data visualization is accessible?  
    I ensured the visualization is accessible by making it easy to view and interpret across different devices and user needs. I publish the figure in PNG format and share it on an open platform GitHub, so it can be accessed without specialized software.  I also used clear axis labels, a descriptive title, and direct value labels on the bars so readers can interpret the chart without relying on colour alone.To support users who may not be able to view the graphic easily, I provided the underlying summary table (POI counts by ward) in csv formart to improve the findings accessibility. 
    
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


Data source: City of Toronto’s Open Data Portal: https://open.toronto.ca/dataset/strollto/
    
Appendix: 

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

# ---------- ranked horizontal bar chart ----------
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
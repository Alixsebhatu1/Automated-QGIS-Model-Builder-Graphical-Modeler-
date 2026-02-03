# Automated-QGIS-Model-Builder-Graphical-Modeler-

Technical Manual section describing an Automated QGIS Model Builder (Graphical Modeler) version of your cascading workflow.
This is written as an SOP extension.

# 🤖 Automated Workflow Using QGIS Model Builder

## Cascading Shapefile Merging and Attribute Enrichment

## 1. Objective
This document describes how to automate the cascading shapefile merging and CSV-based attribute enrichment workflow 
using the QGIS Graphical Modeler.

The automated model:
* Reduces manual errors
* Ensures repeatability
* Supports batch processing
* Standardizes GIS operations across regions

## 2. Automation Scope

The automated model covers:

✔ Merging multiple polygon shapefiles
✔ Cleaning attributes (retain FID only)
✔ Recalculating FID values
✔ Joining cleaned CSV attributes
✔ Producing a final attribute-enriched shapefile

⚠️ External CSV cleaning in Excel remains a manual pre-processing step.

## 3. Required Inputs (Model Parameters)

| Parameter         | Type                   | Description                              |
| ----------------- | ---------------------- | ---------------------------------------- |
| Input Shapefiles  | Multiple Vector Layers | Polygon layers to be merged              |
| Cleaned CSV Table | Table                  | Attribute table with standardized fields |
| Join Field        | Field                  | `FID`                                    |
| Output CRS        | CRS                    | Target coordinate reference system       |
| Output Shapefile  | Vector Destination     | Final enriched shapefile                 |

---

## 4. Model Design (Graphical Modeler)

### Step 1: Create a New Model

1. Open Processing Toolbox
2. Click Graphical Modeler
3. Model name:
   `Cascading_Merge_and_Attribute_Join`
4. Group:
   `REDD_Plus / Attribute_Standardization`

## 5. Model Components (Algorithm Chain)

### 🔹 Component 1: Merge Vector Layers
Algorithm:
`Merge vector layers`

Inputs:
Input layers: *Multiple polygon shapefiles
Output:

* `Merged_Layer`

### 🔹 Component 2: Refactor Fields (Attribute Reset)
Algorithm:
`Refactor fields`
Purpose:
Remove all fields except `FID`.
Configuration:
* Field mapping:
  * Keep: `FID` (Integer)
  * Remove: All other fields
Output:
 `Cleaned_Merged_Layer`
---
### 🔹 Component 3: Recalculate FID Values
Algorithm:
`Field Calculator`
Configuration:
 Create/update field: `FID`
 Expression:

```qgis
$ID + 1
```
Output field type: Integer

Output:
 `Reindexed_Layer`

### 🔹 Component 4: Join Attributes by Field Value
Algorithm:
`Join attributes by field value`

Configuration:
Input layer: `Reindexed_Layer`
Join layer: Cleaned CSV
Input field: `FID`
Join field: `FID`
Fields to copy: All
Method: Take attributes of the first matching feature
Enable prefix: Yes

Output:
`Attribute_Enriched_Layer`

### 🔹 Component 5: Save Final Output
Algorithm:
`Save vector features`
Output:

Final shapefile (persistent output)
---
## 6. Model Logic Flow (Cascading)
```text
Input Shapefiles
        ↓
Merge Vector Layers
        ↓
Refactor Fields (Keep FID)
        ↓
Field Calculator ($ID + 1)
        ↓
Join Attributes by Field Value (CSV)
        ↓
Final Attribute-Enriched Shapefile
```

---
## 7. Model Validation Checklist

✔ Feature count equals CSV row count
✔ No NULL values in required fields
✔ FID values are sequential
✔ Geometry integrity preserved

---
## 8. Model Execution Instructions

1. Open Processing Toolbox
2. Locate model:
   `REDD_Plus → Attribute_Standardization → Cascading_Merge_and_Attribute_Join`
3. Select:

   Input shapefiles
   Cleaned CSV
   Output file path
4. Click Run

---
## 9. Limitations

| Limitation      | Notes                      |
| --------------- | -------------------------- |
| CSV cleaning    | Must be done externally    |
| Geometry type   | Polygons only              |
| Join dependency | Requires perfect FID match |

---
## 10. Recommended Enhancements

* Add geometry validity check
* Add feature count validation step
* Extend model for batch CSV ingestion
* Export QA/QC report (CSV)

---
## 11. Versioning

* Model file: `Cascading_Merge_and_Attribute_Join.model3`
* QGIS version: 3.x
* Automation level: Semi-automated

---
## 12. License

This model is distributed under the **MIT License**.

---
## 13. Author & Acknowledgements

Developed and maintained by the REDD+ Investment Program Monitoring & Evaluation Geoinformation Specialist, in collaboration with regional GIS specialists and environmental partners.

Author:
Alixander Sebhatu Gebremariam

* LinkedIn: [https://www.linkedin.com/in/alixander-sebhatu-569230118](https://www.linkedin.com/in/alixander-sebhatu-569230118)
* GitHub: [https://github.com/Alixsebhatu1/GeoServer-RIP-Program](https://github.com/Alixsebhatu1/GeoServer-RIP-Program)
* Email: [ledetx2005@gmail.com](mailto:ledetx2005@gmail.com)



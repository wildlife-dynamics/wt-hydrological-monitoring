# Hydrological Monitoring Workflow

## Introduction

This workflow helps you to monitor river health and water quality to understand water resources, manage them sustainably, forecast hazards (floods, droughts), and support environmental and climate change planning. 

**What this workflow does:**
- Downloads hydrological observation data captured by **Stevens Connect** sensors from EarthRanger
- Processes observations including water depth and dissolved oxygen measurements
- Calculates daily summaries (average depth and average dissolved oxygen)
- Exports data in multiple formats (CSV, GeoParquet, GPKG)
- Creates interactive charts showing depth and dissolved oxygen trends over time
- Generates a downloadable Hydrological Report (Word document) with charts and data summary

**Who should use this:**
- Conservation managers monitoring river health in protected areas
- Researchers analyzing water quality data
- Anyone needing to export and visualize Stevens Connect sensor information stored in EarthRanger

## Prerequisites

Before using this workflow, you need:

1. **Ecoscope Desktop** installed on your computer
   - If you haven't installed it yet, please follow the installation instructions for Ecoscope Desktop

2. **EarthRanger Data Source** configured in Ecoscope Desktop
   - You must have already set up a connection to your EarthRanger server
   - Your data source should be configured with proper authentication credentials
   - You'll need to know the name of your configured data source (e.g., "gmmf")

3. **Subject Group with Stevens Connect Sensors** set up in EarthRanger
   - You need to have at least one subject group configured with Stevens Connect sensor subjects in your EarthRanger system
   - You'll need to know the exact name of the subject group containing your hydrological sensors
   - You can find them at https://<your-site>.pamdas.org/admin/observations/subjectgroup/

## Installation

1. Open Ecoscope Desktop
2. Select "Workflow Templates"
3. Click "+ Add Template"
4. Copy and paste this URL https://github.com/wildlife-dynamics/wt-hydrological-monitoring and wait for the workflow template to be downloaded and initialized
5. The template will now appear in your available template list

## Configuration Guide

Once you've added the workflow template, you'll need to configure it for your specific needs. The configuration form is organized into several sections.

### Basic Configuration

These are the essential settings you'll need to configure for every workflow run:

#### 1. Workflow Details
Give your workflow a name and description to help you identify it later.

- **Workflow Name** (required): A descriptive name for this workflow run and the dashboard title
  - Example: `"Hydrological Monitoring Workflow"`
- **Workflow Description** (optional): Additional details about this analysis
  - Example: `"A workflow for hydrological monitoring."`

#### 2. Time Range
Specify the time period for the hydrological data you want to download.

- **Timezone** (required): Use the dropdown to select your timezone
- **Since** (required): Use the calendar picker to select the start date and time
  - Example: `11/20/2025, 12:00 AM`
- **Until** (required): Use the calendar picker to select the end date and time
  - Example: `12/25/2025, 11:59 PM`

#### 3. Data Source
Select your EarthRanger connection.

- **Data Source** (required): Choose from your configured data sources
  - Example: Select `gmmf` from the dropdown

#### 4. Subject Group
Choose which subject group contains your Stevens Connect sensors.

- **Subject Group Name** (required): Enter the exact name of the subject group as it appears in EarthRanger. You can find them at https://<your-site>.pamdas.org/admin/observations/subjectgroup/
  - Example: `"Stevens Connect"`
  - Note: Using a group with mixed subtypes could lead to unexpected results

#### 5. Group Data (Optional)
Organize your data into separate views based on time periods or spatial regions.

- **Group by**: Create separate outputs grouped by:
  - Time: Year, Month, Date, Day of week, Hour, etc.
  - Spatial Regions: Group by geographic regions (if configured)
  - You can select multiple groupers to create nested groupings (e.g., group by month AND region)

#### 6. Persist Observations
Choose how to save your raw observation data with normalized observation details.

- **Filetypes**: Select one or more output formats
  - **CSV**: Standard spreadsheet format, opens in Excel
  - **GeoParquet**: Efficient format for geospatial data
  - **GPKG**: GeoPackage format, opens in GIS software like QGIS
  - Example: Select `CSV` (default)

#### 7. Persist Daily Summary
The workflow automatically creates a daily summary CSV file with aggregated depth and dissolved oxygen data.

- This file is always created and includes:
  - Average daily depth by sensor
  - Average daily dissolved oxygen by sensor

#### 8. Create Hydrological Report
Generate a downloadable Word document (.docx) containing your hydrological analysis.

- **Template Path** (required): Path or URL to a Word template file with Jinja2 placeholders
  - Default: Uses the provided template from GitHub
  - You can provide your own customized template
  - Supports both local file paths and remote URLs (http://, https://)
- The report includes:
  - Report date range
  - Depth chart visualization
  - Dissolved oxygen chart visualization
  - Summary data table

### Advanced Configuration

These optional settings provide additional control over your workflow:

#### Filename Prefixes
Customize the names of your output files.

- **Persist Observations - Filename Prefix**: Custom prefix for raw observation files
  - Default: `"observations"`
  - Example: `"river_observations"` will create files like `river_observations_abc123.csv`

- **Persist Daily Summary - Filename Prefix**: Custom prefix for daily summary files
  - Default: `"daily_summary"`
  - Example: `"river_summary"` will create files like `river_summary_abc123.csv`

## Running the Workflow

Once you've configured all the settings:

1. **Review your configuration**
   - Double-check your time range, data source, and subject group name

2. **Save and run**
   - Click the "Submit" and the workflow will show up in "My Workflows" table button in Ecoscope Desktop
   - Click on "Run" and the workflow will begin processing

3. **Monitor progress and wait for completion**
   - You'll see status updates as the workflow runs
   - Processing time depends on:
     - The size of your date range
     - Number of sensors
     - Number of observations in the system
   - The workflow completes with status "Success" or "Failed"

## Understanding Your Results

After the workflow completes successfully, you'll find your outputs in the designated output folder.

### Data Outputs

Your hydrological data will be saved in the format(s) you selected:

#### Raw Observations Data

- **File formats**: CSV, GeoParquet, and/or GPKG (based on your selection)
- **Opens in**: Microsoft Excel, Google Sheets (CSV), Python/R (GeoParquet), QGIS/ArcGIS (GPKG)
- **Best for**:
  - CSV: Quick data review and analysis
  - GeoParquet: Large datasets, programmatic analysis
  - GPKG: Spatial analysis in GIS software
- **Contents**: All hydrological observation data with normalized observation details including:
  - `sensor`: Name of the sensor (subject name)
  - `recorded_at`: Timestamp when the observation was recorded
  - `date`: Date extracted from recorded_at
  - `DO`: Dissolved oxygen measurement with units
  - `Depth m`: Water depth measurement with units
  - `geometry`: Geographic point location
  - Additional columns from observation details

#### Daily Summary Data

- **File format**: CSV (always created)
- **Filename**: `daily_summary_*.csv` or custom prefix you specified
- **Opens in**: Microsoft Excel, Google Sheets, or any spreadsheet application
- **Contents**: Aggregated daily data with these columns:
  - `date`: Date of the observations
  - `Depth (m)`: Average water depth for that day (meters)
  - `Dissolved Oxygen (mg/L)`: Average dissolved oxygen for that day (mg/L)

### Visual Outputs (Dashboard)

The workflow creates an interactive dashboard with two main visualizations:

#### Daily River Flow Chart
- **Format**: Interactive line chart
- **Features**:
  - X-axis: Date
  - Y-axis: Depth (m)
  - Interactive hover: Shows exact values when you mouse over data points
  - Smooth spline: Shows depth trends as smooth curves
  - Legend: Identifies sensor information

#### Daily Dissolved Oxygen Chart
- **Format**: Interactive line chart
- **Features**:
  - X-axis: Date
  - Y-axis: Average Daily Dissolved Oxygen (mg/L)
  - Interactive hover: Shows exact values when you mouse over data points
  - Smooth spline: Shows dissolved oxygen trends as smooth curves
  - Legend: Identifies sensor information

### Hydrological Report Output

The workflow generates a downloadable Word document (.docx) containing:
- **Report date range**: Summary of the analysis period
- **Depth chart**: Visual representation of water depth trends
- **Dissolved oxygen chart**: Visual representation of dissolved oxygen data
- **Summary table**: Aggregated daily hydrological data

### Grouped Outputs

If you configured data grouping:
- You'll receive separate files for each group (e.g., one per month or time period)
- Dashboard visualizations will have multiple views, with each group selectable from the dashboard

## Common Use Cases & Examples

Here are some typical scenarios and how to configure the workflow for each:

### Example 1: Simple River Health Monitoring
**Goal**: Download hydrological data for all sensors in a subject group

**Configuration**:
- **Time Range**:
  - Since: `2025-11-20T00:00:00`
  - Until: `2025-12-25T23:59:59`
  - Timezone: `UTC (UTC+00:00)`
- **Subject Group Name**: `"Stevens Connect"`
- **Filetypes**: Select `CSV`

**Result**:
- CSV file with all raw observations from all sensors
- Daily summary CSV with aggregated depth and dissolved oxygen
- Interactive dashboard with depth and dissolved oxygen charts
- Hydrological report (Word document) with charts and summary

---

### Example 2: Monthly Water Quality Trends
**Goal**: Analyze water quality data grouped by month

**Configuration**:
- **Time Range**:
  - Since: `2025-01-01T00:00:00`
  - Until: `2025-12-31T23:59:59`
  - Timezone: `Africa/Nairobi (UTC+03:00)`
- **Subject Group Name**: `"Stevens Connect"`
- **Group Data**:
  - Select `"%B"` (Month name: January, February, etc.)
- **Filetypes**: Select `CSV` and `GeoParquet`
- **Filename Prefix**: `"river_health"`

**Result**:
- 12 sets of observation files (one per month)
- 12 daily summary files (one per month)
- Dashboard with 12 views showing depth and dissolved oxygen trends for each month
- Hydrological report with charts and summary

---

### Example 3: Short-Term Monitoring
**Goal**: Monitor river conditions for a specific week

**Configuration**:
- **Time Range**:
  - Since: `2025-11-20T00:00:00`
  - Until: `2025-11-27T23:59:59`
  - Timezone: `UTC (UTC+00:00)`
- **Subject Group Name**: `"Stevens Connect"`
- **Filetypes**: Select `CSV`

**Result**:
- CSV file with one week of hydrological data
- Daily summary showing 7 days of aggregated measurements
- Dashboard charts showing weekly trends in depth and dissolved oxygen
- Hydrological report with charts and summary

## Troubleshooting

### Common Issues and Solutions

#### Workflow fails to start
**Problem**: The workflow won't run or immediately fails

**Solutions**:
- Verify your EarthRanger data source is properly configured
- Check that you have network connectivity to the EarthRanger server
- Ensure your credentials haven't expired
- Confirm the data source name matches exactly

#### No observations returned
**Problem**: Workflow completes but produces empty results

**Solutions**:
- Verify the date range is correct (start date should be before end date)
- Check that the subject group name is spelled exactly as it appears in EarthRanger
- Visit `https://<your-site>.pamdas.org/admin/observations/subjectgroup/` to confirm subject group names
- Verify the subject group contains Stevens Connect sensor subjects with observations during the specified time range
- Try a broader date range to verify observations exist

#### Workflow runs very slowly
**Problem**: The workflow takes an extremely long time to complete

**Solutions**:
- Reduce the date range to smaller time periods
- Process data in smaller batches (by week or month instead of year)
- Consider using only CSV format instead of multiple formats
- The first run may take longer as the environment gets warmed up. The following ones should be faster.

#### Authentication errors
**Problem**: Errors related to login or permissions

**Solutions**:
- Re-configure your EarthRanger data source in Ecoscope Desktop
- Verify your user account has permission to access subject data in EarthRanger
- Check that your account has permission to view the specific subject group

#### Charts are empty or not showing data
**Problem**: Dashboard displays but charts are blank

**Solutions**:
- Verify your observations have valid data in the DO (dissolved oxygen) and Depth m fields
- Check that the daily summary was created successfully
- Ensure the time range contains enough data points for meaningful visualization
- Check if observations were filtered out completely

#### Missing depth or dissolved oxygen data
**Problem**: Some values are missing in the output

**Solutions**:
- Verify the Stevens Connect sensors are recording both depth and dissolved oxygen measurements
- Check the observation_details field in the raw data to see which measurements are present
- Some sensors may only provide partial data - this is normal and will show as empty/null values
- The daily summary will calculate averages only from available data points

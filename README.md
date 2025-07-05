# Geog576_Midterm_real
 This is an electric company reporting tool, for a client based in Northern Virginia with functionality of add report with choiches such blackout, fire, electrical failure, and obstruction.This reporting tool will enable a quick respond by dispatch center to sending out workers to fix the problems. Basemap choices are streets, terrains, vegetation, and combination of structures and vegetation. The report is gathered at my survey in arcgis online account.

 add the whole completed basic script without a title, a legend, icons, etc:
 <!--
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ALEX GO! - Incident Reporting Map</title>

  <!-- ArcGIS JS API stylesheet 
  <link rel="stylesheet" href="https://js.arcgis.com/4.25/esri/themes/light/main.css">

  <style>
    /* Full-screen map container and UI controls styling */
  
    /* Button and dropdown menu */
   
    /* Positioning for basemap selector, I put it below "add report" button */
    #basemapSelector {
      top: 70px;
    }

    /* Styling the Add Report button, I put a bigger button as the most important tool  */
    #addReportBtn button {
      font-size: 18px;
      padding: 12px 24px;
      background-color: #0079c1;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    #addReportBtn button:hover {
      background-color: #005a87;
    }
  </style>

  <!-- ArcGIS JavaScript API , I use a generic API
  <script src="https://js.arcgis.com/4.25/"></script>
</head>
<body>

  <!-- Button to trigger report form popup 
  <div id="addReportBtn">
    <button onclick="addReport()">Add Report</button>
  </div>

  <!-- Dropdown to change basemap style 
  <div id="basemapSelector">
    <label for="basemapSelect">Basemap:</label>
    <select id="basemapSelect" onchange="changeBasemap(this.value)">
      <option value="streets">Street</option>
      <option value="topo" selected>Terrain</option>
      <option value="satellite">Vegetation (Satellite)</option>
      <option value="hybrid">Structures (Hybrid)</option>
    </select>
  </div>

  <!-- Main map view container 
  <div id="viewDiv"></div>

  <!-- Main application logic using ArcGIS JS API 
  <script>
    require([
      "esri/Map",
      "esri/views/MapView",
      "esri/widgets/Search",
      "esri/widgets/Locate",
      "esri/layers/FeatureLayer",
      "esri/Graphic",
      "esri/geometry/Point"
    ], function(Map, MapView, Search, Locate, FeatureLayer, Graphic, Point) {

      // Create the map and set the initial basemap
      let map = new Map({
        basemap: "topo"
      });

      // Initialize the MapView and center on Northern Virginia
      const view = new MapView({
        container: "viewDiv",
        map: map,
        center: [-77.5, 38.5],  // Longitude, Latitude
        zoom: 9
      });

      // Load the territory boundary layer with 50% opacity
      const territoryLayer = new FeatureLayer({
        url: "https://services.arcgis.com/HRPe58bUyBqyyiCt/arcgis/rest/services/drive_download_20250623T221527Z_1_001/FeatureServer/0",
        opacity: 0.5
      });
      map.add(territoryLayer);

      // Load the incident reports layer (from Survey123 form)
      const incidentLayer = new FeatureLayer({
        url: "https://services.arcgis.com/HRPe58bUyBqyyiCt/arcgis/rest/services/survey123_dbec98d452884e3ca51548ab0ca97c1f_form/FeatureServer/0",
        outFields: ["*"],
        popupTemplate: {
          title: "{report_type}",
          content: "{description}<br>Status: {status}"
        }
      });
      map.add(incidentLayer);

      // Add Search widget for address/location lookup
      const search = new Search({ view: view });
      view.ui.add(search, "top-left");

      // Add Locate widget to find user's current location
      const locate = new Locate({ view: view });
      view.ui.add(locate, "top-left");

      // Move zoom controls to bottom-right corner
      view.ui.move("zoom", "bottom-right");

      // Function to open popup for submitting a new report
      window.addReport = function() {
        view.popup.open({
          title: "New Report",
          content: createReportForm(),
          location: view.center
        });
      };

      // Dynamically generate HTML form inside popup
      function createReportForm() {
        const container = document.createElement("div");
        container.innerHTML = `
          <label>Type:</label><br>
          <select id="reportType">
            <option value="Blackout">Blackout</option>
            <option value="Electrical Failure">Electrical Failure</option>
            <option value="Fire">Fire</option>
            <option value="Obstruction">Obstruction</option>
          </select><br><br>
          <label>Description:</label><br>
          <textarea id="description" rows="4" cols="30"></textarea><br><br>
          <button onclick="submitReport()">Submit</button>
        `;
        return container;
      }

      // Handle form submission and save new report to incident layer
      window.submitReport = function() {
        const type = document.getElementById("reportType").value;
        const desc = document.getElementById("description").value;

        // Use current map center as report location
        const point = new Point({
          longitude: view.center.longitude,
          latitude: view.center.latitude
        });

        // Create a new graphic feature to submit
        const newReport = new Graphic({
          geometry: point,
          attributes: {
            report_type: type,
            description: desc,
            report_time: new Date().toISOString(),
            status: "open"
          }
        });

        // Apply the edit to the hosted feature layer (submit report)
        incidentLayer.applyEdits({
          addFeatures: [newReport]
        }).then(function() {
          alert("Report submitted!");
          view.popup.close();
        });
      };

      // Change the basemap dynamically when the user selects a new one
      window.changeBasemap = function(basemapName) {
        view.map.basemap = basemapName;
      };
    });
  </script>
</body>
</html>
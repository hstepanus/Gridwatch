# Geog576_Midterm_real
# This is a reporting tool with functionality of add report with choiches such blackout, fire, electrical failure, and obstruction. Basemap choices are streets, terrains, vegetation, and combination of structures and vegetation. The report is gathered at my survey in arcgis online account.

# add styles
# add base map
# add report type
# add the whole completed script
<style>
    html, body, #viewDiv {
      padding: 0;
      margin: 0;
      height: 100%;
      width: 100%;
    }
    #addReportBtn, #basemapSelector {
      position: absolute;
      top: 10px;
      right: 10px;
      z-index: 10;
      background: white;
      padding: 10px;
      margin-bottom: 5px;
    }
    #basemapSelector {
      top: 70px;
    }
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

# base map choices :
 <div id="basemapSelector">
    <label for="basemapSelect">Basemap:</label>
    <select id="basemapSelect" onchange="changeBasemap(this.value)">
      <option value="streets" selected>Street</option>
      <option value="topo">Terrain</option>
      <option value="satellite">Vegetation (Satellite)</option>
      <option value="hybrid">Structures (Hybrid)</option>
    </select>
  </div>

  
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
# add the whole script

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ALEX GO! - Incident Reporting Map</title>
  <link rel="stylesheet" href="https://js.arcgis.com/4.25/esri/themes/light/main.css">
  <style>
    html, body, #viewDiv {
      padding: 0;
      margin: 0;
      height: 100%;
      width: 100%;
    }
    #addReportBtn, #basemapSelector {
      position: absolute;
      top: 10px;
      right: 10px;
      z-index: 10;
      background: white;
      padding: 10px;
      margin-bottom: 5px;
    }
    #basemapSelector {
      top: 70px;
    }
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
  <script src="https://js.arcgis.com/4.25/"></script>
</head>
<body>
  <div id="addReportBtn">
    <button onclick="addReport()">Add Report</button>
  </div>

  <div id="basemapSelector">
    <label for="basemapSelect">Basemap:</label>
    <select id="basemapSelect" onchange="changeBasemap(this.value)">
      <option value="streets" selected>Street</option>
      <option value="topo">Terrain</option>
      <option value="satellite">Vegetation (Satellite)</option>
      <option value="hybrid">Structures (Hybrid)</option>
    </select>
  </div>

  <div id="viewDiv"></div>

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

      let map = new Map({
        basemap: "streets"
      });

      const view = new MapView({
        container: "viewDiv",
        map: map,
        center: [-77.5, 38.5],
        zoom: 9
      });

      const territoryLayer = new FeatureLayer({
        url: "https://services.arcgis.com/HRPe58bUyBqyyiCt/arcgis/rest/services/drive_download_20250623T221527Z_1_001/FeatureServer/0",
        opacity:0.5
      });
      map.add(territoryLayer);

      const incidentLayer = new FeatureLayer({
        url: "https://services.arcgis.com/HRPe58bUyBqyyiCt/arcgis/rest/services/survey123_dbec98d452884e3ca51548ab0ca97c1f_form/FeatureServer/0",
        outFields: ["*"],
        popupTemplate: {
          title: "{report_type}",
          content: "{description}<br>Status: {status}"
        }
      });
      map.add(incidentLayer);

      const search = new Search({ view: view });
      view.ui.add(search, "top-left");

      const locate = new Locate({ view: view });
      view.ui.add(locate, "top-left");

      view.ui.move("zoom", "bottom-right");

      window.addReport = function() {
        view.popup.open({
          title: "New Report",
          content: createReportForm(),
          location: view.center
        });
      };

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

      window.submitReport = function() {
        const type = document.getElementById("reportType").value;
        const desc = document.getElementById("description").value;

        const point = new Point({
          longitude: view.center.longitude,
          latitude: view.center.latitude
        });

        const newReport = new Graphic({
          geometry: point,
          attributes: {
            report_type: type,
            description: desc,
            report_time: new Date().toISOString(),
            status: "open"
          }
        });

        incidentLayer.applyEdits({
          addFeatures: [newReport]
        }).then(function() {
          alert("Report submitted!");
          view.popup.close();
        });
      };

      window.changeBasemap = function(basemapName) {
        view.map.basemap = basemapName;
      };
    });
  </script>
</body>
</html>

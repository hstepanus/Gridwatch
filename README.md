# Geog576_Midterm_real
# add styles
# add base map
# add report type
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

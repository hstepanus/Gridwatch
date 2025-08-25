# Geog576_Final Project
 This is an electric company reporting tool, for a client based in Northern Virginia with functionality of add report with choiches such blackout, fire, electrical failure, and obstruction.This reporting tool will enable a quick respond by dispatch center to sending out workers to fix the problems. Basemap choices are streets and combination of structures and vegetation. The report is gathered at my survey in arcgis online account.

Code.
The current implementation is built in HTML, CSS, and JavaScript using the ArcGIS API for JavaScript (v4.29). The index.html script integrates three main components:

   Survey123 Form Embed – A Survey123 web form is embedded in an <iframe> so users can submit outage reports. Submissions are automatically written to an AGOL Feature Layer.

   Interactive Map – The map displays three data layers: (a) Survey123 “Field Reports,” clustered as points; (b) the NOVEC service territory polygon shapefile, published to AGOL; and (c) real-time NWS (NOAA) weather alerts, consumed via a GeoJSON feed. The script includes UI features such as filters (Type, Status, Date), feature counts, visibility toggles, and click-for-forecast integration with the NOAA API.

   Styling and UX – CSS is used for a responsive, dashboard-like design. The title is enhanced with gradient text, the legend is pinned at the bottom-right of the viewport, and a compact visibility switcher sits next to map zoom controls.

Data.
The application integrates multiple data sources:

   Survey123 Feature Layer (user-generated outage and incident reports).

   NOVEC service boundary shapefile, converted and hosted in AGOL as a Feature Layer.

   NOAA/NWS alerts feed, providing live severe weather warnings.

   Optional environmental data, such as NDVI, can also be added through AGOL services.

Architecture.
Instead of a traditional AWS three-tier stack, the project architecture is serverless and cloud-native:

   Frontend/UI: GitHub Pages hosts the static HTML/JS/CSS client.

   Data Layer: ArcGIS Online provides hosted Feature Layers and Survey123 services.

   APIs/External Feeds: NOAA Weather API supplies real-time weather alerts and point forecasts.

   User Interaction: Reports are written via Survey123 into AGOL (database equivalent), queried back into the map, and filtered dynamically in the client.

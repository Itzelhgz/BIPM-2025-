# BIPM-2025-
Business Intelligence and Process Management 

This report contains important information about our classes in WiSe2025


Generate inmteractive map and save as standalone html page.

(Try different tile servers)

import folium
from pyproj import Geod

geod = Geod(ellps="WGS84")

# Define stops
stops = [
    {"city": "Mexico City, Mexico", "lat": 19.41, "lon": -99.14, "popup": "Birthplace (1996-1997) 👶🏻"},
    {"city": "Mexico State, Mexico", "lat": 19.35, "lon": -99.70, "popup": "School (1997-2015) 📚🚌"},
    {"city": "Puebla, Mexico", "lat": 19.04, "lon": -98.19, "popup": "University & Work (2015-2025) 🎓👩🏻‍💻🚗"},
    {"city": "Berlin, Germany", "lat": 52.52, "lon": 13.40, "popup": "HWR BIPM (2025-present) 🇩🇪"},

]

# Create map
m = folium.Map(
    location=[40, 0],
    zoom_start=2.3,
    tiles="https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png",
    attr="© OpenStreetMap contributors © CARTO"
)

# Add markers
for stop in stops:
    folium.Marker(
        [stop["lat"], stop["lon"]],
        popup=stop["popup"],
        tooltip=stop["city"],
        icon=folium.Icon(color="blue", icon="info-sign")
    ).add_to(m)

# Function to compute many intermediate great-circle points
def great_circle_points(lat1, lon1, lat2, lon2, npts=100):
    points = geod.npts(lon1, lat1, lon2, lat2, npts)
    coords = [(lat1, lon1)] + [(lat, lon) for lon, lat in points] + [(lat2, lon2)]
    return coords

# Draw arcs between consecutive stops
for i in range(len(stops) - 1):
    start, end = stops[i], stops[i + 1]
    arc = great_circle_points(start["lat"], start["lon"], end["lat"], end["lon"], npts=60)
    folium.PolyLine(
        arc,
        color="darkblue",
        weight=3,
        opacity=0.7
    ).add_to(m)

# Save and open
m.save("academic_journey_map.html")
import webbrowser; webbrowser.open("academic_journey_map.html")

m


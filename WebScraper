import requests
import json
from datetime import datetime

# USGS Earthquake Feed URL (Summary of past earthquakes)
usgs_url = "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_day.geojson"

# Function to convert Unix timestamp to a human-readable date and time
def convert_timestamp_to_datetime(timestamp_ms):
    timestamp_sec = timestamp_ms / 1000
    return datetime.utcfromtimestamp(timestamp_sec).strftime('%Y-%m-%d %H:%M:%S')

# Function to fetch earthquake data from USGS and extract relevant statistics
def fetch_earthquake_data_from_usgs():
    earthquake_data = []
    try:
        response = requests.get(usgs_url)
        response.raise_for_status()  # Ensure valid response

        # Parse the GeoJSON data
        data = response.json()

        # Loop through each earthquake event
        for feature in data['features']:
            properties = feature['properties']
            geometry = feature['geometry']

            earthquake = {
                "Type": "Earthquake",
                "Place": properties.get('place', 'Unknown'),
                "Magnitude": properties.get('mag', 'Unknown'),
                "StartTime": convert_timestamp_to_datetime(properties['time']),
                "EndTime": "Unknown",  # Placeholder for end time, which is often unavailable
                "Coordinates": {
                    "Longitude": geometry['coordinates'][0],
                    "Latitude": geometry['coordinates'][1],
                    "Depth (km)": geometry['coordinates'][2]
                },
                "Deaths": properties.get('deaths', 'Unknown'),
                "Displaced": properties.get('displaced', 'Unknown'),
                "IntensityMetric": f"Magnitude: {properties.get('mag', 'Unknown')}",
                "Duration": "Unknown Duration"  # Placeholder for duration
            }
            earthquake_data.append(earthquake)

    except requests.exceptions.RequestException as e:
        print(f"Failed to retrieve earthquake data from USGS API: {e}")

    return earthquake_data

# Function to save earthquake data as JSON
def save_data_as_json(data, filename):
    with open(filename, 'w') as json_file:
        json.dump(data, json_file, indent=4)
    print(f"Data has been saved to {filename}")

# Main function
def main():
    # Fetch earthquake data from USGS API
    earthquake_data = fetch_earthquake_data_from_usgs()

    if earthquake_data:
        # Save the earthquake data as JSON
        save_data_as_json(earthquake_data, 'earthquake_data.json')
    else:
        print("No earthquake data was retrieved.")

if __name__ == "__main__":
    main()

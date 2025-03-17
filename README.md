# realtimetrack
Real-Time Delivery Tracking System
A comprehensive web application for tracking deliveries in real-time, similar to food delivery apps like Zomato. This system visualizes the movement of a delivery item from origin to destination on an interactive map.
Table of Contents

Features
Demo
Technology Stack
Prerequisites
Installation
Project Structure
Configuration
Usage
API Documentation
Socket Events
Customization
Troubleshooting
Contributing
License

Features

Real-time Location Tracking: View delivery movement from origin to destination in real-time
Interactive Map Interface: Powered by Leaflet.js with custom markers and routes
Origin and Destination Input: Support for both address input and coordinate selection
Progress Visualization: Live progress bar and ETA calculations
Responsive Design: Works on both desktop and mobile devices
Geocoding Integration: Convert addresses to coordinates using maps.gomaps.pro API
Customizable Markers: Different markers for origin, destination, and delivery vehicle

Demo
When running, the application will look like this:
Show Image
Technology Stack

Backend:

Node.js (v14+)
Express.js (v4.18.2)
Socket.io (v4.7.2)
EJS template engine (v3.1.9)


Frontend:

HTML5/CSS3
JavaScript (ES6+)
Leaflet.js for map rendering
Socket.io client for real-time communication


Development Tools:

Nodemon for automatic server restarts
Dotenv for environment variable management



Prerequisites

Node.js (v14 or higher)
npm (v6 or higher)
A maps.gomaps.pro API key

Installation

Clone the repository:
bashCopygit clone https://github.com/yourusername/real-time-tracking-system.git
cd real-time-tracking-system

Install dependencies:
bashCopynpm install

Create environment variables file:
bashCopycp .env.example .env

Add your maps.gomaps.pro API key to the .env file:
CopyMAP_API_KEY=AlzaSyILn4tQ43sZeGmgDIyzbl9KLF7R8i-O2Tb

Start the development server:
bashCopynpm run dev

Access the application:
Open your browser and navigate to http://localhost:3000

Project Structure
Copyreal-time-tracking-system/
├── app.js                # Main application file
├── package.json          # Project dependencies
├── .env                  # Environment variables
├── public/               # Static files
│   ├── css/              # Stylesheets
│   │   └── style.css     # Main stylesheet
│   └── js/               # Client-side JavaScript
│       └── map.js        # Map initialization and socket handling
├── views/                # EJS templates
│   ├── index.ejs         # Main page with map
│   └── partials/         # Reusable components
│       ├── header.ejs    # Page header
│       └── footer.ejs    # Page footer
└── routes/               # Express routes
    └── index.js          # Main routes
Configuration
Environment Variables
VariableDescriptionDefaultPORTPort number for the server3000MAP_API_KEYAPI key for maps.gomaps.pro-NODE_ENVEnvironment modedevelopment
Map Configuration
You can customize the map by modifying the following in public/js/map.js:

Default coordinates: Change defaultLat and defaultLng variables
Zoom level: Modify the value in map.setView([defaultLat, defaultLng], 13)
Map tiles: Replace the tile layer URL if needed
Marker icons: Customize the icon URLs and sizes

Usage
Basic Usage

Navigate to the application URL
Enter origin and destination addresses or coordinates
Click "Start Tracking" to begin the simulation
Watch as the delivery marker moves in real-time from origin to destination
View delivery progress, ETA, and remaining distance in the info panels

Address Input
You can either:

Type an address in the input field and it will be geocoded to coordinates
Directly enter latitude and longitude values

Tracking Information
The application provides:

Current status of the delivery
Progress percentage with visual progress bar
Estimated time of arrival (ETA)
Current location coordinates
Distance remaining to destination
Timestamp of the last update

API Documentation
Endpoints
MethodEndpointDescriptionGET/Renders the main tracking pagePOST/start-trackingInitiates a new tracking sessionPOST/update-locationUpdates the current location (for testing)
Request/Response Examples
Start Tracking
Request:
jsonCopyPOST /start-tracking
{
  "origin": {
    "lat": 40.7128,
    "lng": -74.0060
  },
  "destination": {
    "lat": 40.7800,
    "lng": -73.9800
  }
}
Response:
jsonCopy{
  "success": true,
  "trackingId": "TRK1647802489123",
  "message": "Tracking started successfully"
}
Update Location
Request:
jsonCopyPOST /update-location
{
  "trackingId": "TRK1647802489123",
  "position": {
    "lat": 40.7350,
    "lng": -73.9900
  }
}
Response:
jsonCopy{
  "success": true,
  "message": "Location updated successfully"
}
Socket Events
Client to Server
EventDescriptionDatastart_trackingInitiates tracking between points{ origin: {lat, lng}, destination: {lat, lng} }
Server to Client
EventDescriptionDatatracking_startedConfirms tracking has begun{ status: 'success' }location_updateBroadcasts location updates{ position: {lat, lng}, progress: percentage }tracking_completeNotifies when delivery is complete{ message: 'Delivery completed!', finalPosition: {lat, lng} }
Customization
Simulation Speed
Modify the update interval in app.js:
javascriptCopy// Update position every 1 second (1000ms)
const interval = setInterval(() => {
  // ...
}, 1000);
Change 1000 to a different value (in milliseconds) to adjust the speed.
Movement Steps
Adjust the number of steps in the simulation:
javascriptCopyconst totalSteps = 20; // Number of steps to simulate movement
Increase for smoother movement or decrease for faster delivery.
Marker Customization
In public/js/map.js, modify the marker icons:
javascriptCopyconst originIcon = L.icon({
    iconUrl: 'https://cdn.rawgit.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-green.png',
    // ...
});
Replace the iconUrl with custom marker images.
Troubleshooting
Common Issues

Map Not Loading: Check if your API key is correct in the .env file
Socket Connection Failed: Ensure your server is running and accessible
Geocoding Not Working: Verify the correct endpoint is being used for maps.gomaps.pro

Debugging
Add console logs to debug socket events:
javascriptCopysocket.on('location_update', (data) => {
    console.log('Received location update:', data);
    // ...
});
Contributing

Fork the repository
Create a feature branch: git checkout -b feature-name
Commit your changes: git commit -m 'Add some feature'
Push to the branch: git push origin feature-name
Submit a pull request

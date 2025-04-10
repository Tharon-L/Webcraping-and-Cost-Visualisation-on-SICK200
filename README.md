# Webcraping-and-Cost-Visualisation-on-SICK200
This is a project for a programming course at TUKE for a teacher. The idea is to Visual the cost of a multi-sensors installation in his lab.


# Sensor Power Monitor Application

## Technical Documentation

----------

## Table of Contents

1.  [Introduction]
2.  [Application Overview]
3.  [Web Scraping Component]
4.  [Main Features]
5.  [Data Flow]
6.  [User Interface]
7.  [Installation and Setup]
8.  [Bibliography]

----------

## Introduction

The Sensor Power Monitor Application is a Python-based utility designed to monitor and analyze the power consumption of industrial sensors. The application provides real-time visualization of sensor performance, cost calculations, and data logging capabilities. It is particularly valuable for industrial automation environments where monitoring energy usage and associated costs is critical for operational efficiency.

----------

## Application Overview

The application connects to sensors via a network connection, retrieves their identification information, and monitors their power consumption. It calculates costs based on current electricity prices that are automatically retrieved from the web. The application features a graphical user interface with real-time plots, status indicators, and control buttons.

**[SCREENSHOT PLACEHOLDER: Main application interface showing plots and control panel]**

----------

## Web Scraping Component

### Purpose

The web scraping component automatically retrieves current electricity prices from a public source. This enables the application to calculate accurate cost estimates for sensor power consumption without requiring manual price updates.

### Connection to OneNote Documentation

For more detailed information about the web scraping implementation, refer to the following OneNote documentation:

[One Note Link](https://1drv.ms/o/c/b530143e84dc7ae9/EiqUg1K7wRBCqQaPBpfsMNoBUi0lvzd4M3NxwrbIOfr5VQ?e=KFwlYw)

This documentation contains additional details on:

-   Web scraping implementation specifics
-   Troubleshooting common issues
-   Performance considerations
-   Future enhancement plans

**[SCREENSHOT PLACEHOLDER: OneNote documentation page showing web scraping details]**

### Implementation Details

The web scraping functionality is implemented in the `scrape_electricity_price` method of the `SensorMonitorApp` class. Here's how it works:

1.  The method makes an HTTP request to the specified electricity price website
2.  It uses BeautifulSoup to parse the HTML content
3.  It locates the relevant table containing price information
4.  It searches for rows containing "Price" in the first column
5.  It extracts the price value using a regular expression
6.  The price is returned as a floating-point number in €/kWh units

```python
def scrape_electricity_price(self, url):
    """Scrapes electricity price from the given URL."""
    try:
        response = requests.get(url, timeout=10)
        soup = BeautifulSoup(response.content, 'html.parser')
        table = soup.find('table', class_='table table-sm mb-3')
        
        if table:
            for row in table.find_all('tr'):
                cols = row.find_all('td')
                if len(cols) >= 2 and 'Price' in cols[0].text.strip():
                    match = re.search(r'(\d+\.\d+)\s*€/kWh', cols[1].text.strip())
                    if match:
                        return float(match.group(1))
        return None
    except Exception as e:
        print(f"Error scraping price: {e}")
        return None

```

### Error Handling

The scraping function includes error handling to ensure the application continues to function even if the web scraping fails:

1.  If the HTTP request fails, the method catches the exception and returns None
2.  If the website structure changes and the table or price information cannot be found, the method returns None
3.  When None is returned, the application keeps using the last successfully scraped price or the default price

**[SCREENSHOT PLACEHOLDER: Example of error handling when website is unavailable]**

----------

## Main Features

### Sensor Initialization

-   Retrieves sensor identification information via API
-   Automatically categorizes sensors based on their type
-   Assigns default values if API connection fails

**[SCREENSHOT PLACEHOLDER: Sensor initialization process]**

### Power Consumption Monitoring

-   Measures current draw from each sensor (in mA)
-   Calculates power consumption (in W) using fixed voltage (24V)
-   Updates readings at regular intervals

### Cost Calculation

-   Retrieves electricity prices from the web
-   Calculates real-time operating costs for each sensor
-   Displays hourly cost in euros (€/h)

**[SCREENSHOT PLACEHOLDER: Cost calculation display]**

### Data Visualization

-   Real-time graphs showing cost trends
-   Color-coded plots for easy sensor identification
-   Automatic scaling to accommodate varying cost levels

### Data Logging

-   Saves sensor data to CSV files
-   Includes timestamps for each data point
-   Stores files in a designated directory on the desktop

----------

## Data Flow

1.  **Initialization Phase**
    
    -   Application starts and configures the interface
    -   Sensors are initialized with identification info from API
    -   Initial electricity price is retrieved from the web
2.  **Monitoring Phase**
    
    -   Sensor current values are obtained
    -   Power and cost calculations are performed
    -   Visualization is updated
    -   Data is stored in memory arrays
3.  **Data Saving Phase**
    
    -   CSV file is generated with timestamp
    -   All sensor data and electricity price are written to file
    -   Confirmation is provided to the user

**[SCREENSHOT PLACEHOLDER: Diagram showing data flow through the application]**

----------

## User Interface

The application features a user-friendly interface with:

-   **Control Buttons:**
    
    -   Initialize Sensors: Connects to sensors and retrieves identification information
    -   Start/Stop: Controls the monitoring process
    -   Save Data: Exports current data to a CSV file
    -   Update Price: Manually triggers an electricity price update
-   **Status Indicators:**
    
    -   Connection status for each sensor
    -   Monitoring status (running, stopped, etc.)
    -   Current electricity price display
-   **Real-time Displays:**
    
    -   Cost graphs for each sensor
    -   Current, power, and cost values for each sensor

**[SCREENSHOT PLACEHOLDER: Annotated interface showing main UI components]**

----------

## Installation and Setup

### Prerequisites

-   Python 3.7 or higher
-   Internet connection for electricity price updates
-   Network access to sensor device (192.168.0.125)

### Required Libraries

-   tkinter: For the graphical user interface
-   numpy: For data processing
-   matplotlib: For visualization
-   requests: For API calls and web scraping
-   BeautifulSoup: For HTML parsing

### Configuration

-   Sensor types and consumption values can be adjusted in the code
-   Default electricity price can be modified if needed
-   IP address of the sensor device can be changed to match your network

**[SCREENSHOT PLACEHOLDER: Installation or configuration process]**

----------

## Bibliography

1.  **Instant Electricity Price Information:**
    
    -   "Energy Prices EU - Slovakia." https://www.energyprices.eu/electricity/slovakia
2.  **Maximum Intensity References:**
    
    -   "Datasheet for sensor1" - [Photoelectric proximity sensor]
    -   "Datasheet for sensor2" - [Position Indicator]
    -   "Datasheet for sensor3" - [Flow Sensor]
    -   "Datasheet for sensor4" - [Inductive proximity sensor]
3.  **Development and Optimization Tools:**
    
    -   "Claude 3.7 Sonnet" - Anthropic
    -   "Deepseek" - DeepSeek AI
4.  **Documentation Organization:**
    
    -   "OneNote" - Microsoft Corporation

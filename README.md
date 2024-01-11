# ISS-tracker
ISS Detector: Python GUI Description: Real-time ISS location tracking app with Tkinter GUI. Fetches data from APIs for ISS location and astronauts. Provides world map integration, data refresh, exact lat-long location . Tech: Python, Tkinter, external APIs, mapping. Usage: GitHub repository, open for contributions.  License: Open-source license

import tkinter as tk
from PIL import Image, ImageTk
import requests

# Create the main application window
root = tk.Tk()
root.title("ISS Tracker")
root.geometry("800x600")
# Create a label for the heading
heading_label = tk.Label(root, text="ISS Tracker", font=("Helvetica", 24, "bold"))
heading_label.grid(row=0, column=0, columnspan=10, padx=260, pady=67, sticky="w")

# Create a canvas for the map
map_canvas = tk.Canvas(root, width=600, height=400, bg="black")
map_canvas.grid(row=0, column=0, rowspan=4, padx=10, pady=10)

# Initialize high-tech and cool theme colors
bg_color = "black"
text_color = "white"
button_font = ("Helvetica", 12)

# Create a global variable to track the current theme
current_theme = "light"

def toggle_theme():
    global current_theme
    if current_theme == "light":
        current_theme = "dark"
        apply_dark_theme()
    else:
        current_theme = "light"
        apply_light_theme()

# Create "Fetch Data" button
def fetch_astronaut_data():
    # Fetch and display astronaut data here
    astronauts = get_astronaut_details()
    display_astronaut_details(astronauts)

fetch_button = tk.Button(root, text="Fetch Data", command=fetch_astronaut_data, font=button_font)
fetch_button.grid(row=3, column=1, padx=10, pady=10, sticky="w")

# Create a text area for astronaut details
astronaut_details = tk.Text(root, height=10, width=40, bg=bg_color, fg=text_color, font=button_font)
astronaut_details.grid(row=0, column=2, rowspan=4, padx=10, pady=10, sticky="nsew")

scrollbar = tk.Scrollbar(root)
astronaut_details.config(yscrollcommand=scrollbar.set)
scrollbar.config(command=astronaut_details.yview)
scrollbar.grid(row=0, column=3, rowspan=4, padx=10, pady=10, sticky="nsew")

# Define and configure other UI elements as needed
# ...

# Function to configure the dark theme
def apply_dark_theme():
    root.configure(bg="black")
    fetch_button.configure(bg="#242424", fg="white")  # Adjust button colors for dark theme
    astronaut_details.configure(bg="#242424", fg="red", font=("Helvetica", 12))  # Adjust text area colors and text color for dark theme
    # Configure other UI elements for the dark theme
    # ...

# Function to configure the light theme
def apply_light_theme():
    root.configure(bg="white")
    fetch_button.configure(bg="#007acc", fg="red")  # Adjust button colors for light theme
    astronaut_details.configure(bg="white", fg="blue", font=("Helvetica", 12))  # Adjust text area colors and text color for light theme
    
    
    
    # Create a theme toggle button
toggle_button = tk.Button(root, text="Toggle Theme", command=toggle_theme)
toggle_button.grid(row=4, column=0, padx=10, pady=10)

# Set the initial theme
apply_light_theme()

 

# Initialize high-tech fonts
title_font = ("Helvetica", 20, "bold")

# Create labels for latitude and longitude
latitude_label = tk.Label(root, text="Latitude: N/A", font=button_font)
latitude_label.grid(row=0, column=1, padx=10, pady=10, sticky="w")

longitude_label = tk.Label(root, text="Longitude: N/A", font=button_font)
longitude_label.grid(row=1, column=1, padx=10, pady=10, sticky="w")

# Create a refresh button
def refresh_iss_location():
    iss_location = get_iss_location()
    latitude_label.config(text=f"Latitude: {iss_location['iss_position']['latitude']}")
    longitude_label.config(text=f"Longitude: {iss_location['iss_position']['longitude']}")
    update_iss_marker(float(iss_location['iss_position']['latitude']), float(iss_location['iss_position']['longitude']))

refresh_button = tk.Button(root, text="Refresh ISS Location", command=refresh_iss_location, font=button_font)
refresh_button.grid(row=2, column=1, padx=10, pady=10, sticky="w")



# Initialize map image (sample image)
map_image = Image.open(r"D:\Projects\PYTHON\image\map.gif")
map_image = ImageTk.PhotoImage(map_image)
map_canvas.create_image(0, 0, anchor=tk.NW, image=map_image)

# Create an ISS marker on the map
iss_marker = None

# Sample function to update ISS marker on the map
def update_iss_marker(latitude, longitude):
    global iss_marker
    if iss_marker:
        map_canvas.delete(iss_marker)  # Remove the previous marker
    # Calculate the position on the map based on latitude and longitude
    x = int((longitude + 180) * (map_canvas.winfo_width() / 360))
    y = int((90 - latitude) * (map_canvas.winfo_height() / 180))
    iss_marker = map_canvas.create_oval(x, y, x + 10, y + 10, fill="red")  # Customize the marker as needed

# Fetch initial ISS location
def get_iss_location():
    url = "http://api.open-notify.org/iss-now.json"
    response = requests.get(url)
    result = response.json()
    return result

# Fetch and update the initial ISS location
iss_location = get_iss_location()
latitude_label.config(text=f"Latitude: {iss_location['iss_position']['latitude']}")
longitude_label.config(text=f"Longitude: {iss_location['iss_position']['longitude']}")
update_iss_marker(float(iss_location['iss_position']['latitude']), float(iss_location['iss_position']['longitude']))

# Function to fetch and return detailed astronaut information
def get_astronaut_details():
    url = "http://api.open-notify.org/astros.json"
    response = requests.get(url)
    result = response.json()
    return result["people"]

# Function to display astronaut details
def display_astronaut_details(astronauts):
    # Display astronaut details in the text area
    astronaut_details.delete(1.0, tk.END)  # Clear existing details
    astronaut_details.insert(tk.END, "Astronaut Details:\n\n")
    for astronaut in astronauts:
        name = astronaut.get('name', 'N/A')
        nationality = astronaut.get('nationality', 'N/A')
        craft = astronaut.get('craft', 'N/A')
        details = f"Name: {name}\nNationality: {nationality}\nCraft: {craft}\n\n"
        astronaut_details.insert(tk.END, details)

# Create a text area for astronaut details
astronaut_details = tk.Text(root, height=10, width=40, bg=bg_color, fg=text_color, font=button_font)
astronaut_details.grid(row=0, column=2, rowspan=4, padx=10, pady=10, sticky="nsew")

scrollbar = tk.Scrollbar(root)
astronaut_details.config(yscrollcommand=scrollbar.set)
scrollbar.config(command=astronaut_details.yview)
scrollbar.grid(row=0, column=3, rowspan=4, padx=10, pady=10, sticky="nsew")

# Configure grid weights to make the layout flexible
for i in range(4):
    root.grid_rowconfigure(i, weight=1)
root.grid_columnconfigure(0, weight=1)
root.grid_columnconfigure(1, weight=1)
root.grid_columnconfigure(2, weight=1)
root.grid_columnconfigure(3, weight=1)

root.mainloop()


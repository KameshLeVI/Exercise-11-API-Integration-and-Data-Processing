# Exercise-11-API-Integration-and-Data-Processing
# Aim:
To create a workflow in UiPath that calls a REST API (e.g., Weather API), processes the JSON response, and writes the extracted data to a database.

## Equipment Required:
UiPath Studio<br>
API Key from a Weather API provider (e.g., OpenWeatherMap)<br>
Database (e.g., SQL Server, MySQL, SQLite)<br>
## Procedure:
### Step 1: Install Required Packages
Open UiPath Studio.<br>
Go to Manage Packages in the top toolbar.<br>
#### Install the following packages:<br>
UiPath.WebAPI.Activities (to call REST APIs)<br>
UiPath.Database.Activities (to interact with databases)<br>
### Step 2: Obtain the Weather API Details
For this example, we’ll use the OpenWeatherMap API:<br>

API Endpoint:<br>
```
https://api.openweathermap.org/data/2.5/weather?q={city_name}&appid={your_api_key}
```
Replace {city_name} with the desired city (e.g., "London") and {your_api_key} with your personal API key from OpenWeatherMap.

### Step 3: Building the UiPath Workflow
#### 1. Create a New Sequence
Open UiPath Studio and create a new sequence called WeatherAPI_to_DB.
#### 2. Add HTTP Request Activity
Search for HTTP Request activity from the activities panel.<br>
Configure it as follows:<br>
Endpoint (URL):<br>
```
https://api.openweathermap.org/data/2.5/weather?q=London&appid=YOUR_API_KEY
```
Method: Set it to GET as we are retrieving data.<br>
Output: Create a variable called responseJSON to store the API response.
#### 3. Deserialize JSON
Use the Deserialize JSON activity to convert the JSON response into a UiPath-readable format.<br>
Input: responseJSON<br>
Output: Create a variable jsonResponse to store the deserialized data.
#### 4. Extract Required Data
Extract specific information like city name, temperature, and humidity from the JSON response.<br>
Use Assign activities to extract values:<br>
city = jsonResponse("name").ToString<br>
temperature = jsonResponse("main")("temp").ToString<br>
humidity = jsonResponse("main")("humidity").ToString<br>
#### 5. Connect to Database
Use Connect activity from the UiPath.Database.Activities package.<br>
Connection String: Based on the type of database you're using (e.g., SQLite, MySQL). Here’s an example for SQLite:<br>
```
Data Source=path_to_your_database;Version=3;
```
Store the connection in a variable called dbConnection.
#### 6. Insert Data into Database
Use Execute Non Query activity to write data into the database.<br>
SQL Query:<br>
```
INSERT INTO WeatherData (City, Temperature, Humidity)
VALUES (@City, @Temperature, @Humidity)
```
In the Parameters section, assign:<br>
@City -> city<br>
@Temperature -> temperature<br>
@Humidity -> humidity
#### 7. Close Database Connection
Use Disconnect activity to close the database connection after writing the data.
## UiPath WorkFlow:
![image](https://github.com/user-attachments/assets/f915c379-d0da-40ec-9f03-2eae56600442)
![image](https://github.com/user-attachments/assets/66c98fed-61ba-4bba-855a-80dce4a6a707)
![image](https://github.com/user-attachments/assets/1dc9056c-4926-4dab-8252-41c8be99c5e8)
![image](https://github.com/user-attachments/assets/497b1635-f5f1-4850-bfb3-72cb1e11bcc6)
![image](https://github.com/user-attachments/assets/edcbc576-8344-4ad4-b236-c1ee7ea51156)
![image](https://github.com/user-attachments/assets/0c0c2685-1389-45b8-bfd9-da7c191c66b1)
![image](https://github.com/user-attachments/assets/fc9c9404-cb15-4cd4-bc7c-ca8d2b169586)
![image](https://github.com/user-attachments/assets/d59758ad-349b-4025-a627-a9687b881fd2)
![image](https://github.com/user-attachments/assets/f1c97786-adad-4398-876f-d2bf9b031821)
![image](https://github.com/user-attachments/assets/1bb578b2-64bd-444b-99f2-362f580ce27d)
![image](https://github.com/user-attachments/assets/910bced3-66c7-4c8c-84d5-4399da691fff)
![image](https://github.com/user-attachments/assets/0361a83c-caa2-46ed-9eb6-a5d1b64c212e)

## Result:
The UiPath workflow successfully calls the weather API, processes the JSON response, and writes data such as city name, temperature, and humidity into the specified database table.

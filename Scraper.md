Job Data Scraper
This project contains a Python script that scrapes job data from Indeed using the RapidAPI service and writes the data to a CSV file. The script searches for "cyber security" jobs in Texas and collects information such as company name, job location, and salary details.
Prerequisites
Before running the script, ensure you have the following installed:
• Python (Version 3.x)
• VSCode (Optional but recommended for development)
Installation
1. 
Clone the Repository:
bash
Copy code
git clone <repository-url>
cd <repository-directory>
2. 
Install Required Packages:
You need to install the requests package if you haven't already. Run:
bash
Copy code
pip install requests
Configuration
The script uses the RapidAPI service, so you need an API key to access the Indeed API. Update the headers section in the script with your API key:
python
Copy code
headers = {
    "x-rapidapi-key": "YOUR_RAPIDAPI_KEY",
    "x-rapidapi-host": "indeed12.p.rapidapi.com"
}
Replace YOUR_RAPIDAPI_KEY with your actual API key.
Script
Here's the script used for scraping job data and saving it to a CSV file:
python
Copy code
import requests
import json
import csv

url = "https://indeed12.p.rapidapi.com/jobs/search"

querystring = {"query":"cyber security","location":"texas","page_id":"1","locality":"us","fromage":"1","radius":"200","sort":"date"}

headers = {
    "x-rapidapi-key": "b617c419cemshf1745e8772a4504p16e01cjsnc7de7c6ebaa0",
    "x-rapidapi-host": "indeed12.p.rapidapi.com"
}

response = requests.get(url, headers=headers, params=querystring)

# Check if the request was successful
if response.status_code == 200:
      # Extract JSON data from the response
      data = response.json()

      # Pretty print the JSON data to understand its structure
      formatted_json = json.dumps(data, indent=4)
      print(formatted_json)  # Print JSON data to console for debugging

      # Specify the CSV file name
      csv_file = 'cyberjobsindeed_data.csv'

      # Open the CSV file for writing
      with open(csv_file, mode='w', newline='', encoding='utf-8') as file:
          writer = csv.writer(file)

          # Write the header row to the CSV file
          writer.writerow(["company_name", "formatted_relative_time", "id", "link", "locality",
                            "location", "pub_date_ts_milli", "salary"])

          # Write each job's data to the CSV file
          for job in data.get('hits', []):  # Use get() to avoid KeyError
                salary_info = job.get('salary', {})
                writer.writerow([
                    job.get('company_name', ''),
                    job.get('formatted_relative_time', ''),
                    job.get('id', ''),
                    job.get('link', ''),
                    job.get('locality', ''),
                    job.get('location', ''),
                    job.get('pub_date_ts_milli', ''),
                    f"{salary_info.get('min', '')}-{salary_info.get('max', '')} {salary_info.get('type', '')}"
                ])

          print(f"Data successfully written to {csv_file}")

else:
    print(f"Error: Unable to fetch data (Status code: {response.status_code})")
Running the Script
To run the script, navigate to the directory containing the script and execute:
bash
Copy code
python script_name.py
Replace script_name.py with the name of your Python file.
License
This project is licensed under the MIT License - see the LICENSE file for details.
 
Feel free to customize the README.md further based on your project's specific needs or additional instructions.
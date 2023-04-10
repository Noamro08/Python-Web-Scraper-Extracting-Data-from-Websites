import requests
from bs4 import BeautifulSoup
import csv

# Prompt user for URL
url = input("Enter the URL to scan: ")

# Make initial request and parse HTML
try:
    ans = requests.get(url).text
    soup = BeautifulSoup(ans, 'lxml')
except requests.exceptions.RequestException as e:
    print("Error: ", e)
    exit()

# Find all links on the page
a1 = soup.find_all("a")
list_of_paths = []
for i in a1:
    try:
        path = i['href']
        if path.startswith("http"):
            pass
        else:
            list_of_paths.append(url + path)
    except:
        pass

# Write output to file
output_file = "output.csv"
with open(output_file, "w", newline="") as file:
    writer = csv.writer(file)
    writer.writerow(["Page URL", "Number of Characters", "Number of Forms", "Number of Links"])

    # Iterate over all pages and extract information
    for u in list_of_paths:
        try:
            answer = requests.get(u).text
            soupi = BeautifulSoup(answer, 'lxml')

            # Extract number of characters on page
            num_chars = len(soupi.text)

            # Extract number of form tags on page
            forms = soupi.find_all("form")
            num_forms = len(forms)

            # Extract number of links on page
            a2 = soupi.find_all("a")
            form2 = []
            for link in a2:
                try:
                    path = link['href']
                    if path.startswith("http"):
                        form2.append(path)
                    else:
                        form2.append(u + path)
                except:
                    pass
            num_links = len(form2)

            # Write information to output file
            writer.writerow([u, num_chars, num_forms, num_links])

        except requests.exceptions.RequestException as e:
            print("Error: ", e)

# Print success message
print(f"Scan complete. Results written to {output_file}")

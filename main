import csv
from datetime import datetime

# Reading ManufacturerList.txt
manufacturer_list = {}
with open('ManufacturerList.txt', 'r') as file:
    reader = csv.reader(file)
    for row in reader:
        item_id = row[0].strip()
        manufacturer = row[1].strip()
        item_type = row[2].strip()
        damaged = row[3].strip() if len(row) > 3 else ""
        manufacturer_list[item_id] = [manufacturer, item_type, damaged]

# Reading PriceList.txt
price_list = {}
with open('PriceList.txt', 'r') as file:
    reader = csv.reader(file)
    for row in reader:
        item_id = row[0].strip()
        price = float(row[1].strip())
        price_list[item_id] = price

# Reading ServiceDatesList.txt
service_dates_list = {}
with open('ServiceDatesList.txt', 'r') as file:
    reader = csv.reader(file)
    for row in reader:
        item_id = row[0].strip()
        service_date = datetime.strptime(row[1].strip(), '%m/%d/%Y')
        service_dates_list[item_id] = service_date

# Merging all data into a full inventory list
full_inventory = []
for item_id in manufacturer_list:
    manufacturer, item_type, damaged = manufacturer_list[item_id]
    price = price_list.get(item_id, 0)
    service_date = service_dates_list.get(item_id, datetime.max)
    full_inventory.append([item_id, manufacturer, item_type, price, service_date, damaged])

# Sorting full inventory by manufacturer
full_inventory.sort(key=lambda x: x[1])

# Writing FullInventory.txt
with open('FullInventory.txt', 'w') as file:
    writer = csv.writer(file)
    for item in full_inventory:
        writer.writerow([item[0], item[1], item[2], item[3], item[4].strftime('%m/%d/%Y'), item[5]])

# Creating and writing inventory list for each item type
item_types = {}
for item in full_inventory:
    item_id, manufacturer, item_type, price, service_date, damaged = item
    if item_type not in item_types:
        item_types[item_type] = []
    item_types[item_type].append([item_id, manufacturer, price, service_date, damaged])

for item_type in item_types:
    item_types[item_type].sort(key=lambda x: x[0])
    file_name = f'{item_type.capitalize()}Inventory.txt'
    with open(file_name, 'w') as file:
        writer = csv.writer(file)
        for item in item_types[item_type]:
            writer.writerow([item[0], item[1], item[2], item[3].strftime('%m/%d/%Y'), item[4]])

# Creating and writing PastServiceDateInventory.txt
past_service_date_inventory = [item for item in full_inventory if item[4] < datetime.now()]
past_service_date_inventory.sort(key=lambda x: x[4])

with open('PastServiceDateInventory.txt', 'w') as file:
    writer = csv.writer(file)
    for item in past_service_date_inventory:
        writer.writerow([item[0], item[1], item[2], item[3], item[4].strftime('%m/%d/%Y'), item[5]])

# Creating and writing DamagedInventory.txt
damaged_inventory = [item for item in full_inventory if item[5].lower() == 'damaged']
damaged_inventory.sort(key=lambda x: x[3], reverse=True)

with open('DamagedInventory.txt', 'w') as file:
    writer = csv.writer(file)
    for item in damaged_inventory:
        writer.writerow([item[0], item[1], item[2], item[3], item[4].strftime('%m/%d/%Y')])


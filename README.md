with open('config.yaml', 'r') as file:
    data = yaml.safe_load(file)

# Step 2: Update the list
new_server = {'name': 'server3', 'ip': '192.168.1.3'}
data['servers'].append(new_server)

# Step 3: Write the updated data back to the YAML file
with open('config.yaml', 'w') as file:
    yaml.dump(data, file, default_flow_style=False)

import pandas as pd

# Create a DataFrame
data = {
    'Name': ['Google', 'OpenAI'],
    'Description': ['Search Engine', 'AI Research'],
    'Other': ['Data1', 'Data2'],
    'URL': ['https://www.google.com', 'https://www.openai.com']
}
df = pd.DataFrame(data)

# Define the path to save the Excel file
file_path = 'example_with_hyperlinks.xlsx'

# Create a Pandas Excel writer using XlsxWriter as the engine
with pd.ExcelWriter(file_path, engine='xlsxwriter') as writer:
    # Write the DataFrame to the Excel file
    df.to_excel(writer, index=False, sheet_name='Sheet1')

    # Access the XlsxWriter workbook and worksheet objects
    workbook  = writer.book
    worksheet = writer.sheets['Sheet1']

    # Loop through the DataFrame and add hyperlinks to the 4th column (index 3)
    for row_num, url in enumerate(df['URL'], start=1):  # start=1 to account for header row
        cell_location = f'D{row_num + 1}'  # Column 'D' is the 4th column, rows start from 2
        worksheet.write_url(cell_location, url, string=url)

print(f"Excel file with hyperlinks saved to {file_path}")

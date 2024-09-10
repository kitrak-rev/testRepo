import pandas as pd
import requests
from requests.auth import HTTPBasicAuth
from dotenv import load_dotenv
import os

# Load environment variables from .env file
load_dotenv()

# Get the Bitbucket token and other necessary details from environment variables
BITBUCKET_TOKEN = os.getenv('BITBUCKET_TOKEN')
BITBUCKET_USER = os.getenv('BITBUCKET_USER')  # Optional if using token-based auth
BITBUCKET_WORKSPACE = os.getenv('BITBUCKET_WORKSPACE')

# Define the Excel file path
EXCEL_FILE_PATH = 'path/to/your/excel/workbook.xlsx'

# Define the Bitbucket API URL base
BITBUCKET_API_BASE_URL = 'https://api.bitbucket.org/2.0'

def create_tag(repo_name, commit_id, tag_name):
    url = f"{BITBUCKET_API_BASE_URL}/repositories/{BITBUCKET_WORKSPACE}/{repo_name}/refs/tags/{tag_name}"
    response = requests.post(url, auth=HTTPBasicAuth(BITBUCKET_USER, BITBUCKET_TOKEN), json={'name': tag_name, 'target': commit_id})
    if response.status_code == 201:
        return response.json()['links']['html']['href']
    else:
        raise Exception(f"Failed to create tag: {response.text}")

def main():
    # Load the Excel file
    with pd.ExcelFile(EXCEL_FILE_PATH) as xls:
        # Read the first sheet into a DataFrame
        df = pd.read_excel(xls, sheet_name=0)

        # Create a new column for tag links
        df['tagLink'] = ''

        # Process each row to create tags and update the DataFrame
        for index, row in df.iterrows():
            repo_name = row['repoName']
            commit_id = row['commitID']
            tag_name = row['tagName']
            
            try:
                tag_link = create_tag(repo_name, commit_id, tag_name)
                df.at[index, 'tagLink'] = tag_link
                print(f"Tag created: {tag_name} at {tag_link}")
            except Exception as e:
                print(f"Error creating tag {tag_name} for repo {repo_name}: {e}")

        # Save the updated DataFrame back to the same Excel file, specifically to sheet0
        with pd.ExcelWriter(EXCEL_FILE_PATH, engine='openpyxl', mode='a', if_sheet_exists='replace') as writer:
            df.to_excel(writer, sheet_name='Sheet0', index=False)

if __name__ == '__main__':
    main()

BITBUCKET_TOKEN=your_bitbucket_token_here
BITBUCKET_USER=your_bitbucket_username_here
BITBUCKET_WORKSPACE=your_bitbucket_workspace_here

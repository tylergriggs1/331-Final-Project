import pandas as pd
from tabulate import tabulate

def load_data(url):
    """Load data from a given URL into a pandas DataFrame."""
    return pd.read_csv(url)

def get_user_filters():
    """Prompt user for city and zip code and return them."""
    city = input("Enter your city: ")
    zipcode = input("Enter your zip code: ")
    return city, zipcode

def filter_data(df, city, zipcode):
    """Filter the DataFrame based on city and zip code and remove specified columns."""
    filtered_df = df[(df['City'] == city) & (df['Zipcode'] == zipcode)]
    # Rename columns
    filtered_df = filtered_df.rename(columns={'Firm_Name': 'Firm Name', 'Work_Phone': 'Phone Number', 'Street_Address': 'Street Address', 'Zipcode': 'Zip Code'})
    columns_to_drop = ['index', 'License_Number', 'Doing_Business_As:', 'Expiration_Date']  # Updated list of columns to remove
    for column in columns_to_drop:
        if column in filtered_df.columns:
            filtered_df = filtered_df.drop(columns=[column])
    return filtered_df

def print_dataframe(df):
    """Print DataFrame in a formatted table style using tabulate."""
    if df.empty:
        print("Sorry, there are no retailers in your area.")
    else:
        print(tabulate(df, headers='keys', tablefmt='psql', showindex=False))

# URLs dictionary for different data sources
urls = {
    'retailers': 'https://docs.google.com/spreadsheets/d/1U-QixIZ8uFoUlmwhfKXaolAI4swy9bjxbg139w11BaA/export?format=csv&gid=2147211158',
}

# Main execution block
def main():
    # Load data for retailers
    df_retailers = load_data(urls['retailers'])
    city, zipcode = get_user_filters()
    filtered_retailers = filter_data(df_retailers, city, zipcode)
    print_dataframe(filtered_retailers)

if __name__ == "__main__":
    main()

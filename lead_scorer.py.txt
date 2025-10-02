import requests
import os

# --- Configuration ---
# NOTE: In a production Latenode flow, the API Key would be securely managed 
# as a secret or passed via the platform's execution context, not hardcoded.
CLEARBIT_API_KEY = os.environ.get('CLEARBIT_API_KEY', 'YOUR_API_KEY_PLACEHOLDER')
CLEARBIT_URL = 'https://company.clearbit.com/v2/companies/find?domain='

def score_lead(domain: str) -> int:
    """
    Pulls firmographic data from Clearbit (or similar API) and calculates a 
    B2B lead score based on predefined criteria.

    Args:
        domain: The domain name of the company (e.g., 'google.com').

    Returns:
        An integer representing the lead score (0-100 scale in this example).
    """
    if not domain or not CLEARBIT_API_KEY:
        print("Error: Domain or API Key is missing.")
        return 0

    try:
        # Fetch data using Basic Auth (API Key is the username, password is empty)
        response = requests.get(
            f'{CLEARBIT_URL}{domain}',
            auth=(CLEARBIT_API_KEY, ''),
            timeout=5  # Set a timeout for robust execution
        )
        response.raise_for_status() # Raises an HTTPError for bad responses (4xx or 5xx)
        data = response.json()
    
    except requests.exceptions.RequestException as e:
        print(f"API Request Error for {domain}: {e}")
        return 0
    except ValueError:
        print(f"Error: Could not parse JSON response for {domain}.")
        return 0

    # --- Scoring Logic ---
    # Total possible score: 100 points
    score = 0

    # 1. Employee Count (Size) - High Value
    employees = data.get('metrics', {}).get('employees', 0)
    if employees > 500:
        score += 40
    elif employees > 100:
        score += 30

    # 2. Industry Vertical - Strategic Value
    industry = data.get('category', {}).get('industry')
    if industry in ['Software', 'FinTech', 'B2B Services']:
        score += 30

    # 3. Location / Market Fit - Geo-Targeting
    location = data.get('location', '')
    if location.startswith('United States') or location.startswith('Canada'):
        score += 30
    
    return score

# --- Example Usage (for local testing) ---
if __name__ == '__main__':
    test_domains = ['salesforce.com', 'startup-blog.io', 'local-bakery.com']
    
    for domain in test_domains:
        final_score = score_lead(domain)
        print(f"Lead Score for {domain}: {final_score}")
        
    print("\n--- Next Step: Plug this script into a Latenode custom code node, passing the lead's domain as the input variable. ---")

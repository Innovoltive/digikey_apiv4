# DigiKey API v4 Python Client

A Python client library for interacting with DigiKey's Product Information API v4. This library makes it easy to search for parts, retrieve product details, pricing information, and more from DigiKey's catalog.

## Features

- Product search by keyword
- Detailed product information retrieval
- Real-time pricing and availability
- Part substitutions and recommendations
- Media and documentation access
- DigiReel pricing calculations
- Package type search by quantity
- Category and manufacturer browsing

## Installation

### Using pip

```bash
pip install git+https://github.com/yourusername/digikey_apiv4.git
```

Or install in development mode:

```bash
git clone https://github.com/yourusername/digikey_apiv4.git
cd digikey_apiv4
pip install -e .
```

## Authentication

The DigiKey API uses OAuth 2.0 for authentication. You'll need to:

1. Register for an API account at [DigiKey API Portal](https://developer.digikey.com/)
2. Create an application to get your Client ID and Client Secret
3. Configure your authentication in your code

## Quick Start

Here's how to search for products:

```python
import dk_api_client
from dk_api_client.rest import ApiException
from pprint import pprint

# Configure authentication
configuration = dk_api_client.Configuration()
configuration.api_key['X-DIGIKEY-Client-Id'] = 'YOUR_CLIENT_ID'
configuration.access_token = 'YOUR_ACCESS_TOKEN'  # OAuth2 token

# Create API instance
api_instance = dk_api_client.ProductSearchApi(dk_api_client.ApiClient(configuration))

# Set locale information
locale_properties = {
    'x_digikey_locale_site': 'US',        # Country site (US, CA, JP, UK, etc.)
    'x_digikey_locale_language': 'en',    # Language (en, ja, de, fr, etc.)
    'x_digikey_locale_currency': 'USD'    # Currency (USD, CAD, JPY, etc.)
}

try:
    # Search for a specific part
    product_details = api_instance.product_details(
        'LM358N',  # Product number to search for
        'YOUR_CLIENT_ID',
        **locale_properties
    )
    pprint(product_details)
    
except ApiException as e:
    print(f"Exception when calling DigiKey API: {e}")
```

## Searching for Parts

### Keyword Search

```python
from dk_api_client.models.keyword_request import KeywordRequest

# Build search request
keyword_request = KeywordRequest(
    keywords="ceramic capacitor 0.1uF",
    record_count=25,
    record_start_position=0,
    sort_option={
        "sort_parameter": "SortByQuantityAvailable",
        "direction": "Descending"
    }
)

# Execute search
search_results = api_instance.keyword_search(
    keyword_request,
    'YOUR_CLIENT_ID',
    **locale_properties
)

# Process results
for product in search_results.products:
    print(f"{product.digi_key_part_number}: {product.product_description}")
```

## Locale Support

DigiKey's API supports multiple locales for different countries, languages, and currencies. Specify these in your API calls:

| Parameter | Description | Examples |
|-----------|-------------|----------|
| x_digikey_locale_site | Country site | US, CA, JP, UK, DE, FR |
| x_digikey_locale_language | Language | en, ja, de, fr, ko |
| x_digikey_locale_currency | Currency | USD, CAD, JPY, EUR |

## Documentation

For complete API documentation:

- [API Endpoints](docs/ProductSearchApi.md)
- [Data Models](docs/)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For questions about the DigiKey API, please visit [DigiKey API Support](https://developer.digikey.com/support).

For issues with this client library, please open an issue on GitHub.


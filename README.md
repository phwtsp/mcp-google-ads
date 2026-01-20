# Google Ads MCP Server

This MCP (Model Context Protocol) server provides tools to interact with the Google Ads API directly from LLM interfaces like Cursor or Claude Desktop. It allows you to query campaign data, analyze search terms, and execute custom GAQL queries.

## Features

This server exposes the following tools:

*   **`google_ads_list_campaigns(customer_id, limit=20)`**:
    Lists enabled campaigns for a given account with key performance metrics such as Impressions, Clicks, Cost (R$), CTR, and Average CPC.
    *   `customer_id`: Account Name (mapped in `accounts.json`) or numeric Customer ID.
    *   `limit`: Max number of campaigns to return.

*   **`google_ads_get_search_terms(customer_id, days=30)`**:
    Retrieves the actual search terms that triggered your ads, sorted by cost. Useful for auditing traffic quality.
    *   `days`: Lookback window in days (default: 30).

*   **`google_ads_run_gaql(customer_id, query)`**:
    Executes a raw GAQL (Google Ads Query Language) query for advanced custom reporting.
    *   `query`: The GAQL query string (e.g., `SELECT campaign.name, metrics.clicks FROM campaign LIMIT 5`).

## Prerequisites

*   Python 3.10 or higher.
*   A Google Ads Manager Account or Standard Account with API Access.
*   Google Ads API Credentials (Developer Token, Client ID, Client Secret, Refresh Token).
*   [`uv`](https://github.com/astral-sh/uv) (recommended) or `pip`.

## Installation

1.  **Clone the repository**:
    ```bash
    git clone https://github.com/phwtsp/mcp-google-ads.git
    cd mcp-google-ads
    ```

2.  **Create a virtual environment**:
    ```bash
    # using uv (fast)
    uv venv
    source .venv/bin/activate

    # using python
    python -m venv venv
    source venv/bin/activate  # On Windows: venv\Scripts\activate
    ```

3.  **Install dependencies**:
    ```bash
    pip install "mcp[cli]" google-ads
    ```
    *Note: The server uses `FastMCP` from the `mcp` package.*

## Configuration

### 1. Environment Variables

You must set the following environment variables. You can do this in your shell configuration or pass them when running the server.

*   `GOOGLE_ADS_DEVELOPER_TOKEN`
*   `GOOGLE_ADS_CLIENT_ID`
*   `GOOGLE_ADS_CLIENT_SECRET`
*   `GOOGLE_ADS_REFRESH_TOKEN`

### 2. Accounts Mapping (`accounts.json`)

To use friendly names instead of numeric Customer IDs, create an `accounts.json` file in the root directory:

```json
{
    "My Client A": "123-456-7890",
    "Another Account": "9876543210"
}
```

The server automatically strips non-numeric characters, so "123-456-7890" becomes "1234567890".

## Usage

### Running with MCP CLI (Recommended)

You can run the server using the MCP CLI inspector to test it interactively:

```bash
mcp dev server.py
```

### Configuring in Cursor / Claude Desktop

Add the server configuration to your MCP settings file (e.g., `~/.cursor/mcp.json` or equivalent).

**Example Configuration:**

```json
{
  "mcpServers": {
    "google-ads": {
      "command": "/path/to/your/venv/bin/python",
      "args": ["/path/to/mcp-google-ads/server.py"],
      "env": {
        "GOOGLE_ADS_DEVELOPER_TOKEN": "your_token",
        "GOOGLE_ADS_CLIENT_ID": "your_client_id",
        "GOOGLE_ADS_CLIENT_SECRET": "your_client_secret",
        "GOOGLE_ADS_REFRESH_TOKEN": "your_refresh_token"
      }
    }
  }
}
```
*Make sure to replace the paths and credentials with your actual values.*

## License

This project is licensed under the MIT License.

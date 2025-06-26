# Troubleshooting and data cleaning approach (Content/troubleshooting_data_cleaning.md)

### 1. Troubleshooting

### 1.1. Rate Limit Errors (403 & 429)

#### Error codes: "403 Forbidden" and "429 Too Many Requests"

Cause: These errors are commonly related to exceeding the number of requests per hour allowed by GitHub's API rate limits.

#### Rate Limits by Authentication Type:

Unauthenticated requests: 60 requests per hour
Authenticated requests: 5.000 requests per hour (with personal access token)

Solutions:

**1. Authenticate with Personal Access Token:** Generate a fine-grained, repository-scoped personal access token from GitHub:

- Navigate to Settings > Developer settings > Personal access tokens
- Generate new token with appropriate permissions
- Use the token in your API requests

**2. Handle Rate Limit Responses:** Use the handle_rate_limit(response) function to:

- Detect rate limit errors
- Calculate appropriate wait time before retrying
- Automatically pause execution when limits are reached

**3. Monitor Rate Limits:** Check remaining requests before making bulk operations to avoid hitting limits unexpectedly.

**Important Note**: The BASE_URL variable should remain unchanged for this implementation.

### 1.2. Authentication Errors (401)

#### Error code: "401 Bad Credentials"

Cause: Invalid or expired authentication token.

Solutions:

**1. Verify Token Validity:** 
- Check that your personal access token is Correctly formatted,
- iss Not expired and
- has appropriate permissions for the requested operations

**2. Check Configuration Variables:** 
- Verify BASE_URL is set correctly (should not be modified).
- Ensure TOKEN variable contains valid authentication token

**3. Regenerate Token:** If token is expired or invalid, generate a new personal access token from GitHub settings.

### 2. Data Cleaning Approach

The API includes several functions for data cleaning and preprocessing, designed to transform raw API responses into structured, analyzable data using pandas DataFrames.

#### 2.1 Repository Data Cleaning

Function: clean_repos_data(repos_list)

Purpose: Processes repository search results into a clean, tabular structure suitable for analysis.

Parameters:

repos_list (required): A repository list created through the search_repositories() function

Output: Pandas DataFrame with cleaned repository data including standardized columns for:

- Repository name
- Owner information
- Star count
- Fork count
- Language
- Description
- Creation date
- Last update date

Function usage:

clean_repo_df = clean_repos_data(repo_1)

<img src="/Content/Images/ex4.png" alt="My Diagram" width="400"/>

#### 2.2 Commit Data Cleaning

Function: clean_commits_data(commits_list)

Purpose: Processes commit data into a structured DataFrame for analysis and visualization.

Parameters:

commits_list (required): A commit list created through the get_repository_commits() function

Output: Pandas DataFrame with cleaned commit data including:

- Commit SHA
- Author name and email
- Commit date
- Commit message
- File changes information

Function usage:

clean_repo_df = clean_repos_data(repo_results)

<img src="/Content/Images/ex5.png" alt="My Diagram" width="400"/>


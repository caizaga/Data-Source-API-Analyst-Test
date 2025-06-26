# API Documentation (Content/api_documentation.md)

### Overview

This documentation covers the GitHub API endpoints and functions used for repository search, commit retrieval, and content access. All endpoints are built on the GitHub REST API v3.

### Base Configuration

Base URL: [https://api.github.com](https://api.github.com)

Note: The BASE_URL variable should remain unchanged for this implementation.

## 1. Endpoints used

### 1.1. Search Public Repositories
Endpoint: GET /search/repositories

Full URL: [//api.github.com/search/repositories](https://api.github.com/search/repositories)

Parameters:
- q (required): Search query (e.g., "python", "artificial inteligence")
- sort (optional): Sort field - stars, forks, updated
- order (optional): Sort order - desc (default), asc
- per_page (optional): Results per page (1-100, default 30)
- page (optional): Page number for pagination

**Note**: The function automatically constructs the endpoint URL using the provided parameters based on client requirements.

### 1.2. Commits
Endpoint: GET /repos/{owner}/{repo}/commits

Full URL: [https://api.github.com/repos/{owner}/{repo}/commits](https://api.github.com/repos/{owner}/{repo}/commits
)

Parameters:
- {owner}: Repository owner (e.g., "Microsoft")
- {repo}: Repository name (e.g., "vscode")
- sha (optional): SHA or branch to start listing commits from
- path (optional): Only commits containing this file path
- author (optional): GitHub username or email address
- since (optional): ISO 8601 date - only commits after this date
- until (optional): ISO 8601 date - only commits before this date
- per_page (optional): Results per page (1-100, default 30)
- page (optional): Page number for pagination

### 1.3. Contents: 
Endpoint: GET /repos/{owner}/{repo}/contents/{path}

Full URL: [https://api.github.com/repos/{owner}/{repo}/contents/{path}](https://api.github.com/repos/{owner}/{repo}/contents/{path}
)

Parameters:
- {owner}: Repository owner (e.g., "microsoft")
- {repo}: Repository name (e.g., "vscode")
- {path}: File or directory path (optional - omit for root directory)
- ref (optional): Branch, tag, or commit SHA (default: main branch)

### 4. Request/response formats and function reserences
  search_repositories(query, sort="stars", order="desc", per_page=30)

Purpose: Searches for repositories, can be sorted and ordered.

Parameters:
  - query (str): The search keywords and filters (e.g. "tetris language:python"). (Uses GitHub search syntax)
  - sort (str, optional): Field to sort results by. Options include: "stars", "forks", "updated". (default: "stars").
  - order (str, optional): Sorting order. Can be "asc" (ascending) or "desc" (descending). (default: "desc").
  - per_page (int, optional): Number of results per page (max: 100). (default: 30).

Example:
  repo_1 = search_repositories("tetris language:assembly", per_page=5, sort = 'name', order="asc")

Output example: 

<img src="/Content/Images/ex1.png" alt="My Diagram" width="400"/>

  get_repository_commits(owner, repo, per_page=30)

Purpose: Searches for commits in the repository.

Parameters:
  - owner (str): The username or organization that owns the repository. (e.g. "kirjavascript")
  - repo (str): The name of the repository to fetch commits from. (e.g. "TetrisGYM")
  - per_page (int, optional): Number of commits to retrieve per page (max: 100). (default: 30).

Example:
  comm_1 = get_repository_commits("kirjavascript", 'TetrisGYM', per_page=1)

Output example: 

<img src="/Content/Images/ex2.png" alt="My Diagram" width="400"/>

  get_repository_contents(owner, repo, path="")

Purpose: Searches for specific items and contents in the repository.

Parameters:
  - owner (str):The username or organization that owns the repository. (e.g. "kirjavascript")
  - repo (str): The name of the repository. (e.g. "TetrisGYM")
  - path (str, optional): The file or directory path inside the repository to retrieve contents from. Use an empty string ("") to get the root directory. (default: "").

Example:
  repo_content1 = get_repository_contents("kirjavascript", 'TetrisGYM', path="README.md")

Output example: 

<img src="/Content/Images/ex3.png" alt="My Diagram" width="400"/>

### 5. Authentication method

GitHub applies different rate limits based on authentication. Unauthenticated requests are limited to 60 per hour, while authenticated requests with a personal access token allow up to 5.000 per hour. To optimize API usage and avoid rate limiting, it's recommended to generate a fine-grained, repository-scoped personal access token from GitHub by navigating to Settings > Developer settings > Personal access tokens.

Response Codes

✅ Success (200): Valid authentication with account information ("login", "id", "avatar_url")

<img src="/Content/Images/ex6.png" alt="My Diagram" width="400"/>

❌ Failure (401): {"message":"Bad credentials","documentation_url":"https://docs.github.com/rest","status":"401"}

<img src="/Content/Images/ex7.png" alt="My Diagram" width="400"/>

### 6. Rate limits

### handle_rate_limit (response) 

Purpose: Handles rate limit responses and calculates wait time for status codes 403 (Forbidden) or 429 (Too Many Requests).

### 7. Pagination strategy

#### paginate_repositories(query, max_pages=3, per_page=30)

Purpose: Handles pagination for bulk repository extraction and advanced analytics.

Parameters:
  - query (str): The search keywords and filters (e.g., "language:python stars:>500"). (Uses GitHub's search syntax)
  - max_pages (int, optional): The maximum number of pages to request. Each page contains up to per_page results. (default: 3)
  - per_page (int, optional): Number of repositories to retrieve per page (max: 100). (default: 30)
This funcuion bsesides loopp paginated is recommenden for bulk extraction and more advanced analytics in terms of fate

### 8. Best practices 

- Use Authentication: Always authenticate to avoid hitting the 60/hour rate limit
- Monitor Rate Limits: Check remaining requests before making bulk operations
- Implement Pagination: Use pagination functions for large datasets
- Efficient Queries: Use specific search filters to reduce unnecessary API calls


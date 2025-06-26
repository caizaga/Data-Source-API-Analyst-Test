# API Documentation (Content/api_documentation.md)

## Endpoints used

### Base URL: All the GitHub API endpoints start with: https://api.github.com
For this particular task the variable "BASE_URL" should not be modified.

### Search Repositories (public): for this particular the corresponding Endpoint is: GET /search/repositories
which full URL is https://api.github.com/search/repositories

- Parameters: for all the endpoints it is necessary to consider required and optional parameters:
q (required): Search query (e.g., "python", "artificial inteligence")
sort (optional): Sort field - stars, forks, updated
order (optional): Sort order - desc (default), asc
per_page (optional): Results per page (1-100, default 30)
page (optional): Page number for pagination

"For this API, you don’t need to manually construct the endpoint URL. The function internally builds it using the provided parameters, depending on the client's requirements."

### Commits: for this particular the corresponding Endpoint is: GET /repos/{owner}/{repo}/commits
which full URL is [https://api.github.com/search/repositories](https://api.github.com/repos/{owner}/{repo}/commits)

- Parameters: for all the endpoints it is necessary to consider required and optional parameters:
{owner}: Repository owner (e.g., "Microsoft")
{repo}: Repository name (e.g., "vscode")
sha (optional): SHA or branch to start listing commits from
path (optional): Only commits containing this file path
author (optional): GitHub username or email address
since (optional): ISO 8601 date - only commits after this date
until (optional): ISO 8601 date - only commits before this date
per_page (optional): Results per page (1-100, default 30)
page (optional): Page number for pagination

### Contents: for this particular the corresponding Endpoint is: GET /repos/{owner}/{repo}/contents/{path}
which full URL is https://api.github.com/repos/{owner}/{repo}/contents/{path}

- Parameters: for all the endpoints it is necessary to consider required and optional parameters:
{owner}: Repository owner (e.g., "microsoft")
{repo}: Repository name (e.g., "vscode")
{path}: File or directory path (optional - omit for root directory)
ref (optional): Branch, tag, or commit SHA (default: main branch)

## Request/response formats
The requests are made trough each corresponding function in terms of client needs:

- search_repositories(query, sort="stars", order="desc", per_page=30)
Parameters:
  - query (str): The search keywords and filters (e.g. "tetris language:python"). (Uses GitHub search syntax)
  - sort (str, optional): Field to sort results by. Options include: "stars", "forks", "updated". (default: "stars").
  - order (str, optional): Sorting order. Can be "asc" (ascending) or "desc" (descending). (default: "desc").
  - per_page (int, optional): Number of results per page (max: 100). (default: 30).
Example:
  repo_1 = search_repositories("tetris language:assembly", per_page=5, sort = 'name', order="asc")

- get_repository_commits(owner, repo, per_page=30)
Parameters:
  - owner (str): The username or organization that owns the repository. (e.g. "kirjavascript")
  - repo (str): The name of the repository to fetch commits from. (e.g. "TetrisGYM")
  - per_page (int, optional): Number of commits to retrieve per page (max: 100). (default: 30).
Example:
  comm_1 = get_repository_commits("kirjavascript", 'TetrisGYM', per_page=1)

- get_repository_contents(owner, repo, path="")
Parameters:
  - owner (str):The username or organization that owns the repository. (e.g. "kirjavascript")
  - repo (str): The name of the repository. (e.g. "TetrisGYM")
  - path (str, optional): The file or directory path inside the repository to retrieve contents from. Use an empty string ("") to get the root directory. (default: ""). It is possible to use python selectors ([]) to select objects within the repo 
Example:
  repo_content1 = get_repository_contents("kirjavascript", 'TetrisGYM', path="README.md")

## Authentication method

GitHub applies different rate limits based on authentication. Unauthenticated requests are limited to 60 per hour, while authenticated requests with a personal access token allow up to 5.000 per hour. To optimize API usage and avoid rate limiting, it's recommended to generate a fine-grained, repository-scoped personal access token from GitHub by navigating to Settings > Developer settings > Personal access tokens.

A correct authentication, comes with the code 200, iclueding a response with information about the autorized account, as "login","id", or "avatar_url".

For other hand a bad authentication, comes with the code 401 {"message":"Bad credentials","documentation_url":"https://docs.github.com/rest","status":"401"},
It is reccomendable to check the variables BASE_URL nad TOKEN, previously created.

## Rate limits

The rate limits cna be cheked trough the functions:
- handle_rate_limit (response) the main function of theis cell is to get the time of wait in the case of the status code "403 Forbidden" or "429 Too Many Requests"
- Also trought a print("Remaining:", response.headers.get("X-RateLimit-Remaining")) it is possible to obteain the number of available reequests

## Pagination strategy
The strategy for pagination is established in the funcion 
- paginate_repositories(query, max_pages=3, per_page=30)
Parameters:
  - query (str): The search keywords and filters (e.g., "language:python stars:>500"). (Uses GitHub's search syntax)
  - max_pages (int, optional): The maximum number of pages to request. Each page contains up to per_page results. (default: 3)
  - per_page (int, optional): Number of repositories to retrieve per page (max: 100). (default: 30)
This funcuion bsesides loopp paginated is recommenden for bulk extraction and more advanced analytics in terms of fate



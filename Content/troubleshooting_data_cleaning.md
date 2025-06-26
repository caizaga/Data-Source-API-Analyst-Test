# Troubleshooting and data cleaning approach (Content/troubleshooting_data_cleaning.md)

## Troubleshooting

### 1.1. Rate Limit Errors (403 & 429)

### Erros 403 and 429: The errors "403 Forbidden" and "429 Too Many Requests", are commonly related to the number of request por hour, as mentioned aerlier, this rate depends or the aithentication. For this reason is recoomended to autienticate trough personal access token, to obtain more request per hour, in case of deplete the numer of request it is possible to obtain the ammounif waiting time to make more request.

For this particular task the variable "BASE_URL" should not be modified.

### Error 401: The error "401 Bad credentials", are related to a bad token, It is reccomendable to check the variables BASE_URL nad TOKEN, previously created.

### Data cleaning approach: into the api there are created several functions, to develop data cleaning processes, this funcions are created to the end of precessing, based in the library pandas in python.
Trough common  varibables as name, email, date or message, it is is possible to obtaint a tabular estucturte en form of DataFrame, obtenien valueale preprocessed data
to start analyzing 

The creatd functions are:

- clean_repos_data
Purpose:

Parameters: a previous created repo list created trough search_repositories
repos_list(required)


- clean_commits_data
Purpose:

Parameters: a previous created repo list created trough search_repositories
commits_list(required) a previous created commit list created trough get_repository_commits



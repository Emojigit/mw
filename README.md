`mwng` (MediaWiki API NG) is a MediaWiki API library for Python 3.

## MediaWiki API
We will use `mw` to call `mwng` in this document. `import mwng as mw`!
### `API = mw.API(api_php,[session])`
Create an `mw.API` instance.
* `api_php`: URL to the `api.php`, usually at `$SITEROOT/w/api.php`.
* `session`: a `requests.Session()`, either supplied or create a new one by the library.
### `API.login(username,botpassword)`
Login via botpassword.
* `username`: the username supplied by Special:Botpasswords
* `botpassword`: the passowrd supplied by Special:Botpasswords
As of MediaWiki 1.27, using the main account for login is not supported. Obtain credentials via Special:BotPasswords or use clientlogin method. We have no plan to introduce clientlogin, so you have to use a botpassword, or do it yourself via raw `API.get` and `API.post` requests.
### `API.edit(page,content,[token,timestamp])`
Edit a page, by its ID or title.
* `page`: either the page ID or the page title
* `content`: either a dictionary or a string, if it is a string, replace the entire page with `content`.
* `token`: (Optional) the `csrf` token, can be obtained via the first returned value of `API.csrf()`, or generated by the library itself.
* `timestamp`: (Optional) a valid timestamp for detecting edit confident, can be obtained via the second returned value of `API.csrf()`. Not detecting edit confident (`"now"`) by default.
### `API.token(type)`
Get a action token.
* `type`: The token type, can be one of these: `createaccount`, `csrf`, `deleteglobalaccount`, `login`, `patrol`, `rollback`, `setglobalaccountstatus`, `userrights`, `watch`
#### `API.logintoken()`
Get a login token (type: `login`).
#### `API.csrf()`
Get a CSRF token (type: `csrf`).
### `API.get(body)` and `API.post(body,[files])`
Do RAW API requests, by GET or POST.
* `body`: the request parameters
* `files`: (Optional, POST only) the file to upload, for `action = upload`
## Error handling
### `mw.MWAPIError`
The father of all MediaWiki Error Class. The following data are given:
* `dump`: the RAW JSON data, in case you have to do more things to the data
* `codes`: List of error codes
* `message`: The error description text (if there are only one), or human-readable list of error codes (if there are more than one)
### `mw.MWLoginError`
Error during login. There are no error codes (an empty list), only `dump` and `message` are given.


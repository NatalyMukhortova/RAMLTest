---
title: GitHub1 Notebook
site: https://api-notebook.anypoint.mulesoft.com/notebooks#dec93a16a8357cc4c37b
---

```javascript
load('https://github.com/chaijs/chai/releases/download/1.9.0/chai.js')
```

```javascript
assert = chai.assert
```

```javascript
// Read about the GitHub at http://api-portal.anypoint.mulesoft.com/onpositive/api/github
API.createClient('client', 'http://api-portal.anypoint.mulesoft.com/onpositive/api/github/GitHubV3.raml');
```

```javascript
API.authenticate(client,"oauth_2_0",{
  clientId: "cbb5738ff3afc8056855",
  clientSecret: "8615710097fbf13905620d110ab241719797570e",
  scopes: [ "user", "repo", "delete_repo", "admin:public_key" ]
})
```

Get the authenticated user

```javascript
userResponse = client.user.get()
```

```javascript
assert.equal( userResponse.status, 200, "Error" )
```

Update the authenticated user

```javascript
currentUserBlog = "My blog about RAML"
```

```javascript
currentUserCompany = "New Software Company"
```

```javascript
patchUserResponse = client.user.patch({
  "blog": currentUserBlog,
  "company": currentUserCompany,
  "location": "location",
  "hireable": false,
  "bio": "bio"
})
```

```javascript
assert.equal( patchUserResponse.status, 200, "Error" )
```

```javascript
currentUserBlog = "My blog"
```

```javascript
currentUserCompany = "Company"
```

```javascript
patchUserResponse = client.user.patch({
  "blog": currentUserBlog,
  "company": currentUserCompany,
  "location": "location",
  "hireable": false,
  "bio": "bio"
})
```

```javascript
assert.equal( patchUserResponse.status, 200, "Error" )
```

Follow a user.
Following a user requires the user to be logged in and authenticated with
basic auth or OAuth with the user:follow scope.

```javascript
userId = "KonstantinSviridov"
```

```javascript
putFollowingResponse = client.user.following.userId( userId ).put()
```

```javascript
assert.equal( putFollowingResponse.status, 204, "Error" )
```

Check if you are following a user

```javascript
followingUserResponse = client.user.following.userId( userId ).get()
```

```javascript
assert.equal( followingUserResponse.status, 204, "Error" )
```

List who the authenticated user is following

```javascript
followingResponse = client.user.following.get()
```

```javascript
assert.equal( followingResponse.status, 200, "Error" )
```

Unfollow a user.
Unfollowing a user requires the user to be logged in and authenticated with
basic auth or OAuth with the user:follow scope.

```javascript
deleteFollowingResponse = client.user.following.userId( userId ).delete()
```

```javascript
assert.equal( deleteFollowingResponse.status, 204, "Error" )
```

Create a new repository for the authenticated user. OAuth users must supply
repo scope.

```javascript
reposName = "New RAML API test repository"
```

```javascript
postReposResponse = client.user.repos.post({
  "name": reposName
})
```

```javascript
assert.equal( postReposResponse.status, 201, "Error" )
```

List repositories for the authenticated user. Note that this does not include
repositories owned by organizations which the user can access. You can lis
user organizations and list organization repositories separately.

```javascript
reposResponse = client.user.repos.get()
```

```javascript
assert.equal( reposResponse.status, 200, "Error" )
```

List public and private organizations for the authenticated user

```javascript
orgsResponse = client.user.orgs.get()
```

```javascript
assert.equal( orgsResponse.status, 200, "Error" )
```

List all public repositories.
This provides a dump of every public repository, in the order that they
were created.
Note: Pagination is powered exclusively by the since parameter. is the
Link header to get the URL for the next page of repositories.

```javascript
publicRepositoriesResponse = client.repositories.get()
```

```javascript
assert.equal( publicRepositoriesResponse.status, 200, "Error" )
```

Star a repository

```javascript
publicOwnerId = publicRepositoriesResponse.body[0].owner.login
```

```javascript
publicRepoId = publicRepositoriesResponse.body[0].name
```

```javascript
putStarRepoResponse = client.user.starred.ownerId( publicOwnerId ).repoId( publicRepoId ).put()
```

```javascript
assert.equal( putStarRepoResponse.status, 204, "Error" )
```

List repositories being starred by the authenticated user

```javascript
starredResponse = client.user.starred.get()
```

```javascript
assert.equal( starredResponse.status, 200, "Error" )
```

Check if you are starring a repository

```javascript
starredRepoResponse = client.user.starred.ownerId( publicOwnerId ).repoId( publicRepoId ).get()
```

```javascript
assert.equal( starredRepoResponse.status, 204, "Error" )
```

Unstar a repositor

```javascript
unstarResponse = client.user.starred.ownerId( publicOwnerId ).repoId( publicRepoId ).delete()
```

```javascript
assert.equal( unstarResponse.status, 204, "Error" )
```

Create a public key. Management of public keys via the API requires
that you are authenticated through basic auth, or OAuth with the 'user', 'write:public_key' scopes.

```javascript
postKeysResponse = client.user.keys.post({ "title": "My public key", "key": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAAgQCPJrIjihtAcCFwUSZIleLSiQeD+KzBTjmuITKJsyjXTG7fWd98Bol7t7BmrrliQMHFO5wlwyu6crP/1VA1OlAZb7gJnpDBRPXhryl/GkPceoyMcAk+0EXh5tZr7PBFyYOMHfUam2XEZA0M6AX58Fi9HsrvNddcwC2IccUHfMLnOQ== RSA-1024"
})
```

```javascript
assert.equal( postKeysResponse.status, 201, "Error" )
```

List your public keys.
Lists the current user's keys. 

```javascript
keysResponse = client.user.keys.get()
```

```javascript
assert.equal( keysResponse.status, 200, "Error" )
```

Get a single public key

```javascript
keyId = keysResponse.body[0].id
```

```javascript
keyResponse = client.user.keys.keyId( keyId ).get()
```

```javascript
assert.equal( keyResponse.status, 200, "Error" )
```

Delete a public key

```javascript
deleteKeyResponse = client.user.keys.keyId( keyId ).delete()
```

```javascript
assert.equal( deleteKeyResponse.status, 204, "Error" )
```

List email addresses for a user.
In the final version of the API, this method will return an array of hashes
with extended information for each email address indicating if the address
has been verified and if it's primary email address for GitHub.
Until API v3 is finalized, use the application/vnd.github.v3 media type to
get other response format.

```javascript
emailsResponse = client.user.emails.get()
```

```javascript
assert.equal( emailsResponse.status, 200, "Error" )
```

Add email address(es).
You can post a single email address or an array of addresses.

```javascript
emails = [ "nataly@mukhortova.ru" ]
```

```javascript
postEmailsResponse = client.user.emails.post( emails )
```

```javascript
assert.equal( postEmailsResponse.status, 201, "Error" )
```

Delete email address(es).
You can include a single email address or an array of addresses.

```javascript
emailsResponse = client.user.emails.delete( emails )
```

```javascript
assert.equal( emailsResponse.status, 204, "Error" )
```

List issues.
List all issues across owned and member repositories for the authenticated
user.

```javascript
issuesResponse = client.user.issues.get({
  "filter": "all",
  "state": "open",
  "labels": "labelsValue",
  "sort": "created",
  "direction": "desc"
})
```

```javascript
assert.equal( issuesResponse.status, 200, "Error" )
```

List repositories being watched by the authenticated user

```javascript
subscriptionsResponse = client.user.subscriptions.get()
```

```javascript
assert.equal( subscriptionsResponse.status, 200, "Error" )
```

Watch a repository

```javascript
watchResponse = client.user.subscriptions.ownerId( publicOwnerId ).repoId( publicRepoId ).put()
```

```javascript
assert.equal( watchResponse.status, 204, "Error" )
```

Check if you are watching a repository

```javascript
watchingResponse = client.user.subscriptions.ownerId( publicOwnerId ).repoId( publicRepoId ).get()
```

```javascript
assert.equal( watchingResponse.status, 204, "Error" )
```

Stop watching a repositor

```javascript
stopWatchingResponse = client.user.subscriptions.ownerId( publicOwnerId ).repoId( publicRepoId ).delete()
```

```javascript
assert.equal( stopWatchingResponse.status, 204, "Error" )
```

Delete the repository

```javascript
currentUser = userResponse.body.login
```

```javascript
repoId = postReposResponse.body.name
```

```javascript
deleteRepoResponse = client.repos.ownerId( currentUser ).repoId( repoId ).delete()
```

```javascript
assert.equal( deleteRepoResponse.status, 204, "Error" )
```

List all of the teams across all of the organizations to which the authenticated user belongs. This method requires user or repo scope when authenticating via OAuth

```javascript
teamsResponse = client.user.teams.get()
```

```javascript
assert.equal( teamsResponse.status, 200, "Error" )
```

List the authenticated user's follower

```javascript
followersResponse = client.user.followers.get()
```

```javascript
assert.equal( followersResponse.status, 200, "Error" )
```

# Web Content and Asset management

An Editor authors (creates, edits) a page with several assets being used on the page.

An asset is a file.

When an asset is used on a page, a "usage type" is associated with the usage. Some examples of usage types are:
* a download link
* a "open in new tab" link
* embedded read-only view (for images or documents)
* embedded thumbnail view (for images or documents)
* embedded folder view (with configurable properties: start folder, whether descend navigation is allowed, whether ascend navigation is allowed, view mode: gallery, list, table + list of columns)

A given asset may be used on one page multiple times.
A given asset may be used on many pages.
A page may not use any assets.
An asset may be not used on any pages.

embed views are provided by "embed applications"

Embed applications are pluggable, and each contributes a set of supported usages.

Physically assets may be stored in various kinds of repositories (DropBox, Google Drive, whatever).
A repository also contributes a set of natively supported usages, per asset type, e.g. repository A supports for images: embed and 'embed-thumbnail-800px'
while repository B supports for images: 'embed', 'embed-thumbnail-maxxy', 'embed-thumbnail-maxx', 'embed-thumbnail-maxy',
for *.docx documents: 'embed-view', 'embed-thumbnail-maxx'.

An embed application may support a usage in two ways:
* either relying on the repository's native support
* implement its own support (e.g. for embed-view usage for *.docx: transform docx to pdf + use pdf.js in browser)

A repository may or may not support permission assignment and checking.
If a repository does not support permission checking, it is accessed on behalf of a technical user (token).
If a repository supports permission checking, it may still be accessed on behalf of a technical user (token),
but it can also be accessed on behalf of the user (end user viewing the page, editor authoring the page).
This is achieved by the asset mgmt application being registered in the backing repository as OAuth client.

If an asset is used on a page via an 'embed' application, and application is configured to access the asset in the backing
repository with user's identity, and the access fails (no access token, expired access/refresh token), the
'embed' application renders "login" button instead of the actual "embedded view", asking the user to login.

The 'asset mgmt' application also provides a way to check that the page is not broken: everybody who can access a page,
can also access all assets on that page. This is achieved by accessing backing repositories in the background
with a technical user (or authenticating with "client credentials" OAuth2 grant type, if backing repository supports that).


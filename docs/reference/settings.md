# Settings

Wagtail makes use of the following settings, in addition to [Django's core settings](django:ref/settings)`:

## Site Name

### `WAGTAIL_SITE_NAME`

```python
WAGTAIL_SITE_NAME = 'Stark Industries Skunkworks'
```

This is the human-readable name of your Wagtail install which welcomes users upon login to the Wagtail admin.

(append_slash)=

## Append Slash

### `WAGTAIL_APPEND_SLASH`

```python
# Don't add a trailing slash to Wagtail-served URLs
WAGTAIL_APPEND_SLASH = False
```

Similar to Django's `APPEND_SLASH`, this setting controls how Wagtail will handle requests that don't end in a trailing slash.

When `WAGTAIL_APPEND_SLASH` is `True` (default), requests to Wagtail pages which omit a trailing slash will be redirected by Django's {class}`~django.middleware.common.CommonMiddleware` to a URL with a trailing slash.

When `WAGTAIL_APPEND_SLASH` is `False`, requests to Wagtail pages will be served both with and without trailing slashes. Page links generated by Wagtail, however, will not include trailing slashes.

```{note}
If you use the ``False`` setting, keep in mind that serving your pages both with and without slashes may affect search engines' ability to index your site. See [this Google Search Central Blog post](https://developers.google.com/search/blog/2010/04/to-slash-or-not-to-slash) for more details.
```

## ADMIN BASE URL

(wagtailadmin_base_url)=

### `WAGTAILADMIN_BASE_URL`

```python
WAGTAILADMIN_BASE_URL = 'http://example.com'
```

This is the base URL used by the Wagtail admin site. It is typically used for generating URLs to include in notification emails.

If this setting is not present, Wagtail will try to fall back to `request.site.root_url` or to the request's host name.

```{versionchanged} 3.0
This setting was previously named ``BASE_URL`` and was undocumented, using ``BASE_URL`` will be removed in a future release.
```

## Search

### `WAGTAILSEARCH_BACKENDS`

```python
WAGTAILSEARCH_BACKENDS = {
    'default': {
        'BACKEND': 'wagtail.search.backends.elasticsearch5',
        'INDEX': 'myapp'
    }
}
```

Define a search backend. For a full explanation, see [](wagtailsearch_backends).

(wagtailsearch_hits_max_age)=

### `WAGTAILSEARCH_HITS_MAX_AGE`

```python
WAGTAILSEARCH_HITS_MAX_AGE = 14
```

Set the number of days (default 7) that search query logs are kept for; these are used to identify popular search terms for [promoted search results](editors_picks). Queries older than this will be removed by the [](search_garbage_collect) command.

## Internationalisation

Wagtail supports internationalisation of content by maintaining separate trees of pages for each language.

For a guide on how to enable internationalisation on your site, see the [configuration guide](enabling_internationalisation).

### `WAGTAIL_I18N_ENABLED`

(boolean, default `False`)

When set to `True`, Wagtail's internationalisation features will be enabled:

```python
WAGTAIL_I18N_ENABLED = True
```

(wagtail_content_languages_setting)=

### `WAGTAIL_CONTENT_LANGUAGES`

(list, default `[]`)

A list of languages and/or locales that Wagtail content can be authored in.

For example:

```python
WAGTAIL_CONTENT_LANGUAGES = [
    ('en', _("English")),
    ('fr', _("French")),
]
```

Each item in the list is a 2-tuple containing a language code and a display name.
The language code can either be a language code on its own (such as `en`, `fr`), or it can include a region code (such as `en-gb`, `fr-fr`).
You can mix the two formats if you only need to localize in some regions but not others.

This setting follows the same structure of Django's `LANGUAGES` setting, so they can both be set to the same value:

```python
LANGUAGES = WAGTAIL_CONTENT_LANGUAGES = [
    ('en-gb', _("English (United Kingdom)")),
    ('en-us', _("English (United States)")),
    ('es-es', _("Spanish (Spain)")),
    ('es-mx', _("Spanish (Mexico)")),
]
```

However having them separate allows you to configure many different regions on your site yet have them share Wagtail content (but defer on things like date formatting, currency, etc):

```python
LANGUAGES = [
    ('en', _("English (United Kingdom)")),
    ('en-us', _("English (United States)")),
    ('es', _("Spanish (Spain)")),
    ('es-mx', _("Spanish (Mexico)")),
]


WAGTAIL_CONTENT_LANGUAGES = [
    ('en', _("English")),
    ('es', _("Spanish")),
]
```

This would mean that your site will respond on the `https://www.mysite.com/es/` and `https://www.mysite.com/es-MX/` URLs, but both of them will serve content from the same "Spanish" tree in Wagtail.

```{note}
``WAGTAIL_CONTENT_LANGUAGES`` must be a subset of ``LANGUAGES``

Note that all languages that exist in ``WAGTAIL_CONTENT_LANGUAGES`` must also exist in your ``LANGUAGES`` setting. This is so that Wagtail can generate a live URL to these pages from an untranslated context (such as the admin interface).
```

## Embeds

Wagtail supports generating embed code from URLs to content on an external providers such as Youtube or Twitter. By default, Wagtail will fetch the embed code directly from the relevant provider's site using the oEmbed protocol.
Wagtail has a builtin list of the most common providers.

The embeds fetching can be fully configured using the `WAGTAILEMBEDS_FINDERS` setting. This is fully documented in [](configuring_embed_finders).

### `WAGTAILEMBEDS_RESPONSIVE_HTML`

```python
WAGTAILEMBEDS_RESPONSIVE_HTML = True
```

Adds `class="responsive-object"` and an inline `padding-bottom` style to embeds, to assist in making them responsive. See [](responsive_embeds) for details.

## Dashboard

### `WAGTAILADMIN_RECENT_EDITS_LIMIT`

```python
WAGTAILADMIN_RECENT_EDITS_LIMIT = 5
```

This setting lets you change the number of items shown at 'Your most recent edits' on the dashboard.

## User

(wagtail_gravatar_provider_url)=

### `WAGTAIL_GRAVATAR_PROVIDER_URL`

```python
WAGTAIL_GRAVATAR_PROVIDER_URL = '//www.gravatar.com/avatar'
```

If a user has not uploaded a profile picture, Wagtail will look for an avatar linked to their email address on gravatar.com. This setting allows you to specify an alternative provider such as like robohash.org, or can be set to `None` to disable the use of remote avatars completely.

## Comments

### `WAGTAILADMIN_COMMENTS_ENABLED`

```python
# Disable commenting
WAGTAILADMIN_COMMENTS_ENABLED = False
```

Sets whether commenting is enabled for pages (`True` by default).

## Images

### `WAGTAILIMAGES_IMAGE_MODEL`

```python
WAGTAILIMAGES_IMAGE_MODEL = 'myapp.MyImage'
```

This setting lets you provide your own image model for use in Wagtail, which should extend the built-in `AbstractImage` class.

### `WAGTAILIMAGES_IMAGE_FORM_BASE`

```python
WAGTAILIMAGES_IMAGE_FORM_BASE = 'myapp.forms.MyImageBaseForm'
```

This setting lets you provide your own image base form for use in Wagtail, which should extend the built-in `BaseImageForm` class.
You can use it to specify or override the widgets to use in the admin form.

### `WAGTAILIMAGES_MAX_UPLOAD_SIZE`

```python
WAGTAILIMAGES_MAX_UPLOAD_SIZE = 20 * 1024 * 1024  # 20MB
```

This setting lets you override the maximum upload size for images (in bytes). If omitted, Wagtail will fall back to using its 10MB default value.

### `WAGTAILIMAGES_MAX_IMAGE_PIXELS`

```python
WAGTAILIMAGES_MAX_IMAGE_PIXELS = 128000000  # 128 megapixels
```

This setting lets you override the maximum number of pixels an image can have. If omitted, Wagtail will fall back to using its 128 megapixels default value. The pixel count takes animation frames into account - for example, a 25-frame animation of size 100x100 is considered to have 100 _ 100 _ 25 = 250000 pixels.

### `WAGTAILIMAGES_FEATURE_DETECTION_ENABLED`

```python
WAGTAILIMAGES_FEATURE_DETECTION_ENABLED = True
```

This setting enables feature detection once OpenCV is installed, see all details on the [](image_feature_detection) documentation.

### `WAGTAILIMAGES_INDEX_PAGE_SIZE`

```python
WAGTAILIMAGES_INDEX_PAGE_SIZE = 20
```

Specifies the number of images per page shown on the main Images listing in the Wagtail admin.

### `WAGTAILIMAGES_USAGE_PAGE_SIZE`

```python
WAGTAILIMAGES_USAGE_PAGE_SIZE = 20
```

Specifies the number of items per page shown when viewing an image's usage (see [`WAGTAIL_USAGE_COUNT_ENABLED`](WAGTAIL_USAGE_COUNT_ENABLED)).

### `WAGTAILIMAGES_CHOOSER_PAGE_SIZE`

```python
WAGTAILIMAGES_CHOOSER_PAGE_SIZE = 12
```

Specifies the number of images shown per page in the image chooser modal.

(wagtailimages_rendition_storage)=

### `WAGTAILIMAGES_RENDITION_STORAGE`

```python
WAGTAILIMAGES_RENDITION_STORAGE = 'myapp.backends.MyCustomStorage'
```

This setting allows image renditions to be stored using an alternative storage backend. The default is `None`, which will use Django's default `FileSystemStorage`.

Custom storage classes should subclass `django.core.files.storage.Storage`. See the {doc}`Django file storage API <django:ref/files/storage>`.

## Documents

### `WAGTAILDOCS_DOCUMENT_MODEL`

```python
WAGTAILDOCS_DOCUMENT_MODEL = 'myapp.MyDocument'
```

This setting lets you provide your own document model for use in Wagtail, which should extend the built-in `AbstractDocument` class.

### `WAGTAILDOCS_DOCUMENT_FORM_BASE`

```python
WAGTAILDOCS_DOCUMENT_FORM_BASE = 'myapp.forms.MyDocumentBaseForm'
```

This setting lets you provide your own Document base form for use in Wagtail, which should extend the built-in `BaseDocumentForm` class.
You can use it to specify or override the widgets to use in the admin form.

(wagtaildocs_serve_method)=

### `WAGTAILDOCS_SERVE_METHOD`

```python
WAGTAILDOCS_SERVE_METHOD = 'redirect'
```

Determines how document downloads will be linked to and served. Normally, requests for documents are sent through a Django view, to perform privacy checks (see [](collection_privacy_settings)) and potentially other housekeeping tasks such as hit counting. To fully protect against users bypassing this check, it needs to happen in the same request where the document is served; however, this incurs a performance hit as the document then needs to be served by the Django server. In particular, this cancels out much of the benefit of hosting documents on external storage, such as S3 or a CDN.

For this reason, Wagtail provides a number of serving methods which trade some of the strictness of the permission check for performance:

-   `'direct'` - links to documents point directly to the URL provided by the underlying storage, bypassing the Django view that provides the permission check. This is most useful when deploying sites as fully static HTML (for example using [wagtail-bakery](https://github.com/wagtail/wagtail-bakery) or [Gatsby](https://www.gatsbyjs.org/)).
-   `'redirect'` - links to documents point to a Django view which will check the user's permission; if successful, it will redirect to the URL provided by the underlying storage to allow the document to be downloaded. This is most suitable for remote storage backends such as S3, as it allows the document to be served independently of the Django server. Note that if a user is able to guess the latter URL, they will be able to bypass the permission check; some storage backends may provide configuration options to generate a random or short-lived URL to mitigate this.
-   `'serve_view'` - links to documents point to a Django view which both checks the user's permission, and serves the document. Serving will be handled by [django-sendfile](https://github.com/johnsensible/django-sendfile), if this is installed and supported by your server configuration, or as a streaming response from Django if not. When using this method, it is recommended that you configure your webserver to _disallow_ serving documents directly from their location under `MEDIA_ROOT`, as this would provide a way to bypass the permission check.

If `WAGTAILDOCS_SERVE_METHOD` is unspecified or set to `None`, the default method is `'redirect'` when a remote storage backend is in use (one that exposes a URL but not a local filesystem path), and `'serve_view'` otherwise. Finally, some storage backends may not expose a URL at all; in this case, serving will proceed as for `'serve_view'`.

(wagtaildocs_content_types)=

### `WAGTAILDOCS_CONTENT_TYPES`

```python
WAGTAILDOCS_CONTENT_TYPES = {
    'pdf': 'application/pdf',
    'txt': 'text/plain',
}
```

Specifies the MIME content type that will be returned for the given file extension, when using the `serve_view` method. Content types not listed here will be guessed using the Python `mimetypes.guess_type` function, or `application/octet-stream` if unsuccessful.

(wagtaildocs_inline_content_types)=

### `WAGTAILDOCS_INLINE_CONTENT_TYPES`

```python
WAGTAILDOCS_INLINE_CONTENT_TYPES = ['application/pdf', 'text/plain']
```

A list of MIME content types that will be shown inline in the browser (by serving the HTTP header `Content-Disposition: inline`) rather than served as a download, when using the `serve_view` method. Defaults to `application/pdf`.

(wagtaildocs_extensions)=

### `WAGTAILDOCS_EXTENSIONS`

```python
WAGTAILDOCS_EXTENSIONS = ['pdf', 'docx']
```

A list of allowed document extensions that will be validated during document uploading.
If this isn't supplied all document extensions are allowed.
Warning: this doesn't always ensure that the uploaded file is valid as files can
be renamed to have an extension no matter what data they contain.

## Password Management

### `WAGTAIL_PASSWORD_MANAGEMENT_ENABLED`

```python
WAGTAIL_PASSWORD_MANAGEMENT_ENABLED = True
```

This specifies whether users are allowed to change their passwords (enabled by default).

### `WAGTAIL_PASSWORD_RESET_ENABLED`

```python
WAGTAIL_PASSWORD_RESET_ENABLED = True
```

This specifies whether users are allowed to reset their passwords. Defaults to the same as `WAGTAIL_PASSWORD_MANAGEMENT_ENABLED`. Password reset emails will be sent from the address specified in Django's `DEFAULT_FROM_EMAIL` setting.

### `WAGTAILUSERS_PASSWORD_ENABLED`

```python
WAGTAILUSERS_PASSWORD_ENABLED = True
```

This specifies whether password fields are shown when creating or editing users through Settings -> Users (enabled by default). Set this to False (along with `WAGTAIL_PASSWORD_MANAGEMENT_ENABLED` and `WAGTAIL_PASSWORD_RESET_ENABLED`) if your users are authenticated through an external system such as LDAP.

### `WAGTAILUSERS_PASSWORD_REQUIRED`

```python
WAGTAILUSERS_PASSWORD_REQUIRED = True
```

This specifies whether password is a required field when creating a new user. True by default; ignored if `WAGTAILUSERS_PASSWORD_ENABLED` is false. If this is set to False, and the password field is left blank when creating a user, then that user will have no usable password; in order to log in, they will have to reset their password (if `WAGTAIL_PASSWORD_RESET_ENABLED` is True) or use an alternative authentication system such as LDAP (if one is set up).

### `WAGTAIL_EMAIL_MANAGEMENT_ENABLED`

```python
WAGTAIL_EMAIL_MANAGEMENT_ENABLED = True
```

This specifies whether users are allowed to change their email (enabled by default).

(email_notifications)=

### `WAGTAILADMIN_USER_PASSWORD_RESET_FORM`

```python
WAGTAILADMIN_USER_PASSWORD_RESET_FORM = 'users.forms.PasswordResetForm'
```

Allows the default `PasswordResetForm` to be extended with extra fields.

## Email Notifications

### `WAGTAILADMIN_NOTIFICATION_FROM_EMAIL`

```python
WAGTAILADMIN_NOTIFICATION_FROM_EMAIL = 'wagtail@myhost.io'
```

Wagtail sends email notifications when content is submitted for moderation, and when the content is accepted or rejected. This setting lets you pick which email address these automatic notifications will come from. If omitted, Wagtail will fall back to using Django's `DEFAULT_FROM_EMAIL` setting.

### `WAGTAILADMIN_NOTIFICATION_USE_HTML`

```python
WAGTAILADMIN_NOTIFICATION_USE_HTML = True
```

Notification emails are sent in `text/plain` by default, change this to use HTML formatting.

### `WAGTAILADMIN_NOTIFICATION_INCLUDE_SUPERUSERS`

```python
WAGTAILADMIN_NOTIFICATION_INCLUDE_SUPERUSERS = False
```

Notification emails are sent to moderators and superusers by default. You can change this to exclude superusers and only notify moderators.

(update_notifications)=

## Wagtail update notifications

### `WAGTAIL_ENABLE_UPDATE_CHECK`

```python
WAGTAIL_ENABLE_UPDATE_CHECK = True
```

For admins only, Wagtail performs a check on the dashboard to see if newer releases are available. This also provides the Wagtail team with the hostname of your Wagtail site. If you'd rather not receive update notifications, or if you'd like your site to remain unknown, you can disable it with this setting.

If admins should only be informed of new long term support (LTS) versions, then set this setting to `"lts"` (the setting is case-insensitive).

## Private pages / documents

### `PASSWORD_REQUIRED_TEMPLATE`

```python
PASSWORD_REQUIRED_TEMPLATE = 'myapp/password_required.html'
```

This is the path to the Django template which will be used to display the "password required" form when a user accesses a private page. For more details, see the [](private_pages) documentation.

### `DOCUMENT_PASSWORD_REQUIRED_TEMPLATE`

```python
DOCUMENT_PASSWORD_REQUIRED_TEMPLATE = 'myapp/document_password_required.html'
```

As above, but for password restrictions on documents. For more details, see the [](private_pages) documentation.

## Login page

### `WAGTAILADMIN_USER_LOGIN_FORM`

```python
WAGTAILADMIN_USER_LOGIN_FORM = 'users.forms.LoginForm'
```

Allows the default `LoginForm` to be extended with extra fields.

### `WAGTAIL_FRONTEND_LOGIN_TEMPLATE`

The basic login page can be customised with a custom template.

```python
WAGTAIL_FRONTEND_LOGIN_TEMPLATE = 'myapp/login.html'
```

### `WAGTAIL_FRONTEND_LOGIN_URL`

Or the login page can be a redirect to an external or internal URL.

```python
WAGTAIL_FRONTEND_LOGIN_URL = '/accounts/login/'
```

For more details, see the [](login_page) documentation.

## Case-Insensitive Tags

### `TAGGIT_CASE_INSENSITIVE`

```python
TAGGIT_CASE_INSENSITIVE = True
```

Tags are case-sensitive by default ('music' and 'Music' are treated as distinct tags). In many cases the reverse behaviour is preferable.

## Multi-word tags

### `TAG_SPACES_ALLOWED`

```python
TAG_SPACES_ALLOWED = False
```

Tags can only consist of a single word, no spaces allowed. The default setting is `True` (spaces in tags are allowed).

## Tag limit

### `TAG_LIMIT`

```python
TAG_LIMIT = 5
```

Limit the number of tags that can be added to (django-taggit) Tag model. Default setting is `None`, meaning no limit on tags.

## Unicode Page Slugs

### `WAGTAIL_ALLOW_UNICODE_SLUGS`

```python
WAGTAIL_ALLOW_UNICODE_SLUGS = True
```

By default, page slugs can contain any alphanumeric characters, including non-Latin alphabets. Set this to False to limit slugs to ASCII characters.

## Auto update preview

### `WAGTAIL_AUTO_UPDATE_PREVIEW`

```python
WAGTAIL_AUTO_UPDATE_PREVIEW = True
```

When enabled, the preview panel in the page editor is automatically updated on each change. If set to `False`, a refresh button will be shown and the preview is only updated when the button is clicked.
This behaviour is enabled by default.

### `WAGTAIL_AUTO_UPDATE_PREVIEW_INTERVAL`

```python
WAGTAIL_AUTO_UPDATE_PREVIEW_INTERVAL = 500
```

The interval (in milliseconds) to check for changes made in the page editor before updating the preview. The default value is `500`.

## Custom User Edit Forms

See {doc}`/advanced_topics/customisation/custom_user_models`.

### `WAGTAIL_USER_EDIT_FORM`

```python
WAGTAIL_USER_EDIT_FORM = 'users.forms.CustomUserEditForm'
```

Allows the default `UserEditForm` class to be overridden with a custom form when a custom user model is being used and extra fields are required in the user edit form.

### `WAGTAIL_USER_CREATION_FORM`

```python
WAGTAIL_USER_CREATION_FORM = 'users.forms.CustomUserCreationForm'
```

Allows the default `UserCreationForm` class to be overridden with a custom form when a custom user model is being used and extra fields are required in the user creation form.

### `WAGTAIL_USER_CUSTOM_FIELDS`

```python
WAGTAIL_USER_CUSTOM_FIELDS = ['country']
```

A list of the extra custom fields to be appended to the default list.

(WAGTAIL_USAGE_COUNT_ENABLED)=

## Usage for images, documents and snippets

### `WAGTAIL_USAGE_COUNT_ENABLED`

```python
WAGTAIL_USAGE_COUNT_ENABLED = True
```

When enabled Wagtail shows where a particular image, document or snippet is being used on your site.
This is disabled by default because it generates a query which may run slowly on sites with large numbers of pages.

A link will appear on the edit page (in the rightmost column) showing you how many times the item is used.
Clicking this link takes you to the "Usage" page, which shows you where the snippet, document or image is used.

The link is also shown on the delete page, above the "Delete" button.

```{note}
The usage count only applies to direct (database) references. Using documents, images and snippets within StreamFields or rich text fields will not be taken into account.
```

## Date and DateTime inputs

### `WAGTAIL_DATE_FORMAT`, `WAGTAIL_DATETIME_FORMAT`, `WAGTAIL_TIME_FORMAT`

```python
WAGTAIL_DATE_FORMAT = '%d.%m.%Y.'
WAGTAIL_DATETIME_FORMAT = '%d.%m.%Y. %H:%M'
WAGTAIL_TIME_FORMAT = '%H:%M'
```

Specifies the date, time and datetime format to be used in input fields in the Wagtail admin. The format is specified in [Python datetime module syntax](https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior), and must be one of the recognised formats listed in the `DATE_INPUT_FORMATS`, `TIME_INPUT_FORMATS`, or `DATETIME_INPUT_FORMATS` setting respectively (see [DATE_INPUT_FORMATS](https://docs.djangoproject.com/en/stable/ref/settings/#std:setting-DATE_INPUT_FORMATS)).

(WAGTAIL_USER_TIME_ZONES)=

## Time zones

Logged-in users can choose their current time zone for the admin interface in the account settings. If is no time zone selected by the user, then `TIME_ZONE` will be used.
(Note that time zones are only applied to datetime fields, not to plain time or date fields. This is a Django design decision.)

### `WAGTAIL_USER_TIME_ZONES`

The list of time zones is by default the common_timezones list from pytz.
It is possible to override this list via the `WAGTAIL_USER_TIME_ZONES` setting.
If there is zero or one time zone permitted, the account settings form will be hidden.

```python
WAGTAIL_USER_TIME_ZONES = ['America/Chicago', 'Australia/Sydney', 'Europe/Rome']
```

(WAGTAILADMIN_PERMITTED_LANGUAGES)=

## Admin languages

Users can choose between several languages for the admin interface in the account settings. The list of languages is by default all the available languages in Wagtail with at least 90% coverage. To change it, set `WAGTAILADMIN_PERMITTED_LANGUAGES`:

### `WAGTAILADMIN_PERMITTED_LANGUAGES`

```python
WAGTAILADMIN_PERMITTED_LANGUAGES = [('en', 'English'),
                                    ('pt', 'Portuguese')]
```

Since the syntax is the same as Django `LANGUAGES`, you can do this so users can only choose between front office languages:

```python
LANGUAGES = WAGTAILADMIN_PERMITTED_LANGUAGES = [('en', 'English'),
                                                ('pt', 'Portuguese')]
```

## Static files

### `WAGTAILADMIN_STATIC_FILE_VERSION_STRINGS`

```python
WAGTAILADMIN_STATIC_FILE_VERSION_STRINGS = False
```

Static file URLs within the Wagtail admin are given a version-specific query string of the form `?v=1a2b3c4d`, to prevent outdated cached copies of JavaScript and CSS files from persisting after a Wagtail upgrade. To disable these, set `WAGTAILADMIN_STATIC_FILE_VERSION_STRINGS` to `False`.

## API Settings

For full documentation on API configuration, including these settings, see [](api_v2_configuration) documentation.

### `WAGTAILAPI_BASE_URL`

```python
WAGTAILAPI_BASE_URL = 'http://api.example.com/'
```

Required when using frontend cache invalidation, used to generate absolute URLs to document files and invalidating the cache.

### `WAGTAILAPI_LIMIT_MAX`

```python
WAGTAILAPI_LIMIT_MAX = 500
```

Default is 20, used to change the maximum number of results a user can request at a time, set to `None` for no limit.

### `WAGTAILAPI_SEARCH_ENABLED`

```python
WAGTAILAPI_SEARCH_ENABLED = False
```

Default is true, setting this to false will disable full text search on all endpoints.

### `WAGTAILAPI_USE_FRONTENDCACHE`

```python
WAGTAILAPI_USE_FRONTENDCACHE = True
```

Requires `wagtailfrontendcache` app to be installed, indicates the API should use the frontend cache.

## Frontend cache

For full documentation on frontend cache invalidation, including these settings, see [](frontend_cache_purging).

### `WAGTAILFRONTENDCACHE`

```python
  WAGTAILFRONTENDCACHE = {
      'varnish': {
          'BACKEND': 'wagtail.contrib.frontend_cache.backends.HTTPBackend',
          'LOCATION': 'http://localhost:8000',
      },
  }
```

See documentation linked above for full options available.

```{note}
``WAGTAILFRONTENDCACHE_LOCATION`` is no longer the preferred way to set the cache location, instead set the ``LOCATION`` within the ``WAGTAILFRONTENDCACHE`` item.
```

### `WAGTAILFRONTENDCACHE_LANGUAGES`

```python
WAGTAILFRONTENDCACHE_LANGUAGES = [l[0] for l in settings.LANGUAGES]
```

Default is an empty list, must be a list of languages to also purge the urls for each language of a purging url. This setting needs `settings.USE_I18N` to be `True` to work.

(WAGTAILADMIN_RICH_TEXT_EDITORS)=

## Rich text

### `WAGTAILADMIN_RICH_TEXT_EDITORS`

```python
WAGTAILADMIN_RICH_TEXT_EDITORS = {
    'default': {
        'WIDGET': 'wagtail.admin.rich_text.DraftailRichTextArea',
        'OPTIONS': {
            'features': ['h2', 'bold', 'italic', 'link', 'document-link']
        }
    },
    'secondary': {
        'WIDGET': 'some.external.RichTextEditor',
    }
}
```

Customise the behaviour of rich text fields. By default, `RichTextField` and `RichTextBlock` use the configuration given under the `'default'` key, but this can be overridden on a per-field basis through the `editor` keyword argument, for example `body = RichTextField(editor='secondary')`. Within each configuration block, the following fields are recognised:

-   `WIDGET`: The rich text widget implementation to use. Wagtail provides `wagtail.admin.rich_text.DraftailRichTextArea` (a modern extensible editor which enforces well-structured markup). Other widgets may be provided by third-party packages.
-   `OPTIONS`: Configuration options to pass to the widget. Recognised options are widget-specific, but `DraftailRichTextArea` accept a `features` list indicating the active rich text features (see [](rich_text_features)).

If a `'default'` editor is not specified, rich text fields that do not specify an `editor` argument will use the Draftail editor with the default feature set enabled.

(WAGTAILADMIN_EXTERNAL_LINK_CONVERSION)=

### `WAGTAILADMIN_EXTERNAL_LINK_CONVERSION`

```python
WAGTAILADMIN_EXTERNAL_LINK_CONVERSION = 'exact'
```

Customise Wagtail's behaviour when an internal page url is entered in the external link chooser. Possible values for this setting are `'all'`, `'exact'`, `'confirm`, or `''`. The default, `'all'`, means that Wagtail will automatically convert submitted urls that exactly match page urls to the corresponding internal links. If the url is an inexact match - for example, the submitted url has query parameters - then Wagtail will confirm the conversion with the user. `'exact'` means that any inexact matches will be left as external urls, and the confirmation step will be skipped. `'confirm'` means that every link conversion will be confirmed with the user, even if the match is exact. `''` means that Wagtail will not attempt to convert any urls entered to internal page links.

(WAGTAILADMIN_GLOBAL_PAGE_EDIT_LOCK)=

## Page locking

### `WAGTAILADMIN_GLOBAL_PAGE_EDIT_LOCK`

`WAGTAILADMIN_GLOBAL_PAGE_EDIT_LOCK` can be set to `True` to prevent users from editing pages that they have locked.

## Redirects

### `WAGTAIL_REDIRECTS_FILE_STORAGE`

```python
WAGTAIL_REDIRECTS_FILE_STORAGE = 'tmp_file'
```

By default the redirect importer keeps track of the uploaded file as a temp file, but on certain environments (load balanced/cloud environments), you cannot keep a shared file between environments. For those cases you can use the built-in cache to store the file instead.

```python
WAGTAIL_REDIRECTS_FILE_STORAGE = 'cache'
```

## Form builder

### `WAGTAILFORMS_HELP_TEXT_ALLOW_HTML`

```python
WAGTAILFORMS_HELP_TEXT_ALLOW_HTML = True
```

When true, HTML tags in form field help text will be rendered unescaped (default: False).

```{warning}
Enabling this option will allow editors to insert arbitrary HTML into the page, such as scripts that could allow the editor to acquire administrator privileges when another administrator views the page. Do not enable this setting unless your editors are fully trusted.
```

(workflow_settings)=

## Workflow

(wagtail_moderation_enabled)=

### `WAGTAIL_MODERATION_ENABLED`

```python
WAGTAIL_MODERATION_ENABLED = True
```

Changes whether the Submit for Moderation button is displayed in the action menu.

### `WAGTAIL_WORKFLOW_ENABLED`

```python
WAGTAIL_WORKFLOW_ENABLED = False
```

Specifies whether moderation workflows are enabled (default: True). When disabled, editors will no longer be given the option to submit pages to a workflow, and the settings areas for admins to configure workflows and tasks will be unavailable.

### `WAGTAIL_WORKFLOW_REQUIRE_REAPPROVAL_ON_EDIT`

```python
WAGTAIL_WORKFLOW_REQUIRE_REAPPROVAL_ON_EDIT = True
```

Moderation workflows can be used in two modes. The first is to require that all tasks must approve a specific page revision for the workflow to complete. As a result, if edits are made to a page while it is in moderation, any approved tasks will need to be re-approved for the new revision before the workflow finishes. This is the default, `WAGTAIL_WORKFLOW_REQUIRE_REAPPROVAL_ON_EDIT = True` . The second mode does not require reapproval: if edits are made when tasks have already been approved, those tasks do not need to be reapproved. This is more suited to a hierarchical workflow system. To use workflows in this mode, set `WAGTAIL_WORKFLOW_REQUIRE_REAPPROVAL_ON_EDIT = False`.

### `WAGTAIL_FINISH_WORKFLOW_ACTION`

```python
WAGTAIL_FINISH_WORKFLOW_ACTION = 'wagtail.workflows.publish_workflow_state'
```

This sets the function to be called when a workflow completes successfully - by default, `wagtail.workflows.publish_workflow_state`,which publishes the page. The function must accept a `WorkflowState` object as its only positional argument.

### `WAGTAIL_WORKFLOW_CANCEL_ON_PUBLISH`

```python
WAGTAIL_WORKFLOW_CANCEL_ON_PUBLISH = True
```

This determines whether publishing a page with an ongoing workflow will cancel the workflow (if true) or leave the workflow unaffected (false).
Disabling this could be useful if your site has long, multi-step workflows, and you want to be able to publish urgent page updates while the workflow continues to provide less urgent feedback.

## Page deletion

### `WAGTAILADMIN_UNSAFE_PAGE_DELETION_LIMIT`

```python
WAGTAILADMIN_UNSAFE_PAGE_DELETION_LIMIT = 20
```

This setting enables an additional confirmation step when deleting a page with a large number of child pages. If the number of pages is greater than or equal to this limit (10 by default), the user must enter the site name (as defined by `WAGTAIL_SITE_NAME`) to proceed.
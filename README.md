# WCDP Mobilize API Feed Generator

A magnificent repository using the [Mobilize API](https://github.com/mobilizeamerica/api) to pull events & interest forms from a given org_id's dashboard and convert them into html fragments suitable for embedding into Wordpress via shortcodes.

## Running

Requirements include the ability to run bash scripts, curl, and an installation of [jq](https://jqlang.github.io/jq/download/) for json parsing.

To run manually/locally, the main script is `doit` and when invoked it will call 

* `getit` to fetch & format the json content
* `fragit` to convert the json to HTML fragments via jinja templates
* `mobit` to SFTP the fragments to the WP installation

in succession.  Then the script will purge the page_caches in WP for the locations where the fragments are embedded via WP shortcode, ensuring the new snippets show up to site viewers.

## Scheduling

Set up a cron job to suit your needs (perhaps once every 15 minutes) and let it rip.  If your cron job runs on the target WP host you don't need the mobit script, rather you only need to move the fragments into place for WP to find them.

## Beware!

As always, tread with care when collecting info via a URL and displaying it on your site.  These scripts could and should be hardened - pull requests welcome!

## Notes

There is no API key or other authentication needed for the Mobilize API at this time.

Constants at the top of `getit` are the main way to customize the API query by org_id, zip code, radius, and event types.

Two queries are made, with the second one fetching only interest forms, which are not returned by default with other event types.  The two resulting lists will be combined into a single json object.

If you don't need interest forms (e.g. volunteer signups) you can comment out that call.

The SFTP to WP step assumes you have a named ssh config for the host in question and have public keys and an agent set up for auth.



*Fin*





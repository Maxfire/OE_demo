# Open Europa Library
This is a kickstarter project for Open Europa Library components integration based on Open Europa [subsite](https://github.com/ec-europa/subsite) by using Open Europa [toolkit](https://github.com/ec-europa/toolkit).

### 1. Drupal instalation
[oe_demo](https://github.com/Maxfire/OE_demo) repository.
```
$ git clone https://github.com/Maxfire/OE_demo.git oe_demo
$ cd oe_demo
$ git checkout release/oel
$ docker-compose up -d
$ docker-compose exec web composer install
$ docker-compose exec web ./vendor/bin/run toolkit:build-dev
$ docker-compose exec web ./vendor/bin/run toolkit:install-clean
```

### 2. Whitelabel theme
[oe_whitelabel](https://github.com/openeuropa/oe_whitelabel) repository.
```
$ docker-compose exec web composer require "openeuropa/oe_whitelabel:^1.0.0-alpha7"
$ docker-compose exec web composer require openeuropa/oe_corporate_blocks
$ docker-compose exec web drush en oe_whitelabel_helper -y && docker-compose exec web drush en twig_field_value -y
$ docker-compose exec web ./vendor/bin/drush uli --uri=http://localhost:8080/web/
```
Install and set as default the whitelabel theme though UI.
```
$ docker-compose exec web drush cr
```

### 3. Search
[search_api](https://www.drupal.org/project/search_api) module.
```
$ docker-compose exec web composer require drupal/search_api && docker-compose exec web composer require drupal/search_api_autocomplete
$ docker-compose exec web drush en oe_whitelabel_search && docker-compose exec web drush en search_api_db -y && docker-compose exec web drush en search_api_autocomplete
```
Go to search api and create server + index.
* Server:  match on part of a word.

Go to search api fields:
* Add rendered html and highlight results.
* In content add title.
* Modify title to fulltext.
* Processors check Ignore case.
* Save and Reindex.

Go to views and create a new one:
* View name "Search".
* Create a page "checked".
* In format Show Rendered entity.
* View mode "Search result highlighting input".
* In advanced Contextual filter "Fulltext search".
* Provide def value "Query parameter".
* query parameter "text".
* Fallback value "all".
* Select "title" for field.
* Skip default checked.
* Check "contains any of these words".
* Save it.

Go to search api autocomplete and enable label:
* Edit and check retrieve from server.

Go to block:
* Check autocomplete part.
* In form action "search".
* In autocomplete label, "search".
* In display "page_1".

Create a new content.

### 4. News and events content type.
[oe_starter_content](https://github.com/openeuropa/oe_starter_content) repository.
```
$ docker-compose exec web composer require openeuropa/oe_content
$ docker-compose exec web composer require "openeuropa/oe_starter_content:^1.0.0-beta1"
$ docker-compose exec web drush en oe_whitelabel_starter_event -y && docker-compose exec web drush en oe_whitelabel_starter_news -y
$ docker-compose exec web drush config-delete media.type.remote_video && docker-compose exec web drush config-delete media.type.image && docker-compose exec web drush config-delete media.type.document && docker-compose exec web drush config-delete core.entity_view_display.media.remote_video.default && docker-compose exec web drush config-delete core.entity_view_display.media.image.default && docker-compose exec web drush config-delete core.entity_view_display.media.document.default && docker-compose exec web drush config-delete core.entity_form_display.media.remote_video.default && docker-compose exec web drush config-delete core.entity_form_display.media.image.default && docker-compose exec web drush config-delete core.entity_form_display.media.document.default
$ docker-compose exec web drush en oe_whitelabel_starter_event -y && docker-compose exec web drush en oe_whitelabel_starter_news -y
```

### 5. Contact form
[oe_contact_forms](https://github.com/openeuropa/oe_contact_forms) repository.
```
$ docker-compose exec web composer require openeuropa/oe_contact_forms
$ docker-compose exec web drush en oe_whitelabel_contact_forms -y
$ docker-compose exec web composer require drupal/description_list_field
$ docker-compose exec web composer require openeuropa/oe_paragraphs
$ docker-compose exec web drush en oe_whitelabel_paragraphs -y
$ docker-compose exec web composer require drupal/block_field
$ docker-compose exec web drush en block_field -y
```
* Create a form through ui (check corporate form).
* Go to paragraph and create one contact form.
* Go to fields and add block.
* Select category and corporate form.
* Click on save.
* Go to basic page and create e new one, place the form.
* Click on save.

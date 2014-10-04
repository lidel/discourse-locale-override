# discourse-locale-override

Simple hack to override official translations files in Discourse and persist them between Docker rebuilds.
Can be used by translators, early adopters of young locales or communities with specific slang.

**This repository contains only `pl_PL` locale. See FAQ below if you need to override a different one.**

## Docker setup

Add to your `/var/discourse/containers/app.yml`:

```ruby
hooks:
  after_code:
    - exec:
        cd: /tmp
        cmd:
          - git clone https://github.com/lidel/discourse-locale-override.git
          - mv discourse-locale-override/*.yml $home/config/locales/
          - mv discourse-locale-override/plugins/emoji/*.yml $home/plugins/emoji/config/locales/
          - mv discourse-locale-override/plugins/poll/*.yml $home/plugins/poll/config/locales/
          - rm -rf discourse-locale-override
```

Rebuild Discourse: `/var/discourse/launcher rebuild app`

## FAQ

### How can I use this to override locale X?

1. Fork this repository
2. Remove `*.pl_PL.yml` files *(optional)*
3. Add `*.xx_XX.yml` ones     
   **Warning:** `xx_XX` should be the language code you want to *override*. Files have to be already present in [discourse/config/locales](https://github.com/discourse/discourse/tree/master/config/locales). **This is important:** adding non-existing locale codes will *break your instance*.
4. Update repository URL in Docker hook to point to your fork

### Where do I get original `.yml` files from?

There are two places I am aware of: 
- Github: https://github.com/discourse/discourse/tree/master/config/locales
- Transifex: https://www.transifex.com/projects/p/discourse-org/ 

If you care about latest translations use Transifex. 
Github is updated with Transifex changes about once a week. 


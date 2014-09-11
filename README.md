# discourse-locale-override

Simple hack to override official translations files in Discourse and persist it between Docker rebuilds.
Can be used by translators, early adopters of young locales, 

## Docker setup

Add to `/var/discourse/containers/app.yml`:

```ruby
hooks:
  after_code:
        cd: /tmp
        cmd:
          - git clone https://github.com/lidel/discourse-locale-override.git
          - mv discourse-locale-override/*.yml $home/config/locales/
          - rm -rf discourse-locale-override
```

Rebuild Discourse: `/var/discourse/launcher rebuild app`

## FAQ

### How can I use this to override locale X?

1. fork this repo
2. remove `*.pl_PL.yml` files
3. add `*.X.yml` ones
4. remember to update URL in Docker hook accordingly

### Where do I get original `.yml` files from?

There are two places I am aware of: 
- Github: https://github.com/discourse/discourse/tree/master/config/locales
- Transifex: https://www.transifex.com/projects/p/discourse-org/ 

If you care about latest translations use Transifex. 
Github is updated with Transifex changes about once a week. 


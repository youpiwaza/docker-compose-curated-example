# Docker compose curated example

Set up both nginx & phpfpm, with a sample page, available on [localhost:80](http://localhost:80/), for convenience (no specific port to set in firewall)

See the [original project](https://github.com/youpiwaza/docker-compose-example) for more comments on nginx + phpfpm setup.

**Curated** > Apply most docker security & best practices to the build.

```bash
# Go to your own folder
# > cd ~/../c/Users/Patolash/Documents/_dev/docker-compose-curated-example

# Start composition
> docker-compose up

# Stop/rm composition
# Ctrl + c

# Use dc down if launched with -d
> docker-compose down
```

Sources :

- *File docker-compose.yml*
- *Folder sources /src*
- *Folder configs /config*
  - Also contains defaults config files for both images, for reference

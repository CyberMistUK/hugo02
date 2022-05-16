---
title: "Installing Hugo on Netlify"
date: 2022-04-20T13:24:20Z
draft: false
---

## Preface

Using a Windows system running WSL2. Can't install Hugo on Windows due to lack of admin rights. Can't install Hugo on WSL2 using snapd due to the fact WSL doesn't use Systemd. So have used the Docker container linked to at <https://gohugo.io/getting-started/installing#docker> in WSL2 using Podman! (Had previously installed Podman on WSL2)

## Issues found

Running `podman run --rm -it -v $(pwd):/src klakegg/hugo:0.93.2` generated an error so had to run `podman run --rm -it -v $(pwd):/src  klakegg/hugo:latest "new site my-hugo-site"` first.

Added a theme using `git submodule` and then copied the content of the theme's config.toml to the site's main config.toml and then got errors when trying to add new pages via the `hugo new blah.md` command. The copied config had settings regarding two languages, so removed the config for the second language but still got the error. Had to create a 'en' folder with the main content folder before I could then add new content as the copied config had a contentDir setting set to `contentDir = "content/en"`

To run the built in development server you need to run `podman run --rm -it -v $(pwd):/src -p 1313:1313 klakegg/hugo:latest server` or `podman run --rm -it -v $(pwd):/src -p 8080:1313 klakegg/hugo:latest server` to run on port 8080 (as this is podman you cannot set the port to 80 unless using sudo(??))

Pages by default are set to draft so to compile draft pages use `podman run --rm -it -v $(pwd):/src  klakegg/hugo:latest "-D"`

If you need to use the extended version of Hugo (to build SCSS/SASS for example) you cannot use the `latest` tag. According to the info at <https://hub.docker.com/r/klakegg/hugo/> you need to use one of the images which start `ext-`. In this instance I used the Alpine extended image `ext-alpine`:

`podman run --rm -it -v $(pwd):/src klakegg/hugo:ext-alpine` and

`podman run --rm -it -v $(pwd):/src -p 1313:1313 klakegg/hugo:ext-alpine server`

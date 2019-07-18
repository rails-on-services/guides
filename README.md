
## Setup

```bash
bundle install
```

## Running

```bash
bundle exec jekyll serve -H 0.0.0.0 -l
```

If jekyll is loading files from a vagrant machine on a shared folder then live reload alone will not work.
Force jekyll to poll the file system for changes with the following options:

```bash
bundle exec jekyll serve -H 0.0.0.0 -l --watch --force_polling
```

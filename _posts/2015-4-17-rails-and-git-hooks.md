---
layout: post
title: Rails and Git hooks
---

### Automate your repeated task like `bundle install` and `rake db:migrate` on every pull.

###Procedure:

* Enter into hooks directory in your projects.

```
cd myawesomeproject/.git/hooks
```

* Create a post-merge file in hooks directory.

```
touch post-merge
```

* Open the post-merge file.

```
vim post-merge
```

* Add the following script to your file:

```
#/usr/bin/env bash
changed_files="$(git diff-tree -r --name-only --no-commit-id ORIG_HEAD HEAD)"

echo $changed_files

check_run() {
  echo "$changed_files" | grep -E --quiet "$1" && eval "$2"
}

check_run Gemfile "bundle install"

check_and_run_migrations(){
  git diff --name-only HEAD@{1} HEAD | grep -E "db/migrate" && eval "rake db:migrate"
}

check_and_run_migrations
```

* Save and exit.
* File is a shell script so we need to make it executable.

```
chmod +x post-merge
```

* From now on everypull script will check if any new dependency(Gem) or new migration it will be resolved automatically.


####Happy coding :)


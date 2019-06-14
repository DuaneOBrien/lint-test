## Repolinter Test Project

![NetflixOSS Lifecycle](https://img.shields.io/osslifecycle/duaneobrien/lint-test.svg)

This is a test project for evaluating custom rules for [Repolinter](https://github.com/todogroup/repolinter/)


## Command Line Linting Of All Projects In A Repo

(You'll need `jq` installed for this bit, I was able to do this with `brew install jq`)

If you want to lint all your org's repos locally, you can iterate over the list of repos and clone them using the following one-liner:

`curl -s https://YOURGITHUBID:@api.github.com/orgs/YOURGITHUBORG/repos?per_page=200 | jq .[].ssh_url | xargs -n 1 git clone`

If you want to filter out forks and archived repositories, you can use this one-liner:

`curl -s https://YOURGITHUBID:@api.github.com/orgs/YOURGITHUBORG/repos?per_page=200&type=sources&sort=full_name | jq '.[] | select(.archived == false) | .ssh_url' | xargs -n 1 git clone`

I recommend doing this in a directory you use only for this purpose (I used `~/Working/`)

If you have many large repositories, that may have taken some time.

Before running repolinter, copy the `repolint.json` file into the `~/Working/` directory (or your Home directory).

You can generate a report for each of the cloned repos by running the following one-liner from within your local copy of `repolinter`:

`echo  >> results.txt;for i in ~/Working/*/;do node bin/repolinter.js "$i" >> results.txt; echo  >> results.txt;done;cat results.txt`

(Note, this assumes you've cloned everything into `~/Working/` like I did)

## Credits

I learned the curl one-liner from [This Article](https://medium.com/@kevinsimper/how-to-clone-all-repositories-in-a-github-organization-8ccc6c4bd9df) by Kevin Simpler

## License

MIT

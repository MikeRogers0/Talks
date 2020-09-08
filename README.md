# Talks

Repo of all the talks I've made.

## Usage

I'm using [Marp](https://marp.app/#get-started) to convert the slides markdown to HTML, you can run it like:

```bash
docker run --rm --init -v $PWD:/home/marp/app -e LANG=$LANG -p 8080:8080 -p 37717:37717 marpteam/marp-cli -s -w .
```

or:

```bash
npm start
```

Then visit http://127.0.0.1:8080/

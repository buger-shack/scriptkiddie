# Exposed .git

## Finding it

- Firefox/Chrome extension : **DotGit**

- Fuzzing with feroxbuster :

  ```bash
  feroxbuster -u <url> -x git
  ```

- Downloading with wget :

  ```bash
  wget --mirror -I .git <url>
  # --mirror : Makes it download everything from git, recursive download.
  # -I : Creates a file with everything that was downloaded.
  ```

- [GitTools](https://github.com/internetwache/GitTools)

## Interacting with it

```bash
# GitTools
./gitdumper.sh <url-with-.git> <dest-folder>

# extract commits and contents
./extractor <.git-repo> <dest-extract>
```


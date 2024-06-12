# AnythingLLM + Ollama in docker-compose

This setup blocks internet access for AnythingLLM and ollama containers for added reassurance that RAG will work offline.


## First time only

### Step 1

Pre-requirements:

```shell
mkdir ./storage
touch .env
sqlite3 ./storage/anythingllm.db "VACUUM;"
```

### Step 2

Allow internet access to `anything-llm` and `ollama` containers:

Inside `./docker-compose.yml`, uncomment `internet` entry of `networks` for the `anything-llm` and `ollama` containers.

### Step 3

Start the services:

```shell
docker-compose up
```

### Step 4

In different terminal, pull the `llama3` model.

```shell
docker-compose exec ollama ollama pull llama3
```

### Step 5

Inside AnythingLLM (in browser `http://localhost:3001`), run one dummy embed of the file since AnythingLLM downloads the embedding model on first run.

- Create some dummy workspace
- Upload one dummy text file and embed it (click `Save and Embed` when upload and select the document)
- Discard whole workspace when done (optional)


### Step 6

Stop all services, use Ctrl+C on the terminal where you ran `docker-compose up`.

### Step 7

Deny internet access to `anything-llm` and `ollama` containers:

Inside `./docker-compose.yml`, make sure you comment out or remove `internet` entry of `networks` under the `anything-llm` and `ollama` containers.


## Usage

Once the `First time only` steps are done, run as normally:

```shell
docker-compose up
```

Then navigate to browser to access `http://localhost:3001`.

Done.

Note:
- Make sure you setup your workspace settings with the `llama3` model on the `Chat Settings` and `Agent Configuration`.


## Known issues

If you can't save settings through AnythingLLM interface, or if you have any other read permission issues when running `docker-compose`:

```shell
chmod o+w ./storage ./collector ./.env
```

<!--⚠️ Note that this file is in Markdown but contain specific syntax for our doc-builder (similar to MDX) that may not be
rendered properly in your Markdown viewer.
-->

# Quickstart

The [Hugging Face Hub](https://huggingface.co/) is the go-to place for sharing machine learning
models, demos, datasets, and metrics. `huggingface_hub` library helps you interact with
the Hub without leaving your development environment. You can create and manage
repositories easily, download and upload files, and get useful model and dataset
metadata from the Hub.

## Installation

To get started, install the `huggingface_hub` library:

```bash
pip install --upgrade huggingface_hub
```

For more details, check out the [installation](installation) guide.

## Download files

Repositories on the Hub are git version controlled, and users can download a single file
or the whole repository. You can use the [`hf_hub_download`] function to download files.
This function will download and cache a file on your local disk. The next time you need
that file, it will load from your cache, so you don't need to re-download it.

You will need the repository id and the filename of the file you want to download. For
example, to download the [Pegasus](https://huggingface.co/google/pegasus-xsum) model
configuration file: 

```py
>>> from huggingface_hub import hf_hub_download
>>> hf_hub_download(repo_id="google/pegasus-xsum", filename="config.json")
```

To download a specific version of the file, use the `revision` parameter to specify the
branch name, tag, or commit hash. If you choose to use the commit hash, it must be the
full-length hash instead of the shorter 7-character commit hash: 

```py
>>> from huggingface_hub import hf_hub_download
>>> hf_hub_download(
...     repo_id="google/pegasus-xsum", 
...     filename="config.json", 
...     revision="4d33b01d79672f27f001f6abade33f22d993b151"
... )
```

For more details and options, see the API reference for [`hf_hub_download`].

## Login

In a lot of cases, you must be logged in with a Hugging Face account to interact with
the Hub: download private repos, upload files, create PRs,...
[Create an account](https://huggingface.co/join) if you don't already have one, and then sign in
to get your [User Access Token](https://huggingface.co/docs/hub/security-tokens) from
your [Settings page](https://huggingface.co/settings/tokens). The User Access Token is
used to authenticate your identity to the Hub.

Once you have your User Access Token, run the following command in your terminal:

```bash
huggingface-cli login
# or using an environment variable
huggingface-cli login --token $HUGGINGFACE_TOKEN
```

Alternatively, you can programmatically login using [`login`] in a notebook or a script:

```py
>>> from huggingface_hub import login
>>> login()
```

It is also possible to login programmatically without being prompted to enter your token by directly
passing the token to [`login`] like `login(token="hf_xxx")`. If you do so, be careful when
sharing your source code. It is a best practice to load the token from a secure vault instead
of saving it explicitly in your codebase/notebook.

You can be logged in only to 1 account at a time. If you login your machine to a new account, you will get logged out
from the previous. Make sure to always which account you are using with the command `huggingface-cli whoami`.
If you want to handle several accounts in the same script, you can provide your token when calling each method. This
is also useful if you don't want to store any token on your machine.

<Tip warning={true}>

Once you are logged in, all requests to the Hub -even methods that don't necessarily require authentication- will use your
access token by default. If you want to disable implicit use of your token, you should set the
`HF_HUB_DISABLE_IMPLICIT_TOKEN` environment variable.

</Tip>

## Create a repository

Once you've registered and logged in, create a repository with the [`create_repo`]
function:

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> api.create_repo(repo_id="super-cool-model")
```

If you want your repository to be private, then:

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> api.create_repo(repo_id="super-cool-model", private=True)
```

Private repositories will not be visible to anyone except yourself.

<Tip>

To create a repository or to push content to the Hub, you must provide a User Access
Token that has the `write` permission. You can choose the permission when creating the
token in your [Settings page](https://huggingface.co/settings/tokens).

</Tip>

## Upload files

Use the [`upload_file`] function to add a file to your newly created repository. You
need to specify:

1. The path of the file to upload.
2. The path of the file in the repository.
3. The repository id of where you want to add the file.

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> api.upload_file(
...     path_or_fileobj="/home/lysandre/dummy-test/README.md",
...     path_in_repo="README.md",
...     repo_id="lysandre/test-model",
... )
```

To upload more than one file at a time, take a look at the [Upload](./guides/upload) guide
which will introduce you to several methods for uploading files (with or without git).

## Next steps

The `huggingface_hub` library provides an easy way for users to interact with the Hub
with Python. To learn more about how you can manage your files and repositories on the
Hub, we recommend reading our [how-to guides](./guides/overview) to:

- [Manage your repository](./guides/repository).
- [Download](./guides/download) files from the Hub.
- [Upload](./guides/upload) files to the Hub.
- [Search the Hub](./guides/search) for your desired model or dataset.
- [Access the Inference API](./guides/inference) for fast inference.
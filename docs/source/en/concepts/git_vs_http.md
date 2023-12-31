<!--⚠️ Note that this file is in Markdown but contain specific syntax for our doc-builder (similar to MDX) that may not be
rendered properly in your Markdown viewer.
-->

# Git vs HTTP paradigm

The `huggingface_hub` library is a library for interacting with the Hugging Face Hub, which is a 
collections of git-based repositories (models, datasets or Spaces). There are two main
ways to access the Hub using `huggingface_hub`.

The first approach, the so-called "git-based" approach, is led by the [`Repository`] class.
This method uses a wrapper around the `git` command with additional functions specifically
designed to interact with the Hub. The second option, called the "HTTP-based" approach,
involves making HTTP requests using the [`HfApi`] client. Let's examine the pros and cons
of each approach.

## Repository: the historical git-based approach

At first, `huggingface_hub` was mostly built around the [`Repository`] class. It provides
Python wrappers for common `git` commands such as `"git add"`, `"git commit"`, `"git push"`,
`"git tag"`, `"git checkout"`, etc.

The library also helps with setting credentials and tracking large files, which are often
used in machine learning repositories. Additionally, the library allows you to execute its
methods in the background, making it useful for uploading data during training.

The main advantage of using a [`Repository`] is that it allows you to maintain a local
copy of the entire repository on your machine. This can also be a disadvantage as
it requires you to constantly update and maintain this local copy. This is similar to
traditional software development where each developer maintains their own local copy and
pushes changes when working on a feature. However, in the context of machine learning,
this may not always be necessary as users may only need to download weights for inference
or convert weights from one format to another without the need to clone the entire
repository.

## HfApi: a flexible and convenient HTTP client

The [`HfApi`] class was developed to provide an alternative to local git repositories, which
can be cumbersome to maintain, especially when dealing with large models or datasets. The
[`HfApi`] class offers the same functionality as git-based approaches, such as downloading
and pushing files and creating branches and tags, but without the need for a local folder
that needs to be kept in sync.

In addition to the functionalities already provided by `git`, the [`HfApi`] class offers
additional features, such as the ability to manage repos, download files using caching for
efficient reuse, search the Hub for repos and metadata, access community features such as
discussions, PRs, and comments, and configure Spaces hardware and secrets.

## What should I use ? And when ?

Overall, the **HTTP-based approach is the recommended way to use** `huggingface_hub`
in most cases. However, there are a few situations where maintaining a local git clone
(using [`Repository`]) may be more beneficial:
- If you are training a model on your machine, it may be more efficient to use a traditional
git-based workflow, pushing regular updates. [`Repository`] is optimized for this type of
situation with its ability to work in the background.
- If you need to manually edit large files, `git` is the best option as it only sends the
diff to the server. With the [`HfAPI`] client, the entire file is uploaded with each edit.
Do keep in mind that most large files are binary so do not benefit from git diffs anyway.

Not all git commands are available through [`HfApi`]. Some may never be implemented, but
we are always trying to improve and close the gap. If you don't see your use case covered,
please open [an issue on Github](https://github.com/huggingface/huggingface_hub)! We
welcome feedback to help build the 🤗 ecosystem with and for our users.

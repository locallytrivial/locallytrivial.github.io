---
title: "Keeping Up with ArXiv using Sxolar"
date: 2024-12-25
draft: false
categories:
  - Physics
  - Code
tags:
  - Python
  - ArXiv
---

## Overview & Motivation

Like many researchers, I use [ArXiv](https://arxiv.org) to keep up with the latest research in my field. However, I find
it difficult to keep track of all the new papers that are posted each day. I have explored many of the existing tools
for tracking ArXiv, but I have not found one that meets my simple requirements. All I wanted was a tool that would let
me configure a set of queries, and send me period emails with the new papers that match those queries. Having found no
such tool, I decided to build my own. This post introduces [`sxolar`](https://www.sxolar.org), a Python library that
allows you to search ArXiv, and shows how to use it to keep up with the latest research in your field of interest.

> "All I wanted was a tool that would let me configure a set of queries,
> and send me period emails with the new papers that match those queries."

Before getting into the simple but robust features of `sxolar`, let's take a look at existing tools for tracking ArXiv.

### Existing Tools for Tracking ArXiv

There are several existing tools for tracking ArXiv, but they all have their limitations. Some of the most popular tools
include:

- [ArXiv Email Alerts](https://arxiv.org/help/subscribe): ArXiv provides an email alert service that allows you to
  subscribe to new papers in specific categories. However, there is no control over the other elements of the search
  API, including authors, keywords, etc.
- [ArXiv Sanity Preserver](http://www.arxiv-sanity.com): A web-based tool that allows you to search ArXiv and get daily
  email alerts for new papers in your field of interest. However, it does not allow you to configure custom queries.
  Instead, it uses a machine learning model to recommend papers based on your reading history.
- [iArXiv](https://iarxiv.com): Similar to ArXiv Sanity Preserver, iArXiv is a web-based tool that allows you to search
  ArXiv and get daily email alerts for new papers in your field of interest. However, it does not allow you to configure
  custom queries. Instead, it uses a machine learning model to recommend papers based on your reading history.
- [ArXiv RSS Feeds](https://arxiv.org/help/rss): ArXiv provides RSS feeds for each category, which you can subscribe to
  in
  your favorite RSS reader. However, this requires you to manually check the feeds each day.
- [ArXiv API](https://arxiv.org/help/api): ArXiv provides an API that allows you to search for papers and get metadata
  about them. However, this requires you to write code to interact with the API.

### Introducing Sxolar

[Sxolar](https://www.sxolar.org) is a Python library that allows you to search ArXiv and get daily email alerts for new
papers in your field of interest. It is designed to be simple and easy to use, with a focus on customizability and
flexibility. It offers a command-line interface, and is easy to use with GitHub actions for automated daily email
alerts. Similar to existing wrappers, `sxolar` uses the ArXiv API to search for papers, and offers the ability to search
by authors, keywords, categories, and more. What makes `sxolar` unique? `sxolar can:

- Search by all fields (title, abstract, authors, etc.) as well as arbitrarily complex logical expressions of these
  fields (e.g., `"title:quantum AND abstract:gravity"`).
- Syntactic sugar for building complex queries (e.g., `(Title("quantum") & Abstract("gravity")).search()`).
- Persist queries to simple configuration files for easy reuse and modification.
- Send email alerts with summaries of new papers that match your queries

This library is relatively new, and I am actively developing it. If you have any feature requests or bug reports, please
feel free to open an issue on the [GitHub repository](https://github.com/JWKennington/sxolar/issues). I hope you find
this tool useful; it has certainly made my life easier!

## Setting Up Periodic Email Alerts with Sxolar

This post is focused on using `sxolar`
to [setup a periodic email digest of ArXiv papers](https://www.sxolar.org/tutorials/setup-periodic/) in your field of
interest. To see more detailed documentation on the library, please visit
the [official documentation](https://www.sxolar.org). [^1] For the purpose of this post, we'll use my field of study (
gravitational waves) to determine a sample query: searching for papers released by the LIGO or VIRGO scientific
collaborations. The package also has a [tutorial](https://www.sxolar.org/tutorials/setup-periodic/) for setting up a
periodic digest. [^2] This post will be divided into the following steps:

1. Configuration of the queries and summary
2. Setup of Google Mail Access
3. Scheduling with GitHub Actions

To get started with `sxolar`, you will need to install the library. You can do this using `pip`:

```bash
pip install sxolar
```

### Step 1: Configure the Queries and Summary

A `Summary` is a collection of `Section`s, each of which represents a query with related search parameters (such as time
period). The summary info can be persisted to a config file (e.g., `summary.yaml`) as shown below:

```yaml
LIGO Virgo Summary:
  - name: "LIGO: Recent 2 Weeks"
    authors: [ "LIGO Scientific Collaboration" ]
    alls: [ "gravitational wave" ]
    trailing:
      num: 14
      unit: "days"

  - name: "Virgo: Recent 2 Months"
    authors: [ "Virgo Collaboration" ]
    alls: [ "gravitational wave" ]
    trailing:
      num: 2
      unit: "months"
```

The configuration file specifies one summary "LIGO Virgo Summary" with two sections: "LIGO: Recent 2 Weeks" and "Virgo:
Recent 2 Months". Each section specifies the authors, search terms, and trailing time period for the search query. The
"trailing" field specifies the number of days or months to search back from the current date.

#### Testing the Configuration

The summary can be generated either using python code or the command line interface. The command-line is
relatively simple to use for testing a summary:

```bash
sxolar summary --config summary.yaml --name "LIGO Virgo Summary" --output print
````

For more detailed testing, the python library can be used directly. The following code snippet demonstrates how to
generate a summary from the config file using python:

```python
from sxolar import Config, Summary

# Load the configuration file, this will also parse the summary objects
config = Config("summary.yaml")

# Get the summary object for "summary name 1"
s1 = config.summaries("summary name 1")
assert isinstance(s1, Summary)

# Generate the summary first by refreshing the query
s1.refresh()

# Print the summary (plain)
print(s1.to_text())

# Print the summary (html, usually for email)
print(s1.to_html())
```

### Step 2: Setup Google Mail Access

To send email alerts, `sxolar` uses the `smtplib` library to send emails through a Gmail account. This is made possible
(and relatively secure) by using an app password. The library documentation includes instructions for [setting up an app
password](https://www.sxolar.org/tutorials/setup-email/). To set up a Gmail account for sending emails, follow these
steps:

1. Go to App Passwords.
2. Enter an app name, e.g. "SampleApp".
3. Click "Create".
4. Copy the generated app password.

The generated app password is a 16-character code that you will use to authenticate your application.

`App password: "abcd efgh ijkl mnop"`

Specifically, we will set an environment variable in the GitHub repository to store the app password. This will allow
the GitHub
Actions to send emails on your behalf.

### Step 3: Scheduling with GitHub Actions

To schedule the email alerts, we will use GitHub Actions. The below snippet is a sample workflow file (
`.github/workflows/sxolar.yml`) that will run the `sxolar` summary command weekly on Sunday at 8am EST (1pm UTC).
For a sample repository, see [sxolar-template-run](https://github.com/JWKennington/sxolar-template-run).

```yaml
name: Example Sxolar Run
on:
  # Uncomment the below to also run on pushes, as a way to test
  #  push:
  #      branches:
  #      - main
  schedule:
    # Run Weekly on Sunday at 8am EST (1pm UTC)
    # For more detail on cron syntax, see https://crontab.guru/
    - cron: '0 13 * * 0'

jobs:
  MySummary:
    # Setup the minimal environment: linux and python 3.11
    name: UNIX Build (${{ matrix.python-version }}, ${{ matrix.os }})
    runs-on: ${{ matrix.os }}
    defaults:
      run:
        shell: bash -l {0}
    strategy:
      fail-fast: false
      max-parallel: 4
      matrix:
        os: [ "ubuntu-latest" ]
        python-version: [ "3.11" ]

    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      # Install the necessary dependencies (sxolar)
      - name: Install sxolar
        run: |
          pip install sxolar

      # Run the summary using the command line and config file
      - name: Run GWaves
        run: |
          sxolar summary --config configs/gwaves.yml \
            --name GWaves \
            --email-to myrecipient@gmail.com \
            --email-from myemail@gmail.com \
            --email-subject "Sxolar Weekly Digest: GWwaves" \
            --gmail-app-password "${{ secrets.SXOLARGMAILAPPPASSWORD }}" \
            --output email
```

This workflow file will run the `sxolar` summary command weekly on Sunday at 8am EST (1pm UTC). The command will use the
config file `configs/gwaves.yml` to generate the summary, and will send the email to `myrecipient@gmail.com` from
`myemail@gmail.com`. The email will have the subject "Sxolar Weekly Digest: GWwaves". The app password is stored in the
GitHub repository secrets as `SXOLARGMAILAPPPASSWORD`.

## Conclusion

In this post, we introduced `sxolar`, a Python library that allows you to search ArXiv and get daily email alerts for
new
papers in your field of interest. We discussed the limitations of existing tools for tracking ArXiv, and showed how
`sxolar` addresses these limitations. We walked through the process of setting up periodic email alerts with `sxolar`,
including configuring the queries and summary, setting up Google Mail access, and scheduling with GitHub Actions. I hope
you find this tool useful for keeping up with the latest research in your field of interest. If you have any feature
requests or bug reports, please feel free to open an issue on
the [GitHub repository](https://github.com/JWKennington/sxolar/issues). Happy reading!

## References


[^1]: Sxolar: Scholars tools for ArXiv: [Documentation](https://www.sxolar.org)
[^2]: Sxolar: Setting up Periodic Search: [Tutorial](https://www.sxolar.org/tutorials/setup-periodic/)


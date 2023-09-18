---
title: DB Essentials
author: Paul
date: 2023-09-17 12:33:00 +0100
categories: [Data, Engineer, Database, DB]
tags: [engineering,DB,docs]
pin: true
math: false
mermaid: false
---

## Using psql

* `psql` is command line utility to connect to the Postgres database server. It is typically used for the following by advanced Database users:
  * Manage Databases
  * Manage Tables
  * Load data into tables for testing purposes
* We need to have at least Postgres Client installed on the server from which you want to use psql to connect to Postgres Server.
* If you are on the server where **Postgres Database Server** is installed, `psql` will be automatically available.
* We can run `sudo -u postgres psql -U postgres` from the server provided you have sudo permissions on the server. Otherwise we need to go with `psql -U postgres -W` which will prompt for the password.
* **postgres** is the super user for the postgres server and hence typically developers will not have access to it in non development environments.
* As a developer, we can use following command to connect to a database setup on postgres server using user credentials.

```shell
psql  -h <host_ip_or_dns_alias> -d <db_name> -U <user_name> -W

# Here is the example to connect to itversity_sms_db using itversity_sms_user
psql -h localhost -p 5432 -d itversity_sms_db -U itversity_sms_user -W
```

* We typically use `psql` to troubleshoot the issues in non development servers. IDEs such as **SQL Alchemy** might be better for regular usage as part of development and unit testing process.
* For this course, we will be primarily using Jupyter based environment for practice.
* However, we will go through some of the important commands to get comfortable with `psql`.
  * Listing Databases - `\l`
  * Switching to a Database - `\c <DATABASE_NAME>`
  * Get help for `psql` - `\?`
  * Listing tables - `\d`
  * Create table - `CREATE TABLE t (i SERIAL PRIMARY KEY)`
  * Get details related to a table - `\d <table_name>`
  * Running Scripts - `\i <SCRIPT_PATH>`
  * You will go through some of the commands over a period of time.

## Setup Postgres using Docker

> Same, just port difference (5433:5432 NOT 5432:5432)
{: .prompt-tip }

## Setup SQL Workbench

<h3 data-toc-skip>Why SQL Workbench</h3>

Let us see the details why we might have to use SQL Workbench.

* Using Database CLIs such as psql for postgres, mysql etc can be cumbersome for those who are not comfortable with command line interfaces.
* Database IDEs such as SQL Workbench will provide required features to run queries against databases with out worrying to much about underlying data dictionaries.
* SQL Workbench provide required features to review databases and objects with out writing queries or running database specific commands.
* Also Database IDEs provide capabilities to preserve the scripts we develop.

> **In short Database IDEs such as SQL Workbench improves productivity.**

<h3 data-toc-skip>Alternative IDEs</h3>

There are several IDEs in the market.

* TOAD
* SQL Developer for Oracle
* MySQL Workbench
and many others

<h3 data-toc-skip>Install SQL Workbench</h3>

Here are the instructions to setup SQL Workbench.

* Download SQL Workbench (typically zip file)
* Unzip and launch

Once installed we need to perform below steps which will be covered in detail as part of next topic.

* Download JDBC driver for the database we would like to connect.
* Get the database connectivity information and connect to the database.

## usefull emoji's

full list can be found [Here](https://gist.github.com/rxaviers/7360908) and for markdown [here](https://github.com/ikatyang/emoji-cheat-sheet)

💥 👉 👈 👇 ☝️ 📌 📆  📈  📘 📒 📕 📗 📓 🔬 🔖 💡 📢  🔍 🔋 🔧 🎉 ⚡ ☁️ 🐍 🐼 🐌 

🐳 🔥 ❗ ❓ ⭐ 💨 💦 👽 👍💪 🙈  🙉 🙊 👀 🚀 🌝 🏁 🔑 🔒 🕙 💯 ✔️ ✅ 🔗

markdown:

🪙 📎  📂

This post is to show Markdown syntax rendering on [**Chirpy**](https://github.com/cotes2020/jekyll-theme-chirpy/fork), you can also use it as an example of writing. Now, let's start looking at text and typography.

## Headings

<h1 class="mt-5">H1 - heading</h1>

<h2 data-toc-skip>H2 - heading</h2>

<h3 data-toc-skip>H3 - heading</h3>

<h4>H4 - heading</h4>

## Lists

### Ordered list

1. Firstly
2. Secondly
3. Thirdly

### Unordered list

- Chapter
  + Section
    * Paragraph

### ToDo list

- [ ] Job
  + [x] Step 1
  + [x] Step 2
  + [ ] Step 3

### Description list

Sun
: the star around which the earth orbits

Moon
: the natural satellite of the earth, visible by reflected light from the sun

## Block Quote

> This line shows the _block quote_.

## Prompts

> An example showing the `tip` type prompt.
{: .prompt-tip }

> An example showing the `info` type prompt.
{: .prompt-info }

> An example showing the `warning` type prompt.
{: .prompt-warning }

> An example showing the `danger` type prompt.
{: .prompt-danger }

## Tables

| Company                      | Contact          | Country |
|:-----------------------------|:-----------------|--------:|
| Alfreds Futterkiste          | Maria Anders     | Germany |
| Island Trading               | Helen Bennett    | UK      |
| Magazzini Alimentari Riuniti | Giovanni Rovelli | Italy   |

## Links

<http://127.0.0.1:4000>

## Footnote

Click the hook will locate the footnote[^footnote], and here is another footnote[^fn-nth-2].

## Inline code

This is an example of `Inline Code`.

## Filepath

Here is the `/path/to/the/file.extend`{: .filepath}.

## Code blocks

### Common

```
This is a common code snippet, without syntax highlight and line number.
```

### Specific Language

```bash
if [ $? -ne 0 ]; then
  echo "The command was not successful.";
  #do the needful / exit
fi;
```

### Specific filename

```sass
@import
  "colors/light-typography",
  "colors/dark-typography";
```

{: file='_sass/jekyll-theme-chirpy.scss'}

## Mathematics

The mathematics powered by [**MathJax**](https://www.mathjax.org/):

$$ \sum_{n=1}^\infty 1/n^2 = \frac{\pi^2}{6} $$

When $a \ne 0$, there are two solutions to $ax^2 + bx + c = 0$ and they are

$$ x = {-b \pm \sqrt{b^2-4ac} \over 2a} $$

### Dark/Light mode & Shadow

The image below will toggle dark/light mode based on theme preference, notice it has shadows.

## Video

{% include embed/youtube.html id='Balreaj8Yqs' %}

## Reverse Footnote

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
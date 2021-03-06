Title: Sensible Defaults
Date: 2017-7-16
Category: Blog
Tags: python, postgres
Author: Amjith Ramanujam

When I first set out to create [pgcli](https://www.pgcli.com), my goal was to
design a Postgresql client that shipped with sensible defaults. It shouldn't
require fiddling with config files to enable features.

How did we do on that goal? This is one of those subjective goals that is not
so easy to measure. But fortunately, [Craig](http://www.craigkerstiens.com)
posted a useful
[blogpost](https://www.citusdata.com/blog/2017/07/16/customizing-my-postgres-shell-using-psqlrc/)
that shows how to configure your `psql` shell to make it powerful. 

I figured I'll use that as a scoring card to see how many of those features
are shipped by default in pgcli.

## Prompt

One of the things covered in the blogpost is customizing the prompt to show the
server name and the database name.

Pgcli ships with a default prompt that has `user@host:dbname>`. When you're
writing a multi-line query the subsequent lines will be indented and filled
with `.....`.

<img src='/images/prompt.png' width=750px align=center alt='prompt'/>

Both of these can be overridden via the config file (~/.config/pgcli/config)
but you hardly ever have to change the defaults. 

## Null values

By default `psql` won't show the NULL values in a table. You could force psql
to show a placeholder value via `pset null`. In `pgcli` the null values are
always shown and they are show as `<null>`.

<img src='/images/null.png' width=750px align=center alt='null'/>

Once again this can be overridden to any character of your preference via the
config file.

## Timing

You can time your sql queries by enabling `\timing` in psql. This is enabled by
default in pgcli. Every query is timed and the results are displayed at the
bottom of the output.

## History

History in pgcli is unlimited. But pgcli has no option to separate the history
for different databases. 

The feature described in the blogpost talks about a cool feature to store the
history of a session scoped to a database. This is an awesome feature that we
might adopt in pgcli in the future versions.

## Output Formatting

Expanded mode in psql is a way to output the results of a query if the output
is too wide to fit in the screen when shown as a table. This can be toggled by
`\x on` or `\x off`.

The `\x auto` command in `psql` will intelligently choose between the table
format or the expanded format based on the screen width. 

In pgcli, we have the ability to do this but this is not enabled by default. It
has to be enabled via the config file (~/.config/pgcli/config) by the user.

I don't think we'll change this behavior. 

## Keyword Casing

One of the psql options is to choose either the upper case or lower case for
keyword completion. So when you type `sel` and then hit tab, psql will
auto-complete `SELECT` if upper-case was chosen as the option. 

In pgcli we have those explicit 'upper' and 'lower' options. In addition we
have an 'auto' option which is set as default. This will choose the casing
based on what the user has typed so far. So if you type `sel` and hit tab,
pgcli will suggest `select`, whereas if you type `SEL` and hit tab it will
suggest `SELECT`. I'm sure there is a way achieve this in psql, but this is the
option that pgcli ships with.

## Conclusion

We have plenty of other config options that ship with default values. You can
see a list of all the config values in `~/.config/pgcli/config` if you have
pgcli installed or you can checkout the
[pgclirc](https://github.com/dbcli/pgcli/blob/master/pgcli/pgclirc) file for a
quick view. [Let us know](https://github.com/dbcli/pgcli/issues) how we did.

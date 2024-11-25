# BabySQL

Try it [here](http://babysql.c1.blahaj.sg/)!

## First look

The index page tells us that routes `/login` and `/register` exist.

![](images/babysql/indexpage.png)

`/register` was broken by the time I got to the problem (chall was a common instance; see pictures later); `/login` informs us of the existence of `/adminLogin` :0

![](images/babysql/loginpage.png)

Submitting a blank username and password (by removing the required attributes on the inputs) let me get into `/user`, which is a page with a neat little search bar for SQLi :D

### Update 25/11/2024

`/register` is working again, and the blank username/password trick doesn't work anymore. You'll actually need to register now.

## SQLi yay!

### Entry point

Entering `'` (single quote) into the search bar gives a 500 (while `"` doesn’t), indicating that the string fields in the query can be closed with `'`.

(side note: after your first query you'll be redirected to `/product`, but that page is functionally identical to `/user`.)

### Number of columns

To find the number of columns in the table, we do `' UNION SELECT 1, 2, 3;--` etc, and we find that there are 3 columns – most likely ID, name, and price.

### What SQL DB is it?

I first tried the  different method of string concatenation (' UNION SELECT null, 'foo' || 'bar', 2;-- is the only one that works), but this is misleading, since the page doesn’t contain entries for SQLite.

Anyway, `' UNION SELECT 0, name, sql FROM sqlite_master;--` works and gives us a list of all tables :D

![](images/babysql/tables.png)

`' UNION SELECT 0, username, password FROM PRIV_USERS` gives us three users: Cisco, Leon and Rigby, and their password hashes.

Dropping the hashes into CrackStation cracks them easily; trying Cisco and his password ("test101") on `/adminLogin` gets us in!

Fun fact: there's a table `USERS_ZAHSHBSH` where you can see everyone’s attempts at SQLi to add a new user to the database :p

![]

Some of these may be mine.

Wow, rude >:(

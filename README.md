Instant IPython - Data Analysis
===============================

[IPython Notebook](http://ipython.org/notebook.html) packaged for Heroku + [pgcontents](https://github.com/quantopian/pgcontents) for persistent file storage. Also includes libraries to get you up and running with data analysis: pandas, statsmodels, scikit-learn, and [ipython-sql](https://github.com/mietek/instant-ipython) to connect to any RDBMS.


Installation
-----

### Heroku app creation

You *must* create a new Heroku app with the buildbpack ["heroku-buildpack-scipy"](https://github.com/thenovices/heroku-buildpack-scipy).

The dependencies have been re-arranged in requirements.txt in such a way that eliminates the ["Fortran" error](http://stackoverflow.com/questions/32341732/no-fortran-heroku-scipy-install) when building on Heroku.

```
$ git clone https://github.com/troyshu/instant-ipython-data-analysis.git
$ cd instant-ipython-data-analysis
$ heroku create --buildpack https://github.com/andrewychoi/heroku-buildpack-scipy
```

### Configure pgcontents

pgcontents allows persistent storage of ipython notebooks, files, etc. via a PostgreSQL database.

1) Decide on a database for pgcontents to use. If you already have a database attached (e.g. DATABASE_URL), you can use it, although it is recommended that you create a new database for pgcontents so no data is accidentally overwritten.

If you decded to create a new database for pgcontents:
```
$ heroku addons:create heroku-postgresql:hobby-dev
```

2) In "ipython_notebook_config.py", change DATABASE_URL to the appropriate environment variable if the database you decided to use for pgcontents in step 1 is different.

### Configure `IPYTHON_PASSWORD`

Set up the `IPYTHON_PASSWORD` environment variable on Heroku to password protect the notebook from unauthorized access.

```
$ heroku config:set IPYTHON_PASSWORD=yourpassword
```

### Deploy

```
$ git push heroku master
```

### Finish configuring pgcontents

Open up bash on your app server, and run "pgcontents init" to finish setting up pgcontents and running the necessary database migrations. When prompted, enter the url (e.g. postgresql://...) of the database you decided to use for pgcontents in step 1 of "Configure pgcontents". 
```
$ heroku run bash
$ ~$ pgcontents init
```

### You're done

Ensure the app is running on at least one dyno, and view the app:
```
$ heroku ps:scale web=1
$ heroku open
```


About
-----

Forked, added stats stuff and persistent storage by [Troy Shu](http://troyshu.com). Original 'instant-ipython' by [Miëtek Bak](https://mietek.io/).  Published under the [MIT X11 license](https://mietek.io/license/).

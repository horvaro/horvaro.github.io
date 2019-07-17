---
layout: post
title: Python - Headless Browser (Mechanize)
description: ""
modified: 2019-07-17
tags: [Python]
share: false
image:
  path: /images/abstract-2.jpg
  feature: abstract-2.jpg
---

Sometimes you need to automatically fill HTML forms in an automated pipeline (e.g. Installation of application, etc.).
One could use UI-Test automating frameworks like Selenium, but all of those need some kind of running Browser with which they can interact with.
This apprioach does not work on servers, where only a text-based interface is available.  

With the [mechanize](https://mechanize.readthedocs.io/en/latest/) library for python, one can script interactions with websites in an headless environment.
Please keep in mind, that mechanize can not run/parse javascript, which means that interactions based on javascript events won't be able to triggered.  

Simple example of formfilling:
```python
import mechanize
 
def select_form_by_id(formid):
    for form in br.forms():
        if form.attrs['id'] == formid:
            return form

db_host = "localhost"
db_user = "postgres"
db_pass = "password"
db_dbname = "postgres"
db_port = "5432"
 
url='http://localhost:8080/'
br = mechanize.Browser()
br.set_handle_robots(False)
br.open(url)
 
br.select_form("startform")
br.form.find_control('setupType').readonly = False
br.form['setupType'] = 'custom'
br.submit()

br.form = select_form_by_id('selectBundlePluginsForm')
res = br.submit()
 
br.select_form("setupdbtype")
br.form['dbConfigInfo.databaseType'] = ['postgresql']
br.form['dbConfigInfo.simple'] = ['true']
br.form['dbConfigInfo.hostname'] = db_host
br.form['dbConfigInfo.port'] = db_port
br.form['dbConfigInfo.databaseName'] = db_dbname
br.form['dbConfigInfo.userName'] = db_user
br.form['dbConfigInfo.password'] = db_pass
res = br.submit()
```

Mechanize selects the forms via `select_form`, which crawls the HTML page for a `<form>` with the `name` attribute equal to the given search term.
This limits us to only selecting `<form>` with a name attribute.  
Because of this limitation, I created a simple function at the beginning of the sample script, with which one can select `<form>` based on their `id` attribute.

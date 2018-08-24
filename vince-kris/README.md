# Project Name

**Author**: Your Name Goes Here
**Version**: 1.0.1 (increment the patch/fix version number up if you make more commits past your first submission)

## Overview
This project pulls in articles from a SQL database and renders them onto a page and stores them in a server which is created by node.js.

## Getting Started
1. Download the repo
2. Make sure Express, FS and PG are installed via the NPM package manager.
3. Run node server and watch the magic happen.

## Architecture
Uses HTML/CSS, JavaScript with jQuery, NPM (with Express, FS and PG), AJAX, and highlight.js.

## Change Log

Author: Vince Masten <vmasten@me.com>

Date:   Fri Aug 24 12:10:34 2018 -0700

    done except for the log update

Author: Vince Masten <vmasten@me.com>

Date:   Thu Aug 23 13:16:54 2018 -0700

    working on the readme

Author: Vince Masten <vmasten@me.com>

Date:   Thu Aug 23 12:52:48 2018 -0700

    update working

Author: Kris3579 <kristianesvelt@hotmail.com>

Date:   Thu Aug 23 11:10:58 2018 -0700

    comments half done

Author: Vince Masten <vmasten@me.com>

Date:   Thu Aug 23 10:23:12 2018 -0700

    initial commit

## Credits and Collaborations
Katy and Koko were both very helpful. We also looked a little at the SQL docs and at the cheat sheat provided with the lab.

## Additions for Lab 8 Part 2, 8.24.18

## Home (/) Route
#### Code Path (functions and locations)
See pictures/hometrace_1.jpg and pictures/hometrace_2.jpg. We wrote it all out on the whiteboard before realizing that we should have been writing here!
#### Data Path (what data do we have in each method, what format is it in, and what are we doing with it)
loadDB() kicks off the data path. It queries the kilovolt SQL database to see if the articles table does not exist, and creates one if it does not. If the table already exists, it pulls in the SQL data.

loadArticles() queries to see if articles exist, and inserts articles into the database from data/hackerIpsum.json if they are missing. In the process, it converts the JSON into SQL data.

fetchAll() makes an AJAX call back to server.js, which gets the SQL data created above and returns the rows of data to be formatted into Article objects. It then calls article.loadAll() with the results from the SQL query.

articleView.initIndexPage pushes the results back into JSON Article objects, preparing them to be rendered on index.html.

#### Images (whiteboard drawings) to support your conclusions.
See pictures/hometrace_pic.jpg

## New Article (/new-article) Route
#### Code Path (functions and locations)
app.get('new-article') kicks the process off, sending the file new.html when the user goes to localhost:3000/new-article.
Next, new.html runs the script articleView.initNewArticlePage, which is in articleView.js. This sets event listeners for articleView.create() and articleView.submit() so that as the new article is built, each field is pushed into a new object via create(), and when the user is finished writing the article, they hit the submit button, firing articleView.submit().

Once the article is submitted, articleView.submit() calls Article.insertRecord() which goes to article.js. Then, the AJAX call $.post('/articles') takes us back to server.js, and then to the SQL server where the new article is submitted into the database, calling back to server.js on completion to make sure everything went okay. Now there's a new article in the database.

#### Data Path (what data do we have in each method, what format is it in, and what are we doing with it)
app.get('new-article') sends a URL to the user.
initNewArticlePage() does not involve data, only jQuery.
articleView.create() uses jQuery and event listeners to create a new Article object.
articleView.submit() builds the full, complete Article object and fires article.insertRecord().
article.insertRecord() passes the complete date back to server.js via AJAX call.
$.post('/articles') takes the Article object and formats and posts it to the SQL server so that it becomes part of the database.

#### Images (whiteboard drawings) to support your conclusions.
See pictures/new.jpg

## Update Article from the console (via the instructions given in the 08-CRUD-testing.md file)
#### Code Path (functions and locations)
From the browser, the user types article.updateRecord(parameters), which calls article.js to kick off the update. From there, an AJAX call leads us back to server.js, where app.put('/articles/:id', (request, response) is run based on the id of the object's URL. 

This function then hits the Postgres server to find the article in need of the update, and SQL is passed in to do so based on the parameters originally provided by the user.

Once the update is complete, the Postgres server passes back status of how the update went via response.send or an error if something went awry.

#### Data Path (what data do we have in each method, what format is it in, and what are we doing with it)
Initially, the user calls the updateRecord method on the object that needs to be updated. The AJAX call parses the object into its constituent fields, and then passes the deconsructed object to server.js. server.js then uses app.put method to build a SQL query to update the data as desired. This query runs on the server, updates the data, and either gives back a status message that the update is complete or an error if something happened.

#### Images (whiteboard drawings) to support your conclusions
See pictures/update.jpg

## Delete Article from the console (via the instructions given in the 08-CRUD-testing.md file)
#### Code Path (functions and locations)
From the browser, the user types article.deleteRecord(parameters), which calls article.js to kick off the update. From there, an AJAX call leads us back to server.js, where app.delete('/articles/:id', (request, response) is run based on the id from the object's URL. 

This function then hits the Postgres server to find the article in need of deletion, and SQL is passed in to do so based on the parameters originally provided by the user.

Once the deletion is complete, the Postgres server passes back status of how the update went via response.send or an error if something went awry.

#### Data Path (what data do we have in each method, what format is it in, and what are we doing with it)
Initially, the user calls the deleteRecord method on the object that needs to be updated. The AJAX call finds the id of the article to be deleted based on the URL of the object passed in , and then passes that to server.js. server.js then uses app.delete method to build a SQL query to remove the record from the database. This query runs on the server, deletes the desired record, and either gives back a status message that the deletion is complete or an error if something didn't work.

#### Images (whiteboard drawings) to support your conclusions..
See pictures/delete.jpg
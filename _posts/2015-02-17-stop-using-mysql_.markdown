---
layout: post
title:  "Stop using mysql_*"
date:   2015-02-17 19:50:05
categories: therapy
---
* Table oh contents, please
{:toc}

# What you did

You used the [mysql_*](http://php.net/manual/en/ref.mysql.php) API.
  
# Why you (probably) did it

You learned it from some page on the internet.  It's possible that whomever wrote that page did so before you could
legally drive, or vote, or drink.  Or maybe they're just an idiot.

You probably know it's bad, but it works, and you just haven't had time to switch to something else.  So you're still
doing it.

# Why it's bad

You've probably heard most of this before.  In short, these functions are objectively terrible.  They're also 
deprecated (read: they're going away in upcoming versions of PHP), [for good reasons](http://stackoverflow.com/a/12860046/172588)

The biggest reason it's bad is because it makes it nearly impossible to avoid introducing [SQL Injection Vulnerabilities](http://en.wikipedia.org/wiki/SQL_injection)
into your code.  If you're writing code with SQL injection vulnerabilities in 2015, you're committing professional 
malpractice.

# What to do

Just. Stop. Using. It.

# How to get better

Lots of people will tell you to use [PDO](http://php.net/pdo).  They're not wrong.

But, I've noticed a lot of people have a hard time making the jump to PDO.  The interface can be frustrating at times,
and I've seen many people get frustrated and give up. 

So my advice now is to pick a user-land library that uses PDO internally, but provides a more user (programmer) friendly
API.  There are a bunch of them. 

These ones are pretty good:

* [aura/sql](https://github.com/auraphp/Aura.Sql)
* [doctrine/dbal](http://doctrine-dbal.readthedocs.org/en/latest/)

You should check out both libraries, and even explore others.  If you're pressed for time, then start with Aura/SQL. 
It is completely self-contained (no external dependencies), and is quite easy to understand.  Doctrine\DBAL is a much
 larger library, and is worth experimenting with.  But if you just want to stop using mysql_* *today*, I recommend you
 go with Aura.

# What's in it for you

Aside from helping you to not commit professional malpractice, your life actually gets better when you use one of these
libraries.  They take the safety of PDO, and then wrap some nice convenience methods around it.

Consider the following examples (using Aura's API):

#### Parameterized Queries

Now you don't need to remember to use `mysql_real_escape_string()`.  You just need to remember to **never** build your
query using string concatenation[^1].  Use nice parameters instead, they're safely escaped for you, and once you get
used to them, it's easier to write.

{% highlight php %}
<?php
$pdo = new \Aura\Sql\ExtendedPdo(...);

// step-by-step
$query = 'SELECT * FROM users WHERE firstname = :firstname';
$params = array('firstname' => 'alice');
$usersNamedAlice = $pdo->fetchAll($sql, $params);

// sometimes you feel like doing it in one line:

$usersNamedAlice = $pdo->fetchAll(
  'SELECT * FROM users WHERE firstname = :fn', 
  array( 'fn' => 'alice' )
);

{% endhighlight %}

#### Easy &lt;select&gt; options

Here's an example of a handy convenience function offered up by Aura\Sql:

{% highlight php %}
<?php
$pdo = new \Aura\Sql\ExtendedPdo(...);

$sql = 'SELECT abbreviation, name FROM us_states ORDER BY name ASC';
$stateOpts = $pdo->fetchPairs($sql);
?>
<select name="state">
  <?php foreach($states as $abbr=>$name): ?>
    <option value="<?= $abbr ?>"><?= $name ?></option>
  <?php endforeach; ?>
</select>
{% endhighlight %}

#### What about my existing projects?

It's easy to start doing things right.  Let's assume you have a file named `includes/database.php` that looks roughly
like this:

{% highlight php %}
<?php  //Connect to Database

mysql_connect("localhost","penguin_user","yummyfish!") or die("Unable to connect to MySQL");
mysql_select_db("penguin_db") or die(mysql_error()); 

{% endhighlight %}

You can start by doing something like:

{% highlight php %}
<?php  //Connect to Database

$dbUser = 'penguin_user';
$dbPass = 'yummyfish!';
$dbName = 'penguin_db';
$dbHost = 'localhost';

mysql_connect($dbHost, $dbUser, $dbPass) or die("Unable to connect to MySQL");
mysql_select_db($dbName) or die(mysql_error()); 

$dbConn = new \Aura\Sql\ExtendedPdo(
    "mysql:host={$dbHost};dbname={$dbName}",
    $dbUser,
    $dbPass);
    
{% endhighlight %}

Now when you'd be tempted to just call `mysql_query()`, you just use $dbConn instead:
 
{% highlight php %}
<?php

// in the global scope, you can just reference $dbConn directly
$startTime = (new \DateTime('-1 day'))->format('Y-m-d H:i:s');
$recentUsers = $dbConn->fetchAll(
    'SELECT * FROM User WHERE lastSeen > :startTime',
     compact('startTime')
);

// inside functions/methods, you'll need to import the global (or pass $dbConn in as a param:

function getRecentUsers($since)
{
    global $dbConn;
    $startTime = (new \DateTime($since))->format('Y-m-d H:i:s');
    $recentUsers = $dbConn->fetchAll(
        'SELECT * FROM User WHERE lastSeen > :startTime',
        compact('startTime')
    );    
}

{% endhighlight %}

As you run across old usages of `mysql_query()` and friends, you can replace them.  Or if you have a free hour you can
just `grep -R mysql_query *` from your project root, and receive a nice little to-do list!

In fact, to get comfortable, I suggest you do that right now.

# Homework

Take a project that is using mysql_* functions, and begin transitioning it to use Aura\Sql.

 * Figure out how to get Aura\Sql into your project (hint: [composer](https://getcomposer.org) is how.)
 * Set up a global $dbConn
 * Do a project-wide search for mysql_query (using grep, or the multi-file search tool of your choice), and convert at
   least 6 usages of mysql_query
   
That should get you comfortable enough to switch away from mysql_* immediately, and never look back.  It should also take
no more than an hour.

Have at it!

----

[^1]: 
    The only exception is if you're doing something tricky like using dynamic column or table names in your query. 
    In those cases, you need to take special care.  But that's the beyond the scope of this page.

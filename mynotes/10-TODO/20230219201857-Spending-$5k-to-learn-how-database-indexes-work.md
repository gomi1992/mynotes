---
title: Spending $5k to learn how database indexes work
url: https://www.briananglin.me/posts/spending-5k-to-learn-how-database-indexes-work/
clipped_at: 2023-02-19 20:18:57
category: default
tags: 
 - 无
---

# Spending $5k to learn how database indexes work

[2021-11-06](https://www.briananglin.me/posts/spending-5k-to-learn-how-database-indexes-work/)

### How we got here

_At [Superwall](https://superwall.me/) we’re building an SDK to help App Developers increase their revenue by showing the right offer to the right person at the right time. [Follow me on twitter for little tips and articles like this one.](https://twitter.com/BriansAngles)_

To deliver this service, we need to keep track of all of our customer’s users, the end users. To keep the numbers simple, assume we’re working with 5 companies who each have ~5k downloads a day, that’s roughly 25k downloads a day, or 750k users in a month. To keep track of all these users, we store them in a “users” table in a MySQL-like database. I say MySQL-like because we’re actually using PlanetScale’s serverless database offering. _After seeing a ton of the best GitHub engineers end up at PlanetScale and seeing the process GitHub went through to issue simple migrations, we chose to use their service._

### Our schema

To keep track of users we have two tables, `ApplicationUser` and `ApplicationUserAlias` which look something like this. `ApplicationUserAlias` has a many-to-one relationship with `ApplicationUser`. We need both of these tables because we assign a random id for a user to track events before our customers tell us who that user is. So I might be known as `$SuperwallAlias:...`, and `custom_id190390930`.

```sql
CREATE TABLE `ApplicationUser` (
  `id` int NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (`id`),
) ENGINE=InnoDB;

CREATE TABLE `ApplicationUserAlias` (
  `id` int NOT NULL AUTO_INCREMENT,
  -- Some identifier by which we know the user, a lot of apps use uuids
  `vendorId` varchar(191) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  -- The "foreign key" back to ApplicationUser 
  `applicationUserId` int NOT NULL,
  PRIMARY KEY (`id`),
) ENGINE=InnoDB;
```

### No foreign keys

One quirk of PlantScale, or more specifically [Vitess](https://vitess.io/) is the [lack of support for foreign keys](https://vitess.io/blog/2021-06-15-online-ddl-why-no-fk/). Foreign Keys are fundamentally incompatible with “online schema migrations”, a fancy way of saying changing your database’s schema without locking anything. No big deal, there are [plenty of work arounds](https://github.com/planetscale/beta/discussions/74).

However, one consequence of not having foreign keys is no automatic index creation. “MySQL requires that foreign key columns be indexed; if you create a table with a foreign key constraint but no index on a given column, an index is created.” [1](#fn:1) **Without knowing it, I had been relying on MySQL’s automatic index creation based on foreign keys to make my queries faster**. It’s common to look up all children of a parent. In our case all aliases of a user.

Too look up all the aliases of a user, we were running this query. With the automatic foreign key indexes this query would have been “cheap” because we would have simply looked up by `applicationUserId`. Without them, this is a full table scan 😬

```sql
SELECT id, vendorId FROM ApplicationUserAlias WHERE applicationUserId = x LIMIT 100;
```

### “Rows read”

_With most database providers, this mistake would have simply lead to slow queries. However, with PlanetScale’s infrastructure we didn’t notice… until we got the bill._

PlanetScale’s [pricing model](https://planetscale.com/pricing) is based on “rows read”, which I translated in my head to “rows returned”. In that previous example, returning the maximum of 100 `ApplicationUserAlias` rows, at $1.5 per 10 million rows read, we could easily afford 100 rows returned, or so I thought.

Looking closely at the pricing page the definition of a read is **“Retrieving or _inspecting_ rows during a query or mutation of any kind to your PlanetScale databases.”**[2](#fn:2) The operative word is **“inspecting”**, so our simple query to look up 100 aliases was actually inspecting the entire users table which is already past 1 million rows in our first month of service.

> Every request making that query cost us $0.15

Looking back at our data, we were making ~280 requests per hour to that endpoint, which comes out to just about $1k per day which matches the billing in PlanetScale.

### The fix

The fix was actually really simple and just required that we create the index which MySQL would have created for us.

```sql
CREATE INDEX `ApplicationUserAlias.applicationUserId_index`
  ON `ApplicationUserAlias`(`applicationUserId`);
```

Bet you can’t spot when we added the index 😉

![planet-scale](assets/1676809137-04636c3e96152da9162d57c13a4e8c07.png)

To figure this out I made use of [`EXPLAIN ANALYZE`](https://www.percona.com/blog/2019/10/28/using-explain-analyze-in-mysql-8/) but that’s for another post.

### Getting off the hook, for now

The fine folks at PlanetScale were very willing to let us off the hook this time and have credited our account to get us back down to the much more reasonable pricing of ~$150/month where we were previously.

I’m genuinely super excited about what they are working on and think the choice for us still makes sense even with the few changes we’ve had to adapt to.

* * *

1.  [https://dev.mysql.com/doc/refman/8.0/en/constraint-foreign-key.html](https://dev.mysql.com/doc/refman/8.0/en/constraint-foreign-key.html) [\[return\]](#fnref:1)
2.  [https://docs.planetscale.com/concepts/billing](https://docs.planetscale.com/concepts/billing) [\[return\]](#fnref:2)

### Contents

*   *   *   [How we got here](#how-we-got-here)
        *   [Our schema](#our-schema)
        *   [No foreign keys](#no-foreign-keys)
        *   [“Rows read”](#rows-read)
        *   [The fix](#the-fix)
        *   [Getting off the hook, for now](#getting-off-the-hook-for-now)

[

When not to learn anything \>

](https://www.briananglin.me/posts/when-not-to-learn-anything/)
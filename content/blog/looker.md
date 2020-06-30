---
title: "Looker might slow you down"
date: 2020-06-29T21:33:47-07:00
draft: true
---

At the beginning of this year the data analysts and engineers at my company undertook two large migration feats. They moved our data warehouse into Snowflake and secondly, they moved our analytics tooling away from Periscope and into Looker.

Looker is heralded as a way for non-SQL savvy users to self serve and build their own data reports and tables without leaning on a Data Analyst who knows SQL to do it for them.

On one hand Looker has delivered on that promise; people in the business are increasingly getting used to building their own Dashboards. On the other hand, I've noticed my Data Analysts are as busy as they've ever been, if not more. What gives?

## Looker creates busywork for your Analysts

Looker is not a zero-config plug and play service. Its not the case that you can just hook it up to your data warehouse and your tables are auto discovered and configured automatically.

In fact, what I've noticed is that Data Analysts in Looker shops are spending a solid chunk of their time writing out LookML; a Domain-specific-language (read Vendor Lockin) that Looker requires Analysts to use to redefine the schema of the underlying data models in a format that Looker understands. These schemas are called views and you need at least one view for each table in your database (a huge task if you are an enterprise company with hundreds of tables). In addition to defining views, explores (another custom Looker concept) have to be defined by the Analyst before an end user can start building off the data.

## Looker hinders rapidly iterating product development

Looker is not a great place for dashboards of experimental work or rapidly evolving datasets and isn't great for getting insight into new features that were built and deployed within the past few days or weeks.

Looker is best for building dashboards on top of your more mature corners of the business. The idea is that your data analyst does an initial up front concerted effort to diligently sort, organize and label your data schema. For them to do this, it implies your data schema should be relatively stable. This might suit some of the more traditional, stable parts of your company like tracking User Sign Ups, or Orders.

My current team's culture is to iterate fast, to try something, measure the results, adopt it and move on if necessary. Ideally we'd like to add a new production table, start amassing data in it, and then quickly build dashboards (that might not last longer than a week or two) to measure the result or inspect the contents. Every time we start collecting a new piece of data and want to report it in Looker for our business partners to play with, we have to do grunt work to get it to appear in Snowflake, and once in Snowflake we have to do grunt work to get it readily available in Looker. Admittedly this could be mitigated if we were flush with Data Analysts embedded in our team who could immediately start defining the LookML schema the moment each migration lands but the reality is that they are a shared resource and you have to get in line and wait your turn if you need something to appear in Looker.

## In Summary

Looker has value for sure, but it doesn't mean you can hire fewer analysts or engineers, if anything you should hire more, and if you're a fast moving business with an ever-evolving product, your team's ability to make data driven decisions is still constrained by the capacity of your Data Analytics team and if you consider yourselves rapid iterators making data driven decisions you may find traditional SQL-based analytics tools are better suited for the job.

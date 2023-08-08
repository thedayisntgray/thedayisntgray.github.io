---
layout: post
title: Life after cron job
author: "Landon Gray"
# categories: journal
tags: [data-engineering]
image: cron-clock.jpg
---

One of my new found interests besides ML stuff is Data Engineering.

One interesting thing I hope to explore more is the way various ways organizations handle scheduling jobs.

Obviously cron is pretty solid initial step, but it doesn’t scale well at all. The frustration cron at scale is that it always runs at a specified time. If a job  runs 1 minute longer than expected it will cause all subsequent dependent jobs to fail (since they are dependent on the task to complete before they run)

Unfortunately to fix such failures costs developer time and effort. Not to mention it could cause un wanted downstream issues in dev, stage and prod environments... OH NO!

So how do we manage these dependencies?

Well to fix these issues we need to better understand the dependencies these jobs have with one another and clearly map them out. 

Using [DAGs](https://en.wikipedia.org/wiki/Directed_acyclic_graph) ( Directed Acyclic Graphs) we can clearly map these dependencies.

![Directed Acyclic Graph example](/assets/img/example-dag.png)

*Credit Wikipedia*




Once we are finished, we can then elicit the help of a **Workflow Scheduling Framework** to run the tasks in order as part of a single workflow.

In addition to preventing failures do to long running tasks, the set of task as part of the workflow will run faster since they can run as soon as the dependent task finishes instead of waiting until it's scheduled time.

3 industry standard workflow management tools that can help us are:
[Apache Airflow](https://airflow.apache.org/) created by Airbnb
[Oozie](https://oozie.apache.org/) (also apache)
[Luigi](https://github.com/spotify/luigi) created by Spotify

This is by no means an exhausive list, there are no doubt many more.

But this should be enough for you to get started. If you happen to have any questions come up feel free to email me 😃

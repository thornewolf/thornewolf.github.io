---
title: Job Scheduling Library
---

Idea: job scheduling small python library that just lets
you decorate a function and register it with a
job queue. This can be done based on just periodic.

```python
queue = pyjob.Scheduler(config=...)

@queue.periodic(minutes=n)
def func_n():
    # This will be invoked with no arguments every
    # N minutes

@queue.from_table(table_name=..., type=...)
def func(...):
    # automatically grabs one row from table matching type
    # then run this function on it
```



Open questions:
- Should table schema be flexible or always
  just id, start, stop, payload
- What if we want multiple daemons/stream processors
  from a table row entry; This brings up consistency
  and tight execution questions; Global lock can
  solve this?
  
- I don't want a hard dependency on SQL Alchemy.
  How to abstract my ORM w/o just bringing another
  ORM?
  - Register a connector that exposes 'atomic_get_job'
    maybe?

- What solutions exist already?
    - https://schedule.readthedocs.io/en/stable/
    - https://pypi.org/project/python-crontab/
      - kinda different, looks legitimately like cron.
    - https://www.starlette.io/background/
---
title: Fix lumen routes in Apache
date: 2016-09-29 17:00:12
tags:
- lumen 
- apache 
- routes
---

By default, apache doesn't recognize lumen routes, this is because on public\index.php the "run" method initializes every request on null, to fix that, simply add this change: 

```php public\index.php
    $app->run($app->make('request'));
```
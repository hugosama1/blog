---
title: Change Lumen error handling to return json
date: 2016-09-30 17:22:52
tags:
- lumen 
- error 
- laravel
- json
---

When we make an ajax request to a REST endpoint in lumen, by default we get a 404 page saying the resource does not exist. That's an unparsable response, and we usually want to get more insight as to what happened.

In order for us to still get the 404 page on the browser BUT! get a json on an ajax call, we can simply modify our App\Http\Exceptions\Handler.php in the render method like so: 

```php App\Http\Exceptions\Handler.php

    public function render($request, Exception $e)
    {
        if ($e instanceof ModelNotFoundException) {
            if ($request->ajax()) {
                return response()->json([
                    'message' => 'Record not found',
                ], 404);
            }
        }
        return parent::render($request, $e);
    }
```

This simple modification will give us exactly what we want : a 404 response on both browser and ajax calls, but on the ajax side it will return a nice little json : 
```javascript
{
  "message": "Record not found"
}
```

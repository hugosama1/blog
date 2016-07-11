---
title: How to configure Laravel Excel in Lumen
date: 2016-07-11 14:47:11
tags:
---
One of the best libraries to handle spreadsheet documents is {% link Laravel-Excel http://www.maatwebsite.nl/laravel-excel/docs %} , but it's laravel API makes it a burden to use on lumen due to some limitations on the laravel's little brother.

But everything can be achieved with enough courage. I'm gonna show you the configurations necesary for this to work.

First require the following package in your composer.json file : 

```
"maatwebsite/excel": "~2.1.0"
```

After that run the well-known composer update: 
```
composer update
```

Now that you have the library installed, the last step for configuration is adding the following lines to your bootstrap/App.php file:

```PHP App.php
$app->register('Maatwebsite\Excel\ExcelServiceProvider');
class_alias('Maatwebsite\Excel\Facades\Excel', 'Excel');
class_alias('Illuminate\Support\Facades\Response', 'Response');
class_alias('Illuminate\Support\Facades\Config', 'Config');
```

Now you can run some example of the library itself! let me give you one directly on Routes.php: 

```PHP Routes.php
$app->get('/', function () use ($app) {
    Excel::create('Laravel Excel', function($excel) {
        $excel->sheet('Excel sheet', function($sheet) {

            $sheet->setOrientation('landscape');

        });
    })->export('xls');
});
```
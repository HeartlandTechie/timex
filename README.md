![timex-logo](https://user-images.githubusercontent.com/2136612/202689778-eb013a03-b0fa-4c0e-941c-7d999c09fd6f.jpeg)


## TIMEX - calendar plugin for filament

[![Latest Version on Packagist](https://img.shields.io/packagist/v/buildix/timex.svg?style=flat-square)](https://packagist.org/packages/buildix/timex)
[![Total Downloads](https://img.shields.io/packagist/dt/buildix/timex.svg?style=flat-square)](https://packagist.org/packages/buildix/timex)

![main](https://user-images.githubusercontent.com/2136612/203250400-8ba51fc4-8d37-49d8-a125-6ab294c01d94.png)

## Installation

You can install the package via composer:

```bash
composer require buildix/timex
```

You can publish and run the migrations with:

```bash
php artisan vendor:publish --tag="timex-migrations"
php artisan migrate
```

You can publish the config file with:

```bash
php artisan vendor:publish --tag="timex-config"
```

This is the contents of the published config file:

```php
return [
    /*
    |--------------------------------------------------------------------------
    | TIMEX Icon set
    |--------------------------------------------------------------------------
    |
    | Don't change that prefix, otherwise icon set will not work.
    |
    */

    'prefix' => 'timex',

    /*
    |--------------------------------------------------------------------------
    | TIMEX Mini widget
    |--------------------------------------------------------------------------
    | * - Not available on the release. Subscribe for future updates
    | You can disable or enable individually widgets or entirely the whole view.
    |
    */

    'mini' => [
        'isMiniCalendarEnabled' => true,
        'isDayViewHidden' => false,
        'isNextMeetingViewHidden' => false,
    ],

    /*
    |--------------------------------------------------------------------------
    | TIMEX Calendar start & end of week
    |--------------------------------------------------------------------------
    |
    | Change according to your locale.
    |
    */

    'week' => [
        'start' => \Carbon\Carbon::MONDAY,
        'end' =>  \Carbon\Carbon::SUNDAY
    ],

    /*
    |--------------------------------------------------------------------------
    | TIMEX Resources & Pages
    |--------------------------------------------------------------------------
    |
    | By default TIMEX out of box will work, just make sure you make migration.
    | But you can also make your own Model and Filament resource and update config accordingly
    |
    */

    'pages' => [
        'timex' => \Buildix\Timex\Pages\Timex::class,
        'slug' => 'timex',
        'group' => 'timex',
        'shouldRegisterNavigation' => true,
        'icon' => [
            'static' => true,
            'timex' => 'timex-timex',
            'day' => 'timex-day-'.Carbon::today()->day
        ],
        'label' => [
            'navigation' => Carbon::today()->isoFormat('dddd, D MMM'),
            'breadcrumbs' => Carbon::today()->isoFormat('dddd, D MMM'),
            'title' => Carbon::today()->isoFormat('dddd, D MMM'),
        ],
        'buttons' => [
            'today' => \Carbon\Carbon::today()->format('d M'),
            'icons' => [
                'previousMonth' => 'heroicon-o-chevron-left',
                'nextMonth' => 'heroicon-o-chevron-right',
                'createEvent' => 'heroicon-o-plus'
            ],
        ],
    ],

    'resources' => [
        'event' => \Buildix\Timex\Resources\EventResource::class,
        'icon' => 'heroicon-o-calendar',
        'slug' => 'timex-events',
        'shouldRegisterNavigation' => true,
    ],
    'models' => [
        'event' => \Buildix\Timex\Models\Event::class,
        'label' => 'Event',
        'pluralLabel' => 'Events'
    ],

    /*
    |--------------------------------------------------------------------------
    | TIMEX Event categories
    |--------------------------------------------------------------------------
    |
    | Categories names are used to define color.
    | Each represents default tailwind colors.
    | You may change as you wish, just make sure your color have -500 / -600 and etc variants
    |
    */

    'categories' => [
            'labels' => [
                'primary' => 'Primary category',
                'secondary' => 'Secondary category',
                'danger' => 'Danger category',
                'success' => 'Success category',
            ],
            'icons' => [
                'primary' => 'heroicon-o-clipboard',
                'secondary' => 'heroicon-o-bookmark',
                'danger' => 'heroicon-o-flag',
                'success' => 'heroicon-o-badge-check',
            ],
            'colors' => [
                'primary' => 'primary',
                'secondary' => 'secondary',
                'danger' => 'danger',
                'success' => 'success',
            ],
    ],
];
```

## Usage

After your fresh installation, TIMEX calendar is working out of the box (make sure to run migration) and start managing your time.
![main-1](https://user-images.githubusercontent.com/2136612/203252427-75d182f5-e60f-4657-bf28-3635bba24045.png)
![main-2](https://user-images.githubusercontent.com/2136612/203252452-dd0f8358-8815-4b9a-8968-d9d00aa7894c.png)

### EventItem

You can use out of the box method to supply your calendar with your events from the Eloquent model, or via overriding function

```php
getEvents(): array {
    return [
        \Buildix\Timex\Events\EventItem::make('id')
        ->subject('Your subject goes here')
        ->body('Body - long text of your event')
        ->color('primary') // secondary / danger / success
        ->category('primary') // secondary / danger / success
        ->start(today())
        ->end(today())
        ->startTime(now()),
        \Buildix\Timex\Events\EventItem::make('id')
        //
    ];
}
```

### Event resource

By default TiMEX register `EventResource` to your navigation panel, if you would like to disable it, simply set `shouldRegisterNavigation` to `false` in TiMEX config:
Also, you can change slug, icon, and the Filament resource itself

```php
'resources' => [
    //
    'shouldRegisterNavigation' => false,
],
```

### TiMEX Page

Package comes with pre-installed Filament page with all necessary configurations. 
If you need to change label naming, slug, navigation group, etc, go ahead to TiMEX config. 


### Dynamic icon set

TiMEX comes with two custom icons:
1. First one is static, represents TiMEX branding
2. The other is totally custom dynamic icon set for each day

You may change navigation icon from static to dynamic, simply changing TiMEX config `timex.icon.static` to `false`:

```php
'icon' => [
    'static' => false,
    //
],
```

### Start & End of week

You may change how the calendar renders your week according to your locale. In order to make week start on Sunday, change TiMEX config accordingly:

```php
'week' => [
    'start' => \Carbon\Carbon::MONDAY,
    'end' =>  \Carbon\Carbon::SUNDAY
],
```

### Day names in Calendar view

You may change the names of weekdays on your calendar by changing TiMEX config `dayName` to 3 options:

```php
'dayName' => 'minDayName', // Mo .. Su
//
'dayName' => 'shortDayName', // Mon ... Sun
//
'dayName' => 'dayName', // Monday ... Sunday
```

To change Carbon locale, make sure to update your `app.php` locale settings.

## Credits

- [mikrosmile](https://github.com/mikrosmile)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

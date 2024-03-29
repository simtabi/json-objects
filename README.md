# JSON Objects

[![Author][ico-author]][link-author]
[![PHP Version][ico-php]][link-php]
[![Build Status][ico-actions]][link-actions]
[![Coverage Status][ico-scrutinizer]][link-scrutinizer]
[![Quality Score][ico-code-quality]][link-code-quality]
[![Latest Version][ico-version]][link-packagist]
[![Software License][ico-license]](LICENSE.md)
[![PSR-7][ico-psr7]][link-psr7]
[![PSR-12][ico-psr12]][link-psr12]
[![Total Downloads][ico-downloads]][link-downloads]

This package extracts JSON objects from large JSON sources like files, endpoints and streams while saving memory. It parses heavy JSONs by using [JsonStreamingParser][link-jsonstreamingparser] and provides an easy API to declare what objects to extract and process.

## Install

Via Composer

``` bash
composer require simtabi/json-objects
```

## Usage

Simply pass the JSON source (files, endpoints or streams) and optionally the key where objects are contained to create a new instance of `JsonObjects`. You can also call the factory method `from()`:

``` php
$source = 'https://jsonplaceholder.typicode.com/users';

// Create a new instance specifying the JSON source to extract objects from
new JsonObjects($source);
// or
JsonObjects::from($source);

// Create a new instance specifying the JSON source and the key to extract objects from
new JsonObjects($source, 'address.geo');
// or
JsonObjects::from($source, 'address.geo');
```

When providing a key to extract objects from, you can use the dot notation to indicate nested sections of a JSON. For example `nested.*.key` extracts all the objects in the property `key` of every object contained in `nested`.

Under the hood `JsonObjects` supports PSR-7, hence any implementation of [MessageInterface][link-message-interface] or [StreamInterface][link-stream-interface] is a valid source. This makes interactions with other packages supporting PSR-7 (e.g. Guzzle) even more convenient:

``` php
$response = $guzzle->get('https://jsonplaceholder.typicode.com/users');

// Create a new instance by passing an implementation of MessageInterface
JsonObjects::from($response);

// Create a new instance by passing an implementation of StreamInterface
JsonObjects::from($response->getBody());
```

Finally you can decide whether to extract and process objects one by one or in chunks. The memory will be allocated to read only these objects instead of the whole JSON document:

``` php
// Extract and process one object at a time from the given JSON source
JsonObjects::from($source)->each(function (array $object) {
    // Process one object
});

// Extract and process a chunk of objects at a time from the given JSON source
JsonObjects::from($source)->chunk(100, function (array $objects) {
    // Process 100 objects
});
```

## Change log

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Testing

``` bash
$ composer test
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) and [CODE_OF_CONDUCT](CODE_OF_CONDUCT.md) for details.

## Security

If you discover any security related issues, please email andrea.marco.sartori@gmail.com instead of using the issue tracker.

## Credits

- [Andrea Marco Sartori][link-author]
- [JsonStreamingParser][link-jsonstreamingparser]

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

[ico-author]: https://img.shields.io/static/v1?label=author&message=cerbero90&color=50ABF1&logo=twitter&style=flat-square
[ico-php]: https://img.shields.io/packagist/php-v/cerbero/json-objects?color=%234F5B93&logo=php&style=flat-square
[ico-version]: https://img.shields.io/packagist/v/cerbero/json-objects.svg?label=version&style=flat-square
[ico-actions]: https://img.shields.io/github/workflow/status/cerbero90/json-objects/build?style=flat-square&logo=github
[ico-license]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square
[ico-psr7]: https://img.shields.io/static/v1?label=compliance&message=PSR-7&color=blue&style=flat-square
[ico-psr12]: https://img.shields.io/static/v1?label=compliance&message=PSR-12&color=blue&style=flat-square
[ico-scrutinizer]: https://img.shields.io/scrutinizer/coverage/g/cerbero90/json-objects.svg?style=flat-square&logo=scrutinizer
[ico-code-quality]: https://img.shields.io/scrutinizer/g/cerbero90/json-objects.svg?style=flat-square&logo=scrutinizer
[ico-downloads]: https://img.shields.io/packagist/dt/cerbero/json-objects.svg?style=flat-square

[link-author]: https://twitter.com/cerbero90
[link-php]: https://www.php.net
[link-packagist]: https://packagist.org/packages/cerbero/json-objects
[link-actions]: https://github.com/cerbero90/json-objects/actions?query=workflow%3Abuild
[link-psr7]: https://www.php-fig.org/psr/psr-7/
[link-psr12]: https://www.php-fig.org/psr/psr-12/
[link-scrutinizer]: https://scrutinizer-ci.com/g/cerbero90/json-objects/code-structure
[link-code-quality]: https://scrutinizer-ci.com/g/cerbero90/json-objects
[link-downloads]: https://packagist.org/packages/cerbero/json-objects
[link-jsonstreamingparser]: https://github.com/salsify/jsonstreamingparser
[link-message-interface]: https://github.com/php-fig/http-message/blob/master/src/MessageInterface.php
[link-stream-interface]: https://github.com/php-fig/http-message/blob/master/src/StreamInterface.php
[link-contributors]: ../../contributors

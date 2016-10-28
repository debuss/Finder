# Finder

This Finder class finds files and directories via a set of rules, the same way as [the Finder Component from Symfony](https://symfony.com/doc/current/components/finder.html) but in a much simpler implementation.

The main purpose was to create an alternative to the **Symfony Finder** because I find it too "heavy" for what it is suppose to do.
Many files, folder, comparators, classes and exceptions. **That's too much...**

As my Finder is a simpler version of the Symfony one, you won't find some features :
* Custom exceptions _(only Exception classes from SPL)_
* Some methods like _depth()_, ...
* Custom SplFileInfo, therefore these methods are not available :
    * getRelativePath()
    * getRelativePathname()
    * getContents()

## Usage

#### List files and folders in a directory

```php
use KeepItSimple\FileSystem\Finder;

foreach (Finder::create()->in(__DIR__) as $file) {
    // Magic here
    // $file is an instance of SplFileInfo
}
```

#### List only files or only folders in a directory

```php
use KeepItSimple\FileSystem\Finder;

foreach (Finder::create()->files()->in(__DIR__) as $file) {
    // Magic here
    // $file is an instance of SplFileInfo
}

foreach (Finder::create()->directories()->in(__DIR__) as $file) {
    // Magic here
    // $file is an instance of SplFileInfo
}
```

#### Select a directory

The only compulsory parameter is a path.  
You may use the _in()_ method with glob() like path.  
The _in()_ method can be chained to include many paths or an array of path can be provided.

```php
use KeepItSimple\FileSystem\Finder;

foreach (Finder::create()->in(__DIR__.'/*/*/test') as $file) {
    // Magic here
    // $file is an instance of SplFileInfo
}

foreach (Finder::create()->in(__DIR__)->in('/home/alex/docs') as $file) {
    // Magic here
    // $file is an instance of SplFileInfo
}

foreach (Finder::create()->in([__DIR__, '/home/alex/docs']) as $file) {
    // Magic here
    // $file is an instance of SplFileInfo
}
```

#### Sorting

Some sorting method are already provided but you can also supply your own.

```php
use KeepItSimple\FileSystem\Finder;

foreach (Finder::create()->in(__DIR__)->sortByName() as $file) {
    // Magic here
    // $file is an instance of SplFileInfo
}

$finder = Finder::create()->in(__DIR__)->sort(function (SplFileInfo $a, SplFileInfo $b) {
    // Equivalent to the method sortByName()
    return strcmp($a->getFilename(), $b->getFilename())
});

foreach ($finder as $file) {
    // Magic here
    // $file is an instance of SplFileInfo
}
```

#### Filtering

Some filtering method are already provided but you can also supply your own.

```php
use KeepItSimple\FileSystem\Finder;

// The name() method accepts globs, strings, or regexes
foreach (Finder::create()->in(__DIR__)->name('*.php') as $file) {
    // Magic here
    // $file is an instance of SplFileInfo
}

$finder = Finder::create()->in(__DIR__)->filter(function (SplFileInfo $current) {
    return $current->getBasename() !== 'index.php';
});

foreach ($finder as $file) {
    // Magic here
    // $file is an instance of SplFileInfo
}
```

## Complete documentation

The complete documentation is provided in the documentation folder.  
Generated with [APIGen](http://www.apigen.org/).

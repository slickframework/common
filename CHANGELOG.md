# Changelog

All Notable changes to `Slick` will be documented in this file

## 1.2.0 - YYYY-MM-DD

### Added
- Annotations now make use of class namespace and use statements to
  be imported
- `Slick\Common\Inspector` now manages a list of class metadata for better
  performance. The static `Slick\Common\Inspector::forClass()` method should
  now be used to get class information and annotations

### Removed
- Codeception support
- `Slick\Common\Inspector::__construct()` cannot be used anymore
- `Slick\Utility` module was removed and its now on `Slick\Common\Utils` namespace
- `Slick\Common\SingletonInterface` and `Slick\Common\BaseSingleton`. You can use the
  `Slick\Common\BaseMethods` trait in any class you want.
  
## 1.1.0 - 2015-04-14

### Added
- Adding travis support
- New `Database` component with `Sql` factory and more robust interfaces
- Schema loader factory
- Rewrite `MVC` component to work with new `ORM` module
- Filter event on `MVC` application
- Whoops as a error handler.

### Fixed
- Multiple bugs `ORM` component
- Connection validation on memcached driver.
- Bug on router extension attribution.
- Bug on delete session keys.
- Bug on multiple relations when loading join values.
- Model name parsing on generate commands.

## 1.0.5 - 2014-10-06
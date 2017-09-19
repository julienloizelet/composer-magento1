# Composer for Magento 1
----------------------

* Set the Magento version you want in the `composer.json` file.
Example : `"magento/core" : "^1.9.3.4"`

* Just run `composer install`


## Remarks
* Magento decided to disallow symlinks in the patch SUPEE-9767. When working with composer and modman, symlinks are a good system to dynamically add modules.
That's why I added the `Mbiz_IWantMySymlinksBack` extension. You will have to allow symlinks in `System->Config->Advanced->Developer->Template Settings->Allow Symlinks`.
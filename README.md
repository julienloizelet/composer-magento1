# Composer for Magento 1
----------------------

* Set the Magento version you want in the `composer.json` file.
Example : `"magento/core" : "^1.9.3.4"`

* Just run `composer install`

## How to use

* All your extension and third part extension should be included with composer (by adding it in the `composer.json` file.).

* When you need to add some files that are not a part of an extension (typically a `robots.txt` or a theme template,css, etc override.)
you should use the `src` folder.
In order to link the `src` folder to the `htdocs` folder, you will need to run the following command :
`modman link /path/to/the/folder/src` in your root directory. The `src` folder should contain the appropriate `modman` file.
Then run `modman deploy-all` in your root directory.


## Remarks
* Magento decided to disallow symlinks in the patch SUPEE-9767. When working with composer and modman, symlinks are a good system to dynamically add modules.
That's why I added the `Mbiz_IWantMySymlinksBack` extension. You will have to allow symlinks in `System->Config->Advanced->Developer->Template Settings->Allow Symlinks`.

* In order to create the `modman` file in the `src` folder, I suggest you to use the script [`generate-modman`](https://github.com/mhauri/generate-modman).
`cd src && ../bin/generate-modman --include-others --include-others-files --include-others-files-mindepth=0` 
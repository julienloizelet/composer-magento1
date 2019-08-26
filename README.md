
# Composer for Magento 1 Project
> Use it to manage a new Magento 1 project with composer (and optionally modman).
## Installing / Getting started
1. Clone this repo : `git clone https://github.com/julienloizelet/composer-magento1.git`.

1. Open the `composer.json` file and set the Magento version you want.

   Example : `"openmage/magento-lts" : "^1.9.4"` or `"magento/core" : "^1.9.3.4"`

1. Run `composer install`.

## Understanding
It will create several folders :

1. `vendor` where composer packages are installed.
1. `bin` where you will find some scripts.
1. `htdocs` where you will find all necessary sources required for a Magento 1 project.

Thus, here is an example of a Virtual Host configuration for Apache 2 :
```
<VirtualHost *:80>
     ServerName magento.local
     ServerAdmin youremail@yourdomain.com
     DocumentRoot /path/to/the/clone/of/this/repo/htdocs
     <Directory />
             Options FollowSymLinks
             AllowOverride None
     </Directory>
     ErrorLog ${APACHE_LOG_DIR}/error.log
     LogLevel warn
     <Directory /path/to/the/clone/of/this/repo/htdocs>
             Options Indexes FollowSymLinks MultiViews
             AllowOverride All
             Allow from all
             Require all granted
     </Directory>
 </VirtualHost>
```

The key point is that sources are linked to `htdocs` using [`modman`](https://github.com/colinmollenhour/modman).
That is one of the purpose of packages `magento-hackathon/magento-composer-installer`, `aydin-hassan/magento-core-composer-installer` and of course
`colinmollenhour/modman`.

## How to use / Developing
1. All your extensions and third part extension should be included with composer (by adding it in the `composer.json` file.).

For example, if you want to use the awesome extension [Okaeli_ComingSoon](https://github.com/julienloizelet/magento-comingsoon),
add the following lines in the `repositories` part of the `composer.json` :

```json
{
  "type": "vcs",
  "url": "https://github.com/julienloizelet/magento-comingsoon"
}
```
and add the line `"okaeli/magento-comingsoon":"dev-master"` in the `require` part.


2. When you need to add some extra files that are not a part of an extension (typically a `robots.txt` or an override of a theme template,css, etc), you should follow one of the following options:

    i. Use Modman

    You should use the `src` folder.
    In order to link the `src` folder to the `htdocs` folder, you will need to run the following command :
    `modman link /path/to/the/folder/src` in your root directory. The `src` folder should contain the appropriate `modman` file.
    Then run `modman deploy-all` in your root directory.

    In order to create the `modman` file in the `src` folder, I suggest you to use the script [`generate-modman`](https://github.com/mhauri/generate-modman).

    `cd src && ../bin/generate-modman --include-others --include-others-files --include-others-files-mindepth=0`

    ii. Use Git

    Add a `vcs` repositories in your `composer.json` file.

    ```json
    {
      "type": "vcs",
      "url": "https://github.com/yourgithubuser/yourepoforextrafiles"
    }
    ```

    and add the line `"yourvendorname/yourepoforextrafiles":"dev-master"` in the `require` part.


## Remarks
* Magento decided to disallow symlinks in the patch SUPEE-9767. When working with composer and modman, symlinks are a good system to dynamically add extensions.
That's why I added the `Mbiz_IWantMySymlinksBack` extension. You will have to allow symlinks in `System->Config->Advanced->Developer->Template Settings->Allow Symlinks`.
If you do not want to use symlink, you should set the `magento-deploystrategy-dev` to `copy` and use `modman deploy-all --copy` when necessary.

* I added some extensions in the `require-dev` part of the `composer.json`. These are for the purpose of debugging, testing or code reviewing.
Just remove these lines if unnecessary.

## Contributing
If you'd like to contribute, please fork the repository and use a feature
branch. Pull requests are warmly welcome.
If you found any issue, please go [there](https://github.com/julienloizelet/composer-magento1/issues).

## Licensing
The code in this project is licensed under MIT license.
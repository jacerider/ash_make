# Ash Make Drupal Installation
This Document describes all the necessary steps to install Drupal using Ash_make.

## Drush
Drush is a command line shell and scripting interface for Drupal, a veritable Swiss Army knife designed to make life easier for those of us who spend some of our working hours hacking away at the command prompt.

### Drush Install/Update - Unix/OSX

The easiest way to install the latest release of Drush on Unix/Linux systems is via our custom PEAR channel:
    
    # pear channel-discover pear.drush.org
    # pear install drush/drush

Tip: Use sudo to overcome permission problems. If the channel-discover fails, try running the following sequence of commands:

    # pear upgrade --force Console_Getopt
    # pear upgrade --force pear
    # pear upgrade-all

To update, run pear upgrade drush/drush

To get alternate drush versions, replace that last line with one of the below that matches your fancy.

    $ pear install drush/drush-4.6.0
    $ pear install drush/drush-5.0.0

If you are using Debian or Ubuntu, you can alternatively use the Debian packages uploaded in your distribution. You may need to use the backports to get the latest version, if you are running a LTS or "stable" release.

### Setting up local environment
This document assumes you already have a webserver, php and database server installed and working in your local machine.
If you need help to set up your local environment please refer to [Setting Up Local Environment](#) 

For this document we are going to be using the following.

`Client Domain: thecrazyhorse.com`
`Client Code:   TCH`
`Your projects folder will be mentioned as ~/projects`


Set up your environment as usual, using the client domain as folder name for your project:

    mkdir ~/projects/thecrazyhorse.com/ && cd ~/projects/thecrazyhorse.com

##### Clone Ash Make
Run the following command.

    $ git clone git://github.com/augustash/ash_make.git


##### Rinning Ash Make
With drush fully functional in your local machine navigate to the project folder and run the following command:

    $ drush make ash_make/ash_make.make

You will be prompted with:

    Make new site in the current directory? (y/n):

Just confirm by typing `y` and press enter to continue. Watch for errors while drush download and create your project file/folders structure.

###### IMPORTANT: After run Drush remove the folder ash_make

    rm -rf ash_make

##### Creating Folders & Editing Files 

Inside the project folder,

    ~/projects/thecrazyhorse.com/sites

Create the following folder structure:

    - thecrazyhorse.com
        - files
        - modules
        - themes
            -custom

    mkdir sites/thecrazyhorse.com
    mkdir sites/thecrazyhorse.com/files
    mkdir sites/thecrazyhorse.com/modules
    mkdir sites/thecrazyhorse.com/themes
    mkdir sites/thecrazyhorse.com/themes/custom

Grant RW permissions to the `files` folder running the command:

    chmod 777 sites/thecrazyhorse.com/files

Copy the file default.settings.php from

    sites/default/default.settings.php
to

    sites/thecrazyhorse.com/settings.php

    $ cp -p sites/default/default.settings.php sites/thecrazyhorse.com/settings.php

Copy the default theme from
    
    $ sites/all/themes/contrib/framework

to 

    $ sites/thecrazyhorse.com/themes/custom/framework

    cp -Rp sites/all/themes/contrib/framework sites/thecrazyhorse.com/themes/custom/

Rename the folder framework to the client code eg: `tch`

    mv sites/thecrazyhorse.com/themes/custom/framework/ sites/thecrazyhorse.com/themes/custom/tch

Rename the theme info file to the client code

    framework.info 
to

    tch.info

    $ mv sites/thecrazyhorse.com/themes/custom/tch/framework.info sites/thecrazyhorse.com/themes/custom/tch/tch.info

Edit the file tch.info and change the theme name, remove timestamps, edit version and set `project` with client code

After finished your file should look like this:

    `name = The Crazy Horse Theme`
    `description = Custom theme created by August Ash, Inc. for thecrazyhorse.com`
    core = 7.x
    engine = phptemplate
    stylesheets[all][] = style.css
    
    regions[header]         = Header
    regions[navigation]     = Navigation
    regions[highlighted]    = Highlighted
    regions[help]           = Help
    regions[content]        = Content
    regions[sidebar_first]  = First Sidebar
    regions[sidebar_second] = Second Sidebar
    regions[footer]         = Footer

    `version = "7.x-0.1"`
    core = "7.x"
    `project = "tch"`

Edit template.php and rename all methods that start with framework_ to the client code.

In this example the method framework_html_head_alter()

```php
function framework_html_head_alter(&$head_elements) 
{
    $head_elements['system_meta_content_type']['#attributes'] = array(
        'charset' => 'utf-8'
    );
}
```

Should become tch_html_head_alter() as shown bellow

```php
function tch_html_head_alter(&$head_elements) 
{
    $head_elements['system_meta_content_type']['#attributes'] = array(
        'charset' => 'utf-8'
    );
}
```

Copy and rename the file sites/example.sites.php to sites/sites.php

cp sites/example.sites.php sites/sites.php
Edit file sites/site.php

Add/Edit this to the end of this file
You are going to add to the array your local url as key and client domain as value. The key/value you will be setting the staging environment, notice the convention: `aai[client_code].[server_name].aaidev.net` as key and client domain as value.

    $sites['thecrazyhorse.dev'] = 'thecrazyhorse.com';
    $sites['aaitch.victor.aaidev.net'] = 'thecrazyhorse.com';


##### Finishing Installation

1 - Access your local environment, open your web browser and point it to thecrazyhorse.com, the Drupal installation start up screen will be shown. At this point `select August Ash profile and click save and continue.`

2 - Next screen `click save and continue.`

3 - Enter your local database information and click save and continue.

4 - Installation Form
 
- Site name field - enter the client company name
- Site Email address field - enter changes@augustash.com
- Username field - enter augustash
- Emain Address field - enter changes@augustash.com
- Password - you can use sequoia to create a password or you can use [Strong Password Generator](http://strongpasswordgenerator.com/) website to generate your password.

IMPORTANT: before you enter the password in the Drupal installation form, create a record of it in [Sequoia](https://sequoia.augustash.com).



5 - Un-check all fields under Update notifications and click save and continue.

The installation should finish.

6 - Access the administration area and Enable/add modules depending on the project needs.



### Push Code

Request creation of domain repository to development manager.
Push code.
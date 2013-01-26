# Pirumage is a simple Magento Channel Server Manager

Pirumage is a simple Channel Server Manager compatible with Magento
Connect Manager v1.5.0.0 (Magento CE v1.5.0.0 and up) that lets you set up channel
servers in a matter of minutes. Pirumage is based on Pirum, the simple PEAR
Channel Server Manager. Instead of PEAR, Pirumage is compatible with Magento's
Connect Manager v1.5.0.0 protocol.

Because it's based on Pirum, there is no need for external dependencies, no not
need for a database, no need to setup credentials, and nothing need to be
installed or configured.

## Installation

Installing Pirumage is as simple as downloading the latest stable `pirumage` file
and saving it where you see fit. You can also download a specific version if you
want.

Check that Pirumage works by calling it without any argument:

    $ cd /path/to/pirumage
    $ php pirumage

You should see something similar to the following:

    Pirumage 0.1.0 by Jeroen Schouten. Based on Pirum by Fabien Potencier
    Available commands:
      pirumage build target_dir
      pirumage add target_dir Twm_Pirumage-1.0.0.tgz
      pirumage remove target_dir Twm_Pirumage-1.0.0.tgz

## Usage

### Creating a MAGE Channel Server

Creating a new MAGE channel server is as easy as creating a single XML
configuration file. Below you find an example configuration:

    <?xml version="1.0" encoding="UTF-8" ?>
    <server>
        <name>thewebmen</name>
        <summary>The Webmen Mage channel</summary>
        <url>http://webmen.com/thewebmen/</url>
    </server>

Save the above XML configuration file as `thewebmen/pirumage.xml`.

That's all. You can now build your MAGE channel server by calling the `build` command:

Of course, as we have not added any Magento module packages yet, you will end up
with an empty channel. But nonetheless, you can already register your channel and
see if everything is setup correctly (beside checking that everything work, it is
also needed to create packages for this new channel - see below).

In your Magento installalation folder:

    $ mage channel-add http://thewebmen.com/thewebmen/
    $ mage list-channels

You should see your channel in the list:

    Available channels:
    community: connect20.magentocommerce.com/community
    thewebmen: thewebmen.com/thewebmen

### Adding new Releases

Adding a new release can be done by calling the add command:

    $ php pirumage add thewebmen path/to/Twm_Foo-1.0.0.tgz

where `path/to/Twm_Foo-1.0.0.tgz` is a valid Mage module package for the 1.5.0.0
version of Magento Connect Manager (built for versions of Magento CE v1.5.0.0 and
above). The `add` command also calls the `build` command automatically.

> Make sure the file `path/to/package.xml` exists as this is required by
> Pirumage!

### Removing releases

Removing a release is as simple as removing the appropriate directory, followed
by the `build` command:

    $ rm -rf /path/to/Twm_Foo/1.0.0
    $ php pirumage build path/to/

## Creating a Magento module package

Creating Magento module packages is done through the Admin panel of your Magento
installation.

Go to System - Magento Connect - Package Extensions. Create your extension by
filling in the required fields. Make sure you set 'Channel' to the name of your
channel server (`thewebmen` in the example above). Select '1.5.0.0 & later' as
Supported Releases. When you have filled in all the required field press
'Save Data and Create Package'. Your package will be saved in
`path\to\mage\var\connect`.

## Updating Magento modules to the latest version

In your Magento admin panel, go to System - Magento Connect - Connect Manager.
You should see channel: thewebmen or whatever name you gave it. Below is a list
of installed modules. Click 'Check for updates' to see if there are any updates.
If there are, the option 'Upgrade to x.y.z' will appear next to the module.
Select this option and click 'Commit changes'. You're done!

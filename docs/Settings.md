# Settings.py

By modifying ``sal/settings.py`` you can customise how plugins and data is displayed in Sal. If you are upgrading from a previous version of Sal, refer to this document to see how your ``settings.py`` file should be changed to take advantage of any new features. There are defaults set in ``sal/system_settings.py``, but they can be overridden if you choose.

## Enabling brute force protection

You can add the following to your ``sal/settings.py`` to enable brute force protection. Change ``BRUTE_LIMIT`` to the number of login attempts allowed before the account is locked, ``BRUTE_COOLOFF`` is the time after which any locked accounts will be unlocked. ``BRUTE_PROTECT`` **must** be set to ``True`` to enable the unlocking UI in the user management page.

```python
BRUTE_PROTECT = True
BRUTE_COOLOFF = 3
BRUTE_LIMIT = 3
###############
INSTALLED_APPS+= ('axes',)
MIDDLEWARE_CLASSES+=('axes.middleware.FailedLoginMiddleware',)
# Max number of login attemts within the ``AXES_COOLOFF_TIME``
AXES_LOGIN_FAILURE_LIMIT = BRUTE_LIMIT
AXES_COOLOFF_TIME=BRUTE_COOLOFF
```

## LIMIT_PLUGIN_TO_FRONT_PAGE

These plugins will only be shown on the front page. They will not appear anywhere else.

```python
LIMIT_PLUGIN_TO_FRONT_PAGE = ['Uptime', 'Memory']
```

## HIDE_PLUGIN_FROM_FRONT_PAGE

Once again, a list of plugin names. These will not be shown on the front page.

```python
HIDE_PLUGIN_FROM_FRONT_PAGE = ['DiskSpace']
```

## HIDE_PLUGIN_FROM_BUSINESS_UNIT

Specify which Business Unit IDs should be hidden from which plugins. The data should be a dictionary containing lists. The Business Unit ID will be shown in the URL when on that particular Business Unit's page.

```python
HIDE_PLUGIN_FROM_BUSINESS_UNIT = {
    'Encryption':['1','2','4'],
    'DiskSpace':['5','7','9']
}
```

## HIDE_PLUGIN_FROM_MACHINE_GROUP

Works exactly the same as ``HIDE_PLUGIN_FROM_BUSINESS_UNIT`` (although you are specifying the Machine Group ID, obviously!),

```python
HIDE_PLUGIN_FROM_MACHINE_GROUP = {
    'DiskSpace':['1'],
    'Uptime':['2','8']
}
```

## EXCLUDED_FACTS

These Facts won't be displayed on the Machine Information page. This won't effect any plugins that rely on the Fact.

```python
EXCLUDED_FACTS = {
    'sshrsakey',
    'sshfp_rsa',
    'sshfp_dsa',
    'sshdsakey',
}
```

## EXCLUDED_CONDTIONS

The same as ``EXCLUDED_FACTS``, but will hide Munki Conditions instead.

```python
EXCLUDED_CONDTIONS = {
    'ipv4_address',
}
```

## ADD_NEW_MACHINES

By default, machines that don't exist in Sal, but have a valid Machine Group Key will be created. If you are using Sal for inventory purposes (for example, signing Puppet Certificates), you may wish to disable this.

``` python
ADD_NEW_MACHINES = False
```

## DEFAULT_MACHINE_GROUP_KEY

By default, all machine submissions must include a machine group key otherwise an error will occur. By defining this value to an existing machine group key then machines without a group key already defined in its preferences will be placed into this group. This can be used, for example, to determine which machines have not been setup properly with the correct machine group.

```python
DEFAULT_MACHINE_GROUP_KEY = 'x1eru38unri08badpo0ux4ahz043hapbyqyixdz482l047u9xe60nn6cux1sj0ad5bq7hwblyzjpmaqb17psygfwlfeo4x6hozb1jejaf1nee6paj68glducdt5575dz'
```

## HISTORICAL_FACTS

Normally only the most recent fact is recorded for a machine. Any facts defined here will also have historical data from each run kept in addition to the most recent run.

```python
HISTORICAL_FACTS = [
    'memoryfree_mb',
]
```

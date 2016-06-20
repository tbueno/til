# Elastik Beanstalk intallation with `pip` fails on El Capitan

According to [AWS EB CLI Documentation](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html?icmpid=docs_elasticbeanstalk_console), it is just a matter of running the following command

    pip install awsebcli

But that on El Capitan will trigger a bunch of `not permitted errors`:

    [('/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib/__init__.py', '/var/folders/nt/wk14fc6s37b1gzl8q7td6dgc0000gp/T/pip-OXBkDM-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib/__init__.py', "[Errno 1] Operation not permitted: '/var/folders/nt/wk14fc6s37b1gzl8q7td6dgc0000gp/T/pip-OXBkDM-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib/__init__.py'"), ('/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib/__init__.pyc', '/var/folders/nt/wk14fc6s37b1gzl8q7td6dgc0000gp/T/pip-OXBkDM-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib/__init__.pyc', "[Errno 1] Operation not permitted: '/var/folders/nt/wk14fc6s37b1gzl8q7td6dgc0000gp/T/pip-OXBkDM-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib/__init__.pyc'"), ('/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib/markers.py', '/var/folders/nt/wk14fc6s37b1gzl8q7td6dgc0000gp/T/pip-OXBkDM-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib/markers.py', "[Errno 1] Operation not permitted: '/var/folders/nt/wk14fc6s37b1gzl8q7td6dgc0000gp/T/pip-OXBkDM-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib/markers.py'"), ('/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib/markers.pyc', '/var/folders/nt/wk14fc6s37b1gzl8q7td6dgc0000gp/T/pip-OXBkDM-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib/markers.pyc', "[Errno 1] Operation not permitted: '/var/folders/nt/wk14fc6s37b1gzl8q7td6dgc0000gp/T/pip-OXBkDM-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib/markers.pyc'"), ('/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib', '/var/folders/nt/wk14fc6s37b1gzl8q7td6dgc0000gp/T/pip-OXBkDM-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib', "[Errno 1] Operation not permitted: '/var/folders/nt/wk14fc6s37b1gzl8q7td6dgc0000gp/T/pip-OXBkDM-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib'")]

It turns out that El Capitan comes with a set of libraries already installed, and when trying to install something with `pip` it will try to update this libraries that belong to the System and not to the user of the session.

If upgrade dependencies is not a requirement for the library to be installed (it is usually not), a special flag can be used to avoid the error.

    pip install --ignore-installed awsebcli



## More info

- [EB CLI Installation](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html?icmpid=docs_elasticbeanstalk_console)

AWS Elastic Beanstalk Deployment Notifications for New Relic
============================================================

This script can be executed as part of the [container commands directive](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/customize-containers-ec2.html#customize-containers-format-files) supported by [Elastic Beanstalk extensions](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/customize-containers-ec2.html) to send a deployment notification to [New Relic](http://www.newrelic.com/) via the HTTP API service.

Installation
------------

1. [Download](https://raw.github.com/apancutt/aws-eb-newrelic-deploynotify/master/aws-eb-newrelic-deploynotify.sh) the bash script from this project into your application.

        cd /path/to/your/app
        mkdir bin
        wget -P bin "https://raw.github.com/apancutt/aws-eb-newrelic-deploynotify/master/aws-eb-newrelic-deploynotify.sh"
        chmod +x bin/aws-eb-newrelic-deploynotify.sh

2. Create an `.ebextensions` directory in your application root:

        mkdir .ebextensions

3. Create a new file (or append to an existing one) for the `container_commands` configuration:

        echo "00_aws-eb-newrelic-deploynotify:" >> .ebextensions/03_container_commands.config
        echo "  command: \"bin/aws-eb-newrelic-deploynotify.sh -a <APP NAME> -k <API KEY>\"" >> .ebextensions/03_container_commands.config

    Note: If you already have a file for container commands, simply append the following lines:

        00_aws-eb-newrelic-deploynotify:
          command: "bin/aws-eb-newrelic-deploynotify.sh -a <APP NAME> -k <API KEY>"

    Don't forget to replace the arguments with the correct values.

4. Deploy to Elastic Beanstalk and check New Relic for a deployment notification (Applications > Your App > Events > Deployments).

Usage
-----

The installation steps above describe the minimal effort required to get this script working, but you may wish to make use of these extra options.


* `-a` The name your the application in Elastic Beanstalk.
* `-d` The name of the deployer (default: `AWS Elastic Beanstalk`).
* `-e` Error if the HTTP request fails. Note that this will abort the deployment.
* `-h` Displays this help message.
* `-k` Your New Relic API key.
* `-q` Quiet mode.
* `-v` Display version information.

Caveats
-------

* This script has only been tested with PHP applications. Please help improve this script by submitting pull requests for compatibility with alternative environments.
* The detection of the current application version is based on an undocumented feature of Elastic Beanstalk's deployment process which is subject to change without notice. The accuracy of the current version is not guaranteed.

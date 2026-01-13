---
title: PHPUnit Tests
date: 2026-01-16T22:54:16.700Z
author: Ryan Robinson
description: "Drupal offers a few types of phpunit regression testing suites, and those tests can also be run in CI/CD and in local development."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - PHP
draft: true
---

This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

This post is a bit bigger than just that series, though. When you're maintaining a site over an extended period of time, something extremely useful is to have automated regression testing that will catch if you break one thing in the process of fixing another thing. I'm only adding it as part of that series because of the role of the automated testing stages.

I've also heard the suggestion that it is best to write the tests before you even write the live code. That way, as you work on the code, you can have the automated tests confirming what you have solved and what you haven't. I have not gotten very close to that point yet, and there are probably some scenarios where it isn't practical because you don't know the exact outcome you want until you start seeing it in practice. It does help demonstrate the real ideal, though, with constant verification that everything is still doing what it should before it gets merged and eventually sent to production.

## Types of Tests in Drupal

In Drupal, there are four built-in test suites, in escalating complexity where the faster ones are also the least flexible:

1. Unit tests: these are bare bones PHP. If the function you're testing is very simple, this might be adequate and would be very fast.
2. Kernel tests: this adds some more core Drupal functionality, making it much easier to invoke services or install other modules.
3. Functional tests: this adds the ability to walk through a workflow as a mock user and confirm certain behaviours. This is great for things like testing that permissions are having the effect you expect.
4. Functional with JavaScript tests: this is similar to the above, but with JavaScript.

I have also installed Drupal Test Traits which is not included in core, but haven't gotten much real use for it yet. I think the biggest advantage of it is when you want your tests to include some realistic sample content.

## Running Tests in CI/CD

Running all the tests in certain conditions upon committing or merging to your repository is the best way to be sure that you don't push through a regression. To do that, you'll need an image that can run the tests, and the GitLab CI file to define in what circumstances to run that job.

TODO

## Running Tests Locally

Of course, running them in CI/CD is a nice failsafe, but I also should be running tests in my local developer environment to confirm it works BEFORE I commit things thinking that I am done.

Running the unit tests and the kernel tests came together pretty easily to run locally.

Running the functional tests got more complicated, because my local developer environment is running its command line as root instead of the www-data that is used for the Apache hosting the site. This took forcing it to run tests as www-data. I already had a postCreateCommand.sh script in my project that would run when starting up the VS Code developer environment, so it made the most sense to put it in here:

```bash
# phpunit functional tests need to run as www-data
echo -e '\nphpunit() {\n    su -s /bin/bash -c "vendor/bin/phpunit $@" www-data\n}' >> /root/.bashrc
source /root/.bashrc
mkdir -p web/sites/simpletest && chmod 777 web/sites/simpletest
```

## VS Code Extensions

I already had the excellent general PHP extension installed. In those documents, it says that it will automatically list and allow running the tests from the VS Code Tests Explorer panel. I could never get that to work. Maybe it's a weird bit of configuration I missed. Maybe it's a conflict with some other extension. Maybe it just doesn't work as well as the developers thought it would.

I then added another extension to help, PHPUnit Test Explorer. This is only for the purpose of integrating tests to Tests Explorer. It works well. I can see all tests and run all tests. I can even run the tests while generating a coverage report, and the coverage information will then be displayed elsewhere in VS Code to help see what has coverage and what doesn't, although I have found that it isn't entirely consistent in how often those results show up and on which folders.

You can also use the VS Code Tests Explorer to set some tests to run continuously. If you are working on something which already has tests, you can leave those tests running continuously while you work and can then know very quickly if something stops working.

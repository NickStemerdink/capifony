# Capifony

Capistrano is a utility and framework for executing commands in parallel on multiple remote machines, via SSH.
Capifony is a batch of Capistrano reciepts, writed for symfony framework.

## DEPENDENCIES

* Capistrano (http://github.com/capistrano/capistrano/tree/master);
* git (http://git-scm.com/) on development & production. For now, only git supported, working on SVN integration;
* symfony framework 1.2+, installed on development & production servers;
* SSH connection to production server.

## WORKFLOW

![Diagram](http://everzet.com/images/capifony.png)

* There is your symfony project on development server (your PC/Notebook), that stored inside git repository;
* There is production server. Your VPS or server with connection over SSH. Main git repository stored on it and production version of project is pulled from it.
* You make changes in your development project, commit them and push to the remote repository on the production server. 'cap deploy' task connects to the production server over SSH and pull all your changes from remote repository to remote project.

## USAGE

In general, you'll use Capifony as follows:

1. Go into new project directory on your development server/machine, generate symfony project;

2. Run "capify .";

3. Download or clone capifony, replace "Capfile" in symfony project root directory with downloaded one & place symfony.rb in config subdirectory;

4. Edit config/deploy.rb to match this:

  set :application,   "example-app"
  set :repository,    "/path/to/your/production/repos/"
  set :releases_path, "/path/to/your/www/"
  set :host,          "hostname"

  set :db_orm,        "Propel"
  set :db_dsn,        "mysql:host=localhost;dbname=example-app"
  set :db_user,       "root"
  set :db_pass,       "$secr3t"

5. Run "cap git:init". This will generates ".gitignore" file & inits empty repository in project directory. ".gitignore" by defaults ignoring "config/ProjectConfiguration.class.php" & "config/databases.yml", because them is server dependent. You can autogenerate them later on production by calling "cap symfony:setup_remote" & "cap db:setup_remote";

6. Run "git add . && git commit -am 'initial commit'". You now done initial commit;

7. Run "cap symfony:test_remote" task. It will download "check_configuration.php" to your production server, runs it, removes it from production & show it's output;

8. If everything in previous step goes fine, run "cap deploy:setup" task. This task is wrapper to call "git:setup_remote", "symfony:setup_remote", "db:setup_remote" tasks.
  * "cap git:setup_remote" creates 'bare' repository on production server, pushes local commits into it, and clones production project from it;
  * "cap symfony:setup_remote" generates "config/ProjectConfiguration.class.php" with right path for symfony installation;
  * "cap db:setup_remote" runs "symfony configure:database" with data from "deploy.rb".

9. You're done. Now you can:
  * Make changes
  * Commit them
  * Push them to production repository
  * "cap deploy" to deploy them on production project via production repository

## LICENSE:

(The MIT License)

Copyright (c) 2009 Konstantin Kudryashov <ever.zet@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

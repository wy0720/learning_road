# composer的使用

我常常在使用composer的时候不知道这里面到底有些什么东西可以操作，他的水有多深，于是乎，每次碰到不知道的操作都开始了漫长的搜索之旅，这事干多了发现真的效率很低。。。。

回归正题！

首先我们在使用composer时候无非有两种方式：

* 使用composer下的命令，例如config等
* 使用参数

那么如何来自自己查找呢

* 使用composer下的命令，例如config等	直接在命令行输入composer回车即可，在详细信息中会显示出Available commands，这下面就是可以用的命令，使用方式就是输入composer + Available commands中的其中一两个命令了	

  ```
  C:\Users\wy220>composer
     ______
    / ____/___  ____ ___  ____  ____  ________  _____
   / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
  / /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
  \____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                      /_/
  Composer version 1.6.4 2018-04-13 12:04:24

  Usage:
    command [options] [arguments]

  Options:
    -h, --help                     Display this help message
    -q, --quiet                    Do not output any message
    -V, --version                  Display this application version
        --ansi                     Force ANSI output
        --no-ansi                  Disable ANSI output
    -n, --no-interaction           Do not ask any interactive question
        --profile                  Display timing and memory usage information
        --no-plugins               Whether to disable plugins.
    -d, --working-dir=WORKING-DIR  If specified, use the given directory as working directory.
    -v|vv|vvv, --verbose           Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

  Available commands:
    about                Shows the short information about Composer.
    archive              Creates an archive of this composer package.
    browse               Opens the package's repository URL or homepage in your browser.
    check-platform-reqs  Check that platform requirements are satisfied.
    clear-cache          Clears composer's internal package cache.
    clearcache           Clears composer's internal package cache.
    config               Sets config options.
    create-project       Creates new project from a package into given directory.
    depends              Shows which packages cause the given package to be installed.
    diagnose             Diagnoses the system to identify common errors.
    dump-autoload        Dumps the autoloader.
    dumpautoload         Dumps the autoloader.
    exec                 Executes a vendored binary/script.
    global               Allows running commands in the global composer dir ($COMPOSER_HOME).
    help                 Displays help for a command
    home                 Opens the package's repository URL or homepage in your browser.
    info                 Shows information about packages.
    init                 Creates a basic composer.json file in current directory.
    install              Installs the project dependencies from the composer.lock file if present, or falls back on the composer.json.
    licenses             Shows information about licenses of dependencies.
    list                 Lists commands
    outdated             Shows a list of installed packages that have updates available, including their latest version.
    prohibits            Shows which packages prevent the given package from being installed.
    remove               Removes a package from the require or require-dev.
    require              Adds required packages to your composer.json and installs them.
    run-script           Runs the scripts defined in composer.json.
    search               Searches for packages.
    self-update          Updates composer.phar to the latest version.
    selfupdate           Updates composer.phar to the latest version.
    show                 Shows information about packages.
    status               Shows a list of locally modified packages, for packages installed from source.
    suggests             Shows package suggestions.
    update               Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
    upgrade              Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
    validate             Validates a composer.json and composer.lock.
    why                  Shows which packages cause the given package to be installed.
    why-not              Shows which packages prevent the given package from being installed.
  ```

* 如何查看可以使用什么参数呢？

  ```
  在每个命令后加上 -h即可
  ```

  ​
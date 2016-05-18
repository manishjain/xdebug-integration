Steps to setup xdebug profiler in mac-

Generating Xdebug profile Data:

* Install Xcode in your Mac OSX from App store (If not already)
* Install xdebug: 
sudo pecl install xdebug
* locate xdebug.so
* Now open your php.ini file (to locate it php -i | grep "php.ini")
* set the path of xdebug.so in the end of php.ini   
e.g. zend_extension=/usr/lib/php/extensions/no-debug-non-zts-20121212/xdebug.so 
* create a folder in home as xdebug-profiler with write permissions to all.  It will have all xdebug profile generated:
mkdir /Users/<u-name>/xdebug-profiler 
sudo chmod go+w /Users/<u-name>/xdebug-profiler
* insert the following configuration of xdebug in php.ini.  
(%R will add url and %t will add timestamp in profile data file generated. 
 xdebug.remote_enable=1 is for permanent enable
xdebug.profiler_enable_trigger = 1 is specific enable for adding  param  XDEBUG_PROFILE=1)

[xdebug] 
xdebug.remote_enable=0 
xdebug.remote_host=localhost 
xdebug.remote_port=9000 
xdebug.remote_handler="dbgp" 
xdebug.profiler_enable = 1 
xdebug.profiler_output_dir = /Users/<u-name>/xdebug-profiler 
xdebug.profiler_output_name = "callgrind.%R.%t" 
xdebug.profiler_enable_trigger = 1
* Restart apache: 
sudo apachectl restart
* Request a page by adding XDEBUG_PROFILE=1 in your site url 
* A new file having profile data will be added in xdebug-profiler folder

Visualise Profile Data:

* install qt by homebrew: brew install qt 
* If it gives Error: The `brew link` step did not complete successfully, 
It means some folders in /usr/local required here are not writable. Then do:  
brew uninstall qt  
change owner of /usr/local/<folder-name> to youself: 
sudo chown -R $USER /usr/local/<folder-name> 
install brew again: 
brew install qt
* Run `brew linkapps qt` to symlink these to /Applications
* verify qmake installation, it will give you path where it is installed: 
which qmake
* Install qcachegrand (A tool to visualize profile data) 
brew install qcachegrind 
Install graphviz (qcachegrind uses graphviz's dot application to generate the graphs.) brew install graphviz
* Link graphviz
           brew link graphviz
* To open profile window of file generated in step (10) of first part run in xdebug-profiler folder: 
qcachegrind <callgrind file name>
e.g. qcachegrind callgrind._Escrow_Myoffers_XDEBUG_PROFILE\=1.1462598894
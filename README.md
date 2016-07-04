## Dependency

### scraper

Install selenium, run `brew install selenium-server-standalone`, and chromeDriver.

### content engine

Install python library dependency in virtual environment using "conda", and redis by `brew install redis`.

## Logs

### scraper

Check [this post](http://docs.python-guide.org/en/latest/scenarios/scrape/) for basic python web scraping, but is is only fetching the static html page, without rending javascript part of code, since some content of the page is generated by javascript, we want to simulate a browser environment to get an complete html page for scraping.(selenium?)

From [this post](http://stackoverflow.com/questions/29449982/installing-pyqt4-with-brew), we can install `PyQt4` by doing `brew install PyQt4 --with-python27`.

The next question would be how to simulate the "load more" event? Only sliding the page to the end can trigger the load more action.

- One option is to use "selenium" to simulate the infinite scrolling event. Since we will be using chromedriver, use `brew install chromedriver` to install. One of the handy thing about selenium is that it allows us to execute javascript script on the selected elements. Check [this post](http://stackoverflow.com/questions/21006940/how-to-load-all-entries-in-an-infinite-scroll-at-once-to-parse-the-html-in-pytho) for more information of how to simulate infinite scroll.(Thus, basically with selenium we don't have to use pyqt4 webkit.)

- The other one is to simple inspect the difference url being used when scrolling happens, find the pattern. But in the wechat website exmple, this trick doesn't work.

After scraping all required static html content as string, we can do regex `findall` to find all matching urls from those three panels. Then save it to csv file. Then hand it to next step of fetching, note that the urls here still contains symbols like "&amp;", need to remove them before calling.

### content engine

List all python dependencies in `conda.txt`, then run `conda create -n <virtual env's name> --file conda.txt` to create a new environment based on the library from `conda.txt`. Then the following things will be just get into that environment and get out by using: `source activate <env's name>` and `source deactivate`

Then, need to install redis on mac to be tested in local environment. Use `ps aux | grep redis` to check if redis server is running. If it's not running, use the following command `nohup redis-server &` to start a redis-server process and let it run in background.

## TODOs

After collecting all post url into csv file, we trace up to its pointing article page and scrape for the first three paragraphs(in case of not choking redis for too much content?), then use google translate to make it in English and do TF-IDF training. 


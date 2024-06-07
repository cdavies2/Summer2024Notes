# curl Command In Linux
* curl is a command-line tool and library used to transfer data with URLs.
* curl supports a wide range of protocols (including HTTP, HTTPS and FTP), so it is incredibly valuable for fetching, uploading, and managing data over the Internet.
* The syntax for curl is...
    * curl [options] [URL]
    * [options] are the command-line flags that modify the behavior of curl
    * [URL] specifies the location from which to fetch or send data

  ## Fetching Data Using curl Command
  * To fetch a web page using 'curl', you simply provide the URL as an argument
      * curl https://example.com
      * The command retreives the HTML content of the URL and displays it in the terminal
  * URLs with numeric sequence series can be written as:
      * curl ftp://ftp.example.com/file[1-20].jpeg
  * curl displays a progress meter to indicate transfer rate, time left, and amount of   data transferred. You can put -# to change the meter to a progress bar and -silent to disable it completely.
 
## Handling HTTP Requests Using curl Command
* Curl allows you to send custom HTTP requests
  1. curl -X GET https://api.example.com/resource
  2. curl -X POST -d "key1=value1&key2=value2" https://api.example.com/resource
     * the -d flag is used to specify data to be sent with the request

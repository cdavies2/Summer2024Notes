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

## Downloading Files Using curl Command
* To download a file, simply provide the URL of the file as an argument.
* -o saves the downloaded file on the local machine with the name provided in the parameters.
   * curl -o hello.zip ftp://speedtest.tele2.net/1MB.zip

## Uploading Files
* curl can be used to upload files to a server. It can be useful for automating file uploads or transferring files via FTP from the command line.
* The -T tag is used to upload a file with FTP
   * curl -T uploadfile.txt ftp://example.com/upload

## Handling Authentication
* You can specify authentication credentials using the '-u' tag
   * curl -u username:password https://example.com/api

## Examples of Curl Command
* -C - Option: resumes download which has been stopped
   * curl -C -o ftp://speedtest.tele2.net/1MB.zip
* -limit-rate Option: limits the upper bound of the rate of data transfer and keeps it around the given value in bytes
   * curl --limit-rate 1000K -O ftp://speedtest.tele2.net/1MB.zip
* -u Option: gives option to download files from user authenticated FTP servers
* -T Option: helps to download a file to the FTP server
* -libcurl Option: If this option is appended to any cURL command, it outputs the C source code that uses libcurl for the specified option. It is a code similar to the command line implementation.
   * Syntax: curl [URL...] --libcurl [filename]
   *  curl https://www.geeksforgeeks.org > log.html --libcurl code.c
   *  The above example downloads HTML and saves it into log.html and the code in the code.c file
* Sending Mail: because curl can transfer data over different protocols, including SMTP, we can use curl to send mails.
   * curl –url [SMTP URL] –mail-from [sender_mail] –mail-rcpt [receiver_mail] -n –ssl-reqd -u {email}:{password} -T [Mail text file]
 * DICT Protocol: can be used to easily get the definition or meaning of any word directly from the command line.
    * Syntax: curl [protocol: [dictionary_URL]:[word]
    * Ex: curl dict://dict.org/d:overclock 

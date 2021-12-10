# mailR 0.8.1 - 10 December 2021
#### What's new in version 0.8.1
- Updates to Readme
- Created News file

# mailR 0.8 - 5th November 2021
#### What's new in version 0.8
##### **Fixes **
- Added activation.jar dependency
- Switched from deprecated function setSmtpPort to setSSLOnConnect when using SSL
- Switched from deprecated function setTLS to setStartTLSEnabled when using SSL
- Internal function .valid.email() actually works now!

# mailR 0.7 - 24th August 2021
#### What's new in version 0.7 (Thank you, [sclewis23](https://github.com/sclewis23))
##### **Updates**
- Upgraded Commons Email Jar to version 1.5
- Upgraded Javax.mail Jar to version 1.6.2

# mailR 0.6 - 18th January 2016
#### What's new in version 0.6
##### **Enhancements**
- Refinement to the stripped down text version of HTML emails for incompatible clients (thanks to @dtenenba)
- Ability to add email headers by passing a named list `headers` (thanks to @dtenenba)

# mailR 0.5 - 6th December 2015
#### What's new in version 0.5
##### **Enhancements**
- Better handling of errors thrown by Java (thanks to @chlorenz)
- Email clients unable to view HTML emails now receive stripped down version of message (Fixes #24)

# mailR 0.4 - 30th December 2014
#### What's new in version 0.4
##### **Features**
- Attach files to the email using URLs, e.g., you can send files from your Dropbox public folder using the URL.
- A 'debug' parameter to set that will make send.mail() provide a detailed log.
- Option to set a email address to reply to using the 'replyTo' parameter.

#####  **Enhancement**
- Upgraded Commons Email Jar to version 1.3.3
- Upgraded Javax.mail Jar to version 1.5.2

# mailR 0.3.1 - 08th September 2014
#### What's new in version 0.3.1
##### **Enhancement**
- Better resolution of paths to allow attaching files from locations other than the working directory.
- Updated documentation to give example of use on MS Exchange

# mailR 0.3 - 12th May 2014
#### What's new in version 0.3
##### **Features**
- Added support to encode emails using iso-8859-1, utf-8, us-ascii, and koi8-r character sets.
- The body parameter can point to a locally stored text (or HTML) file and mailR will parse its contents to create the body of the email.

##### **Bug fixes**
- Experimental: changed called methods to set SSL/TLS to true to check whether it resolves issue that causes port number to default to 465.

# mailR 0.2 - 20th April 2014
#### What's new in version 0.2
##### **Features**
- mailR now allows sending email content as HTML including allowing for embedding images as inline (currently an experimental feature).
- Email addresses conforming to RFC 2822 allowed, e.g., "FirstName LastName <sender@domain.com>" allowed.
- A java stacktrace is printed out in case of failure when sending the email to allow better root cause analysis.

##### **Bug fixes**
- Fixed a bug that incorrectly set the TLS parameter as TRUE whenever the SSL parameter was set as TRUE.

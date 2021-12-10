[![CRAN\_Status\_Badge](https://www.r-pkg.org/badges/version/mailR)](http://cran.r-project.org/web/packages/mailR)
[![Downloads](https://cranlogs.r-pkg.org/badges/mailR)](http://cran.rstudio.com/package=mailR)

Overview
========
mailR allows users to send emails from R.

It is developed as a wrapper around [Apache Commons Email](http://commons.apache.org/proper/commons-email/) and offers several features to send emails from R such as:
- using authentication-based SMTP servers
- sending emails to multiple recipients (including the use of Cc, Bcc, and ReplyTo recipients)
- attaching multiple files from the file system or from URLs
- sending HTML formatted emails with inline images

Note: This is the new home for mailR. The previous repository [https://github.com/rpremraj/mailR](https://github.com/rpremraj/mailR) is discontinued.


Installation instructions
=========================
You can install the latest development version of mailR using devtools:

```R
install.packages("devtools", dep = T)
library(devtools)
install_github("rpremrajGit/mailR")

library(mailR)
```

The latest release of mailR is available on [CRAN](http://cran.r-project.org/web/packages/mailR/):

```R
install.packages("mailR", dep = T)

library(mailR)
```

Usage
=====
To send an email via a SMTP server that does not require authentication:

```R
send.mail(from = "sender@gmail.com",
          to = c("Recipient 1 <recipient1@gmail.com>", "recipient2@gmail.com"),
          cc = c("CC Recipient <cc.recipient@gmail.com>"),
          bcc = c("BCC Recipient <bcc.recipient@gmail.com>"),
          subject = "Subject of the email",
          body = "Body of the email",
          smtp = list(host.name = "aspmx.l.google.com", port = 25),
          authenticate = FALSE,
          send = TRUE)
```

*Note that aspmx.l.google.com works for gmail recipients only. Check your gmail spam folder if using this server.*

To send an email via a SMTP server that requires authentication:

```R
send.mail(from = "sender@gmail.com",
          to = c("recipient1@gmail.com", "Recipient 2 <recipient2@gmail.com>"),
          replyTo = c("Reply to someone else <someone.else@gmail.com>")
          subject = "Subject of the email",
          body = "Body of the email",
          smtp = list(host.name = "smtp.gmail.com", port = 465, user.name = "gmail_username", passwd = "password", ssl = TRUE),
          authenticate = TRUE,
          send = TRUE)
```

To send an email with utf-8 or other encoding:

```R
email <- send.mail(from = "Sender Name <sender@gmail.com>",
                   to = "recipient@gmail.com",
                   subject = "A quote from Gandhi",
                   body = "In Hindi :  थोडा सा अभ्यास बहुत सारे उपदेशों से बेहतर है।
                   English translation: An ounce of practice is worth more than tons of preaching.",
                   encoding = "utf-8",
                   smtp = list(host.name = "smtp.gmail.com", port = 465, user.name = "gmail_username", passwd = "password", ssl = T),
  			   authenticate = TRUE,
				   send = TRUE)
```           
           
To send an email with one or more file attachments, and set the debug parameter to see a detailed log message:

```R
send.mail(from = "sender@gmail.com",
          to = c("recipient1@gmail.com", "recipient2@gmail.com"),
          subject = "Subject of the email",
          body = "Body of the email",
          smtp = list(host.name = "smtp.gmail.com", port = 465, user.name = "gmail_username", passwd = "password", ssl = TRUE),
          authenticate = TRUE,
          send = TRUE,
          attach.files = c("./download.log", "upload.log", "https://dl.dropboxusercontent.com/u/5031586/How%20to%20use%20the%20Public%20folder.rtf"),
          file.names = c("Download log.log", "Upload log.log", "DropBox File.rtf"), # optional parameter
          file.descriptions = c("Description for download log", "Description for upload log", "DropBox File"), # optional parameter
          debug = TRUE)
```

To send a HTML formatted email:

```R
send.mail(from = "sender@gmail.com",
          to = c("recipient1@gmail.com", "recipient2@gmail.com"),
          subject = "Subject of the email",
          body = "<html>The apache logo - <img src=\"http://www.apache.org/images/asf_logo_wide.gif\"></html>", # can also point to local file (see next example)
          html = TRUE,
          smtp = list(host.name = "smtp.gmail.com", port = 465, user.name = "gmail_username", passwd = "password", ssl = TRUE),
          authenticate = TRUE,
          send = TRUE)
```

To send a HTML formatted email with embedded inline images:

```R
send.mail(from = "sender@gmail.com",
          to = c("recipient1@gmail.com", "recipient2@gmail.com"),
          subject = "Subject of the email",
          body = "path.to.local.html.file",
          html = TRUE,
          inline = TRUE,
          smtp = list(host.name = "smtp.gmail.com", port = 465, user.name = "gmail_username", passwd = "password", ssl = TRUE),
          authenticate = TRUE,
          send = TRUE)
```
*send.mail expects the images in the HTML file to be referenced relative to the current working directory (something to improve upon in the future).*

You can add headers to your emails by passing a named list called `headers`. Some mail clients allow defining rules based on email headers, where such a feature can come in handy.

```R
send.mail(from = "sender@gmail.com",
          to = c("Recipient 1 <recipient1@gmail.com>", "recipient2@gmail.com"),
          subject = "Subject of the email",
          body = "Body of the email",
          smtp = list(host.name = "aspmx.l.google.com", port = 25),
          headers = list("X-Department" = "Finance", "X-Source" = "Automated report"),
          authenticate = FALSE,
          send = TRUE)
```

This will result in additional headers added to your email as below:
```
X-Department: Finance
X-Source: Automated report
```

MS Exchange server
==================
Provided you have the correct SMTP settings, mailR plays well with MS Exchange. Two mailR users confirmed being able to send emails via Exchange using the following code. I also successfully use mailR at work connecting to MS Exchange.
```R
send.mail(from = from,
          to = to,
          subject = subject,
          body = msg, 
          authenticate = TRUE,
          smtp = list(host.name = "smtp.office365.com", port = 587,
                      user.name = "xxx@domain.com", passwd = "xxx", tls = TRUE))
```

Sending HTML files compiled using Markdown
==========================================
mailR does not currently support resolving inline images encoded using the [data URI scheme](http://en.wikipedia.org/wiki/Data_URI_scheme). Use the workaround below instead:

First off, create the HTML file from the R terminal (the important thing here is that options does not include "base64_images" --- see `?markdown::markdownHTMLOptions`):
```R
library(knitr)
knit2html("my_report.Rmd", options = "")
```

Now you can send the resulting HTML file via mailR:
```R
send.mail(from = "sender@gmail.com",
          to = c("recipient1@gmail.com", "recipient2@gmail.com"),
          subject = "HTML file generated using Markdown",
          body = "my_report.html",
          html = TRUE,
          inline = TRUE,
          smtp = list(host.name = "smtp.gmail.com", port = 465, user.name = "gmail_username", passwd = "password", ssl = TRUE),
          authenticate = TRUE,
          send = TRUE)
```

mailR equivalent of "Blue Screen of Death"
==========================================
Many folks have run into this error using mailR: `Error in ls(envir = envir, all.names = private)`.

The source of the problem most likely lies in the connection to the SMTP server. This could either be due to incorrect settings (someone confirmed trying several servers at work until finally discovering the one playing well with mailR) or be a proxy issue. Turn on the `debug` parameter to get more pointers to resolving your problem.

While I will do my best to support you, there is little I can do remotely if you fall into this trap.

Issues/Contibutions
===================
Please post any issues encountered using mailR to the [issue tracker](https://github.com/rpremrajGit/mailR/issues).

If you would like to submit a patch to improve mailR, please send a pull request to the *develop branch*.

